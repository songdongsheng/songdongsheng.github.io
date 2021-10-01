---
title: Julia Data Types
excerpt: Julia Data Types
date: 2021-04-17 08:11:15
tags:
    - Programming
    - Julia
categories: [Programming, Julia]
---

# Julia Data Types

## Integer types

| Type    | Signed? | Number of bits | Range               |
|---------|---------|----------------|---------------------|
|    Int8 | ✓       |   8            | -2^7 ~ 2^7 - 1      |
|   UInt8 |         |   8            | 0 ~ 2^8 - 1         |
|   Int16 | ✓       |  16            | -2^15 ~ 2^15 - 1    |
|  UInt16 |         |  16            | 0 ~ 2^16 - 1        |
|   Int32 | ✓       |  32            | -2^31 ~ 2^31 - 1    |
|  UInt32 |         |  32            | 0 ~ 2^32 - 1        |
|   Int64 | ✓       |  64            | -2^63 ~ 2^63 - 1    |
|  UInt64 |         |  64            | 0 ~ 2^64 - 1        |
|  Int128 | ✓       | 128            | -2^127 ~ 2^127 - 1  |
| UInt128 |         | 128            | 0 ~ 2^128 - 1       |
|    Bool | N/A     |   8            | false (0), true (1) |

```julia
julia> typeof(3000000000)
Int64

julia> (typemin(Int32), typemax(Int32))
(-2147483648, 2147483647)

julia> for T in [Int8, Int16, Int32, Int64, Int128, UInt8, UInt16, UInt32, UInt64, UInt128]
           println("$(lpad(T, 8)): [$(typemin(T)), $(typemax(T))]")
       end
    Int8: [-128, 127]
   Int16: [-32768, 32767]
   Int32: [-2147483648, 2147483647]
   Int64: [-9223372036854775808, 9223372036854775807]
  Int128: [-170141183460469231731687303715884105728, 170141183460469231731687303715884105727]
   UInt8: [0, 255]
  UInt16: [0, 65535]
  UInt32: [0, 4294967295]
  UInt64: [0, 18446744073709551615]
 UInt128: [0, 340282366920938463463374607431768211455]
```

## Floating-point types

Floating point numbers follow IEEE 754 standard.

| Type    | Precision | Number of bits |
|---------|-----------|----------------|
| Float16 | half      | 16             |
| Float32 | single    | 32             |
| Float64 | double    | 64             |

## Complex number type

The global constant im is bound to the complex number i, representing the principal square root of -1.

| Type             | Alias      | Number of bits |
|------------------|------------|----------------|
| Complex{Float16} | ComplexF16 | 16             |
| Complex{Float32} | ComplexF32 | 32             |
| Complex{Float64} | ComplexF64 | 64             |

```julia
julia> 1 + 2im
1 + 2im

julia> (1 + 2im)*(2 - 3im)
8 + 1im

julia> (1 + 2im)/(1 - 2im)
-0.6 + 0.8im

julia> (-1 + 2im)^(1 + 1im)
-0.27910381075826657 + 0.08708053414102428im
```

## Arbitrary Precision Arithmetic

The BigInt and BigFloat types are available in Julia for arbitrary precision integer and floating point numbers respectively.

### BigInt

```julia
julia> BigInt(typemax(Int64)) + 1
9223372036854775808

julia> big"123456789012345678901234567890" + 1
123456789012345678901234567891

julia> parse(BigInt, "123456789012345678901234567890") + 1
123456789012345678901234567891

julia> typeof(ans)
BigInt

julia> string(big"2"^200, base=16)
"100000000000000000000000000000000000000000000000000"
```

### BigFloat

```julia
julia> big"1.23456789012345678901"
1.234567890123456789010000000000000000000000000000000000000000000000000000000004

julia> typeof(ans)
BigFloat

julia> (big"2")^66/big"3"
2.459565876494606882133333333333333333333333333333333333333333333333333333333344e+19

julia> typeof(ans)
BigFloat

julia> Base.MPFR.precision((big"2")^66/big"3")
256

julia> setprecision(16) do
           ans = (big"2")^66/big"3"
           println("$(ans): $(Base.MPFR.precision(ans))")
       end
2.45958e+19: 16

julia> setprecision(512) do
           ans = (big"2")^66/big"3"
           println("$(ans): $(Base.MPFR.precision(ans))")
       end
2.45956587649460688213333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333343e+19: 512
```

### Rational Numbers

Julia has a rational number type to represent exact ratios of integers. Rationals are constructed using the // operator:

```julia
julia> 2//3
2//3

julia> numerator(2//3)
2

julia> denominator(2//3)
3

julia> 2//3 == 6//9
true

julia> 2//3^36
2//150094635296999121

julia> typeof(ans)
Rational{Int64}

julia> big"5"//3
5//3

julia> typeof(ans)
Rational{BigInt}
```

## Strings

Strings are finite sequences of characters. The built-in concrete type used for strings (and string literals) in Julia is String. This supports the full range of Unicode characters via the UTF-8 encoding.

```julia
julia> c = 'x'
'x': ASCII/Unicode U+0078 (category Ll: Letter, lowercase)

julia> typeof(c)
Char

julia> Char(120)
'x': ASCII/Unicode U+0078 (category Ll: Letter, lowercase)

julia> Char(0x110000)
'\U110000': Unicode U+110000 (category In: Invalid, too high)

julia> isvalid(Char, 0x110000)
false

julia> isvalid(Char, 0x11000)
true

julia> Char(0x11000)
'𑀀 ': Unicode U+11000 (category Mc: Mark, spacing combining)

julia> '\u2200'
'∀': Unicode U+2200 (category Sm: Symbol, math)

julia> str = "Hello, world.\n"
"Hello, world.\n"

julia> typeof(ans)
String

julia> """Contains "quote" characters"""
"Contains \"quote\" characters"

julia> typeof(ans)
String
```

### Regular Expressions

Julia has Perl-compatible regular expressions (regexes), as provided by the PCRE library.

```julia
julia> re = r"^\s*(?:#|$)"
r"^\s*(?:#|$)"

julia> typeof(re)
Regex
```

### Byte Array Literals

Another useful non-standard string literal is the byte-array string literal: b"...". This form lets you use string notation to express read only literal byte arrays – i.e. arrays of UInt8 values. The type of those objects is CodeUnits{UInt8, String}.

```julia
julia> b"DATA\xff\u2200"
8-element Base.CodeUnits{UInt8, String}:
 0x44
 0x41
 0x54
 0x41
 0xff
 0xe2
 0x88
 0x80

julia> typeof(ans)
Base.CodeUnits{UInt8, String}

julia> x = b"123"
3-element Base.CodeUnits{UInt8, String}:
 0x31
 0x32
 0x33

julia> Vector{UInt8}(x)
3-element Vector{UInt8}:
 0x31
 0x32
 0x33
```

### Version Number Literals

Version numbers can easily be expressed with non-standard string literals of the form v"...".

```julia
if v"0.2" <= VERSION < v"0.3-"
    # do something specific to 0.2 release series
end
```
