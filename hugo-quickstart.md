1. Download & install Hugo

```shell
$ curl -sL https://github.com/gohugoio/hugo/releases/download/v0.99.1/hugo_extended_0.99.1_Linux-64bit.tar.gz | tar -xvzf -
$ curl -sL https://github.com/gohugoio/hugo/releases/download/v0.99.1/hugo_extended_0.99.1_Windows-64bit.zip | bsdtar -xvf -
LICENSE
README.md
hugo

$ rm -f LICENSE README.md
$ sudo mv hugo /usr/bin/
$ hugo version
hugo v0.99.1-d524067382e60ce2a2248c3133a1b3af206b6ef1+extended windows/amd64 BuildDate=2022-05-18T11:18:14Z VendorInfo=gohugoio
hugo v0.99.1-d524067382e60ce2a2248c3133a1b3af206b6ef1+extended linux/amd64 BuildDate=2022-05-18T11:18:14Z VendorInfo=gohugoio
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

$ git submodule add -b master https://github.com/zhaohuabing/hugo-theme-cleanwhite.git themes/cleanwhite
$ echo 'theme = "cleanwhite"' >> config.toml

$ git submodule add -b master https://github.com/kimcc/hugo-theme-noteworthy.git themes/noteworthy
$ echo 'theme = "noteworthy"' >> config.toml

$ git submodule add -b master https://github.com/CaiJimmy/hugo-theme-stack.git themes/stack
$ echo 'theme = "stack"' >> config.toml

$ git submodule add -b master https://github.com/cofess/hexo-theme-pure.git themes/pure
$ echo 'theme = "pure"' >> config.toml

$ git submodule add -b main https://github.com/dataCobra/hugo-vitae.git themes/vitae
$ echo 'theme = "vitae"' >> config.toml

$ git submodule add -b master https://github.com/taikii/whiteplain.git themes/whiteplain
$ echo 'theme = "whiteplain"' >> config.toml
```

```
$ cat config.toml

languageCode = 'en-us'
title = 'My New Hugo Site'

#theme = "cleanwhite"
#theme = "noteworthy"
#theme = "pure"
#theme = "stack"
#theme = "vitae"
theme = "whiteplain"
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
