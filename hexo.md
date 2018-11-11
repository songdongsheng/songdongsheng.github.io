# 配置文件
hexo 有两个配置文件，一个在 hexo 项目的根目录，另一个在主题文件夹的根目录，文件名都是均为 _config.yml 。

# 安装插件
    npm install -g hexo-cli
    npm install --save hexo-renderer-jade hexo-generator-feed hexo-generator-sitemap hexo-generator-i18n hexo-browsersync hexo-generator-archive
    npm install --save

    git submodule update --init --recursive

# 使用主题
## 下载主题
    git submodule add --force https://github.com/theme-next/hexo-theme-next themes/next
    git submodule add --force https://github.com/lewis-geek/hexo-theme-Aath themes/aath
    git submodule add --force https://github.com/chaooo/hexo-theme-BlueLake themes/BlueLake
    git submodule add --force https://github.com/pinggod/hexo-theme-apollo  themes/apollo
    git submodule add --force https://github.com/yanm1ng/hexo-theme-vexo    themes/vexo

    git submodule status
    git submodule update --init --recursive

    git submodule deinit -f themes/apollo
    git rm -f themes/apollo
    rm -fr .git/modules/themes/apollo

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
    hexo deploy
