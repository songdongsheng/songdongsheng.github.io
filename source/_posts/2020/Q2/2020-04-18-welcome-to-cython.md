---
title: Welcome to Cython
description: Welcome to Cython
date: 2020-04-18 14:52:15
tags:
  - Programming
  - Python
categories: [Programming, Python]
permalink: welcome-to-cython
---

# Welcome to Cython

## About Cython

[Cython](https://cython.org/) is an optimising static compiler for both the Python programming language and the extended Cython programming language (based on Pyrex). It makes writing C extensions for Python as easy as Python itself.

The Cython language is a superset of the Python language that additionally supports calling C functions and declaring C types on variables and class attributes. This allows the compiler to generate very efficient C code from Cython code. The C code is generated once and then compiles with all major C/C++ compilers in all later CPython versions.

## Installing Cython

```shell
pip install Cython
```

## Building Cython code

Cython code must, unlike Python, be compiled. This happens in two stages:

- A .pyx file is compiled by Cython to a .c file, containing the code of a Python extension module.
- The .c file is compiled by a C compiler to a .so file (or .pyd on Windows) which can be import-ed directly into a Python session. [setuptools](https://setuptools.readthedocs.io/) takes care of this part. Although Cython can call them for you in certain cases.

There are several ways to build Cython code:

- Write a setuptools setup.py. This is the normal and recommended way.
- Run the cython command-line utility manually to produce the .c file from the .pyx file, then manually compiling the .c file into a shared object library or DLL suitable for import from Python. (These manual steps are mostly for debugging and experimentation.)

### Compiling with the cythonize command

Run the **cythonize** compiler command with your options and list of .pyx files to generate an extension module. For example:

```shell
$ cythonize -a -i yourmod.pyx
```

This creates a **yourmod.c** file (or **yourmod.cpp** in C++ mode), compiles it, and puts the resulting extension module (**.so** or **.pyd**, depending on your platform) next to the source file for direct import (**-i** builds “in place”). The **-a** switch additionally produces an annotated html file of the source code.

The **cythonize** command accepts multiple source files and glob patterns like __\*\*/\*.pyx__ as argument and also understands the common **-j** option for running multiple parallel build jobs. When called without further options, it will only translate the source files to **.c** or **.cpp** files. Pass the **-h** flag for a complete list of supported options.

### Building a Cython module using setuptools

Imagine a simple “hello world” script in a file hello.pyx:

```python
def say_hello_to(name):
    print("Hello %s!" % name)
```

The following could be a corresponding setup.py script:

```python
from setuptools import setup
from Cython.Build import cythonize

setup(
    name='Hello World Application',
    ext_modules=cythonize("src/*.pyx"),
    version="1.0.0",
)
```

To build, run **python setup.py build_ext --inplace**. Then simply start a Python session and do **from hello import say\_hello\_to** and use the imported function as you see fit.

## Using C libraries

Apart from writing fast code, one of the main use cases of Cython is to call external C libraries from Python code. As Cython code compiles down to C code itself, it is actually trivial to call C functions directly in the code.

### Defining external declarations

The C API of the queue implementation, which is defined in the header file **c-algorithms/src/queue.h**, essentially looks like this:

```c
/* queue.h */

typedef struct _Queue Queue;
typedef void *QueueValue;

Queue *queue_new(void);
void queue_free(Queue *queue);

int queue_push_head(Queue *queue, QueueValue data);
QueueValue queue_pop_head(Queue *queue);
QueueValue queue_peek_head(Queue *queue);

int queue_push_tail(Queue *queue, QueueValue data);
QueueValue queue_pop_tail(Queue *queue);
QueueValue queue_peek_tail(Queue *queue);

int queue_is_empty(Queue *queue);
```

The first step is to redefine the C API in a .pxd file

```python
#cython: language_level=3
#libuv.pyd

# https://github.com/fragglet/c-algorithms/blob/master/src/queue.h
# https://github.com/fragglet/c-algorithms/blob/master/src/queue.c
cdef extern from "c-algorithms/src/queue.h":
    ctypedef struct Queue:
        pass
    ctypedef void* QueueValue

    Queue* queue_new()
    void queue_free(Queue* queue)

    int queue_push_head(Queue* queue, QueueValue data)
    QueueValue queue_pop_head(Queue* queue)
    QueueValue queue_peek_head(Queue* queue)

    int queue_push_tail(Queue* queue, QueueValue data)
    QueueValue queue_pop_tail(Queue* queue)
    QueueValue queue_peek_tail(Queue* queue)

    bint queue_is_empty(Queue* queue)
```

### Writing a wrapper class

After declaring our C library’s API, we can start to design the Queue class that should wrap the C queue. It will live in a file called **queue.pyx**.

Here is a first start for the Queue class:

```python
# queue.pyx

cimport cqueue

cdef class Queue:
    cdef cqueue.Queue* _c_queue

    def __cinit__(self):
        self._c_queue = cqueue.queue_new()
```

### Memory management

Before we continue implementing the other methods, it is important to understand that the above implementation is not safe. In case anything goes wrong in the call to **queue_new()**, this code will simply swallow the error, so we will likely run into a crash later on. According to the documentation of the **queue_new()** function, the only reason why the above can fail is due to insufficient memory. In that case, it will return **NULL**, whereas it would normally return a pointer to the new queue.

The Python way to get out of this is to raise a **MemoryError**. We can thus change the init function as follows:

```python
# queue.pyx

cimport cqueue

cdef class Queue:
    cdef cqueue.Queue* _c_queue

    def __cinit__(self):
        self._c_queue = cqueue.queue_new()
        if self._c_queue is NULL:
            raise MemoryError()
```

The next thing to do is to clean up when the Queue instance is no longer used (i.e. all references to it have been deleted). To this end, CPython provides a callback that Cython makes available as a special method **__dealloc__()**. In our case, all we have to do is to free the C Queue, but only if we succeeded in initialising it in the init method:

```python
def __dealloc__(self):
    if self._c_queue is not NULL:
        cqueue.queue_free(self._c_queue)
```

### Mapping functionality

Adding an append() method should now be straight forward:

```python
cdef append(self, int value):
    if not cqueue.queue_push_tail(self._c_queue,
                                  <void*>value):
        raise MemoryError()
```

Adding an extend() method should now be straight forward:

```python
cdef extend(self, int* values, size_t count):
    """Append all ints to the queue.
    """
    cdef int value
    for value in values[:count]:  # Slicing pointer to limit the iteration boundaries.
        self.append(value)
```

So far, we can only add data to the queue. The next step is to write the two methods to get the first element: **peek()** and **pop()**, which provide read-only and destructive read access respectively. To avoid compiler warnings when casting **void*** to **int** directly, we use an intermediate data type that is big enough to hold a **void\***. Here, **Py_ssize_t**:

cdef int peek(self):
    return <Py_ssize_t>cqueue.queue_peek_head(self._c_queue)

cdef int pop(self):
    return <Py_ssize_t>cqueue.queue_pop_head(self._c_queue)

### Handling errors

Now, what happens when the queue is empty? According to the documentation, the functions return a **NULL** pointer, which is typically not a valid value. But since we are simply casting to and from ints, we cannot distinguish anymore if the return value was **NULL** because the queue was empty or because the value stored in the queue was **0**. In Cython code, we want the first case to raise an exception, whereas the second case should simply return **0**. To deal with this, we need to special case this value, and check if the queue really is empty or not:

```python
cdef int peek(self) except? -1:
    cdef int value = <Py_ssize_t>cqueue.queue_peek_head(self._c_queue)
    if value == 0:
        # this may mean that the queue is empty, or
        # that it happens to contain a 0 value
        if cqueue.queue_is_empty(self._c_queue):
            raise IndexError("Queue is empty")
    return value
```

### Compiling and linking

At this point, we have a working Cython module that we can test. To compile it, we need to configure a **setup.py** script for **setuptools**. Here is the most basic script for compiling a Cython module:

```python
from setuptools import Extension, setup
from Cython.Build import cythonize

setup(
    ext_modules = cythonize([Extension("queue", ["queue.pyx"])])
)
```

To build against the external C library, we need to make sure Cython finds the necessary libraries. There are two ways to archive this. First we can tell setuptools where to find the c-source to compile the **queue.c** implementation automatically. Alternatively, we can build and install C-Alg as system library and dynamically link it.

#### Static Linking

To build the c-code automatically we need to include compiler directives in queue.pyx:

```python
# distutils: sources = c-algorithms/src/queue.c
# distutils: include_dirs = c-algorithms/src/

cimport cqueue

cdef class Queue:
    cdef cqueue.Queue* _c_queue
    def __cinit__(self):
        self._c_queue = cqueue.queue_new()
        if self._c_queue is NULL:
            raise MemoryError()

    def __dealloc__(self):
        if self._c_queue is not NULL:
            cqueue.queue_free(self._c_queue)
```

The **sources** compiler directive gives the path of the C files that setuptools is going to compile and link (statically) into the resulting extension module. In general all relevant header files should be found in **include_dirs**. Now we can build the project using:

```shell
$ python setup.py build_ext -i
```

And test whether our build was successful:

```shell
$ python -c 'import queue; Q = queue.Queue()'
```

#### Dynamic Linking

Dynamic linking is useful, if the library we are going to wrap is already installed on the system. To perform dynamic linking we first need to build and install c-alg.

In this approach we need to tell the setup script to link with an external library. To do so we need to extend the setup script to install change the extension setup from

```python
ext_modules = cythonize([Extension("queue", ["queue.pyx"])])
```

to

```python
ext_modules = cythonize([
    Extension("queue", ["queue.pyx"],
              libraries=["calg"])
    ])
```
Now we should be able to build the project using:

```shell
python setup.py build_ext -i
```

If the libcalg is not installed in a ‘normal’ location, users can provide the required parameters externally by passing appropriate C compiler flags, such as:

```shell
CFLAGS="-I/usr/local/otherdir/calg/include"  \
LDFLAGS="-L/usr/local/otherdir/calg/lib"     \
    python setup.py build_ext -i
```

Before we run the module, we also need to make sure that **libcalg** is in the **LD_LIBRARY_PATH** environment variable, e.g. by setting:

```shell
$ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
```

Once we have compiled the module for the first time, we can now import it and instantiate a new Queue:

```shell
$ export PYTHONPATH=.
$ python -c 'import queue; Q = queue.Queue()'
```

## Implementing functions in C

When you want to call C code from a Cython module, usually that code will be in some external library that you link your extension against. However, you can also directly compile C (or C++) code as part of your Cython module. In the **.pyx** file, you can put something like:

```python
cdef extern from "spam.c":
    void order_spam(int tons)
```

Cython will assume that the function **order_spam()** is defined in the file **spam.c**. If you also want to cimport this function from another module, it must be declared (not extern!) in the **.pxd** file:

```python
cdef void order_spam(int tons)
```

For this to work, the signature of **order_spam()** in **spam.c** must match the signature that Cython uses, in particular the function must be static:

```c
static void order_spam(int tons)
{
    printf("Ordered %i tons of spam!\n", tons);
}
```

## Including verbatim C code

For advanced use cases, Cython allows you to directly write C code as “docstring” of a **cdef extern from block**:

```python
cdef extern from *:
    """
    /* This is C code which will be put
     * in the .c file output by Cython */
    static long square(long x) {return x * x;}
    #define assign(x, y) ((x) = (y))
    """
    long square(long x)
    void assign(long& x, long y)
```
