1. Download & install Hugo

```shell
$ aria2c https://github.com/gohugoio/hugo/releases/download/v0.79.0/hugo_0.79.0_Linux-64bit.tar.gz

$ tar -xvzf hugo_0.79.0_Linux-64bit.tar.gz
LICENSE
README.md
hugo

$ rm -f hugo_0.79.0_Linux-64bit.tar.gz LICENSE README.md
$ sudo mv hugo /usr/bin/
$ hugo version
Hugo Static Site Generator v0.79.0-1415EFDC linux/amd64 BuildDate: 2020-11-27T09:09:02Z

```

2. Create a new site

```shell
$ SITE_DIR=$(mktemp -d)
$ cd ${SITE_DIR}
$ hugo new site quickstart
```

3. Add a theme

```shell
$ cd ${SITE_DIR}/quickstart
$ git init
$ git submodule add https://github.com/CaiJimmy/hugo-theme-stack.git themes/stack
$ echo 'theme = "stack"' >> config.toml
```

4. Add some content

```shell
hugo new posts/my-first-post.md
```

5. Start the Hugo server

```shell
hugo server -D
```

6. Navigate to your new site

```shell
$ xdg-open http://localhost:1313/.
```

7. Site Configuration

```shell
$ vi config.yaml

baseURL: http://songdongsheng.github.io/
title: My New Hugo Site
languageCode: en-us
theme: stack

DefaultContentLanguage: en
```

8. Build static pages

```shell
hugo -D
```
