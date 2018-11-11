
# 安装配置
## 初始化版本库
    npm install -g hexo-cli

    hexo init songdongsheng.github.io
    cd songdongsheng.github.io

    npm install --save
    npm install --save hexo-renderer-jade hexo-generator-feed hexo-generator-sitemap hexo-generator-i18n hexo-browsersync hexo-generator-archive

## 使用旧版本库
    npm install -g hexo-cli

    git clone -b hexo git@github.com:songdongsheng/songdongsheng.github.io.git
    cd songdongsheng.github.io

    git submodule update --init --recursive

    npm install --save

## 配置文件简介
    hexo 有两个配置文件，一个在 hexo 项目的根目录，另一个在主题文件夹的根目录，文件名都是均为 _config.yml 。

# 使用主题
## 下载主题
    git submodule add --force https://github.com/lewis-geek/hexo-theme-Aath themes/aath
    git submodule add --force https://github.com/chaooo/hexo-theme-BlueLake themes/BlueLake
    git submodule add --force https://github.com/theme-next/hexo-theme-next themes/next
    git submodule add --force https://github.com/pinggod/hexo-theme-apollo  themes/apollo
    git submodule add --force https://github.com/yanm1ng/hexo-theme-vexo    themes/vexo

    git submodule status
    git submodule update --init --recursive

    git submodule deinit -f themes/apollo
    git rm -f themes/apollo
    rm -fr .git/modules/themes/apollo

    搜索插件 algolia https://www.algolia.com/
    评论插件 来必力  https://www.livere.com/

## 配置主题
    修改 hexo 项目的根目录的 _config.yml，配置 theme 为新主题的名称。

# 常用命令
## 列出站点信息
    hexo list page|post|route|tag|category

## 站点本地预览
    hexo server

## 生成静态文件
    hexo generate

## 部署静态文件
    # dig www.songdongsheng.info +nostats +nocomments +nocmd

    hexo deploy
