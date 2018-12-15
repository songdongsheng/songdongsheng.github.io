---
title: List and Install VSCode Extensions
description: List and Install VSCode Extensions
date: 2020-01-18 21:34:51
tags:
  - VSCode
categories: [Programming, IDE]
permalink: vscode-extensions
---

# List and Install VSCode Extensions

## List Extensions

```bash
code --list-extensions
```

## Install Extensions - bash

```bash
code --list-extensions | LC_COLLATE=en_US sort | xargs -n 1 echo code --install-extension

code --install-extension aaron-bond.better-comments
code --install-extension ahebrank.yaml2json
code --install-extension AlanWalk.markdown-toc
code --install-extension alefragnani.Bookmarks
code --install-extension austin.code-gnu-global
code --install-extension bat67.markdown-extension-pack
code --install-extension bierner.markdown-emoji
code --install-extension brendandburns.vs-kubernetes
code --install-extension buianhthang.xml2json
code --install-extension christian-kohler.npm-intellisense
code --install-extension ckolkman.vscode-postgres
code --install-extension CoenraadS.bracket-pair-colorizer
code --install-extension darkriszty.markdown-table-prettify
code --install-extension DavidAnson.vscode-markdownlint
code --install-extension dbaeumer.vscode-eslint
code --install-extension dgileadi.java-decompiler
code --install-extension donjayamanne.githistory
code --install-extension DotJoshJohnson.xml
code --install-extension eamodio.gitlens
code --install-extension ecmel.vscode-html-css
code --install-extension ecmel.vscode-spring-boot
code --install-extension EditorConfig.EditorConfig
code --install-extension eriklynd.json-tools
code --install-extension esbenp.prettier-vscode
code --install-extension felipecaputo.git-project-manager
code --install-extension firefox-devtools.vscode-firefox-debug
code --install-extension formulahendry.code-runner
code --install-extension formulahendry.terminal
code --install-extension formulahendry.vscode-mysql
code --install-extension goessner.mdmath
code --install-extension gurayyarar.editorenhancements
code --install-extension henoc.svgeditor
code --install-extension hollowtree.vue-snippets
code --install-extension HookyQR.beautify
code --install-extension humao.rest-client
code --install-extension ipedrazas.kubernetes-snippets
code --install-extension James-Yu.latex-workshop
code --install-extension jebbs.plantuml
code --install-extension kalitaalexey.vscode-rust
code --install-extension lextudio.restructuredtext
code --install-extension mariorodeghiero.vue-theme
code --install-extension michelemelluso.code-beautifier
code --install-extension mohsen1.prettify-json
code --install-extension ms-azuretools.vscode-docker
code --install-extension msjsdiag.debugger-for-chrome
code --install-extension msjsdiag.vscode-react-native
code --install-extension ms-kubernetes-tools.vscode-kubernetes-tools
code --install-extension ms-mssql.mssql
code --install-extension ms-python.python
code --install-extension ms-vscode.cpptools
code --install-extension ms-vscode.csharp
code --install-extension ms-vscode.Go
code --install-extension ms-vscode.powershell
code --install-extension ms-vscode-remote.remote-ssh-edit
code --install-extension ms-vscode-remote.remote-wsl
code --install-extension ms-vsliveshare.vsliveshare
code --install-extension nickdemayo.vscode-json-editor
code --install-extension octref.vetur
code --install-extension Pivotal.vscode-boot-dev-pack
code --install-extension Pivotal.vscode-concourse
code --install-extension Pivotal.vscode-manifest-yaml
code --install-extension Pivotal.vscode-spring-boot
code --install-extension pkosta2006.rxjs-snippets
code --install-extension quillaja.goasm
code --install-extension redhat.java
code --install-extension redhat.vscode-xml
code --install-extension redhat.vscode-yaml
code --install-extension rust-lang.rust
code --install-extension sdras.night-owl
code --install-extension sdras.vue-vscode-extensionpack
code --install-extension sdras.vue-vscode-snippets
code --install-extension shanoor.vscode-nginx
code --install-extension shd101wyy.markdown-preview-enhanced
code --install-extension tombonnike.vscode-status-bar-format-toggle
code --install-extension VisualStudioExptTeam.vscodeintellicode
code --install-extension vscjava.vscode-java-debug
code --install-extension vscjava.vscode-java-dependency
code --install-extension vscjava.vscode-java-pack
code --install-extension vscjava.vscode-java-test
code --install-extension vscjava.vscode-maven
code --install-extension vscjava.vscode-spring-boot-dashboard
code --install-extension vscjava.vscode-spring-initializr
code --install-extension wmaurer.vscode-jumpy
code --install-extension xabikos.JavaScriptSnippets
code --install-extension yzane.markdown-pdf
code --install-extension yzhang.markdown-all-in-one
code --install-extension ZakCodes.rust-snippets
```

## Install Extensions - PowerShell

```bash
code --list-extensions | sort | % { "code --install-extension $_" }

code --install-extension aaron-bond.better-comments
code --install-extension ahebrank.yaml2json
code --install-extension AlanWalk.markdown-toc
code --install-extension alefragnani.Bookmarks
code --install-extension austin.code-gnu-global
code --install-extension bat67.markdown-extension-pack
code --install-extension bierner.markdown-emoji
code --install-extension brendandburns.vs-kubernetes
code --install-extension buianhthang.xml2json
code --install-extension christian-kohler.npm-intellisense
code --install-extension ckolkman.vscode-postgres
code --install-extension CoenraadS.bracket-pair-colorizer
code --install-extension darkriszty.markdown-table-prettify
code --install-extension DavidAnson.vscode-markdownlint
code --install-extension dbaeumer.vscode-eslint
code --install-extension dgileadi.java-decompiler
code --install-extension donjayamanne.githistory
code --install-extension DotJoshJohnson.xml
code --install-extension eamodio.gitlens
code --install-extension ecmel.vscode-html-css
code --install-extension ecmel.vscode-spring-boot
code --install-extension EditorConfig.EditorConfig
code --install-extension eriklynd.json-tools
code --install-extension esbenp.prettier-vscode
code --install-extension felipecaputo.git-project-manager
code --install-extension firefox-devtools.vscode-firefox-debug
code --install-extension formulahendry.code-runner
code --install-extension formulahendry.terminal
code --install-extension formulahendry.vscode-mysql
code --install-extension goessner.mdmath
code --install-extension gurayyarar.editorenhancements
code --install-extension henoc.svgeditor
code --install-extension hollowtree.vue-snippets
code --install-extension HookyQR.beautify
code --install-extension humao.rest-client
code --install-extension ipedrazas.kubernetes-snippets
code --install-extension James-Yu.latex-workshop
code --install-extension jebbs.plantuml
code --install-extension kalitaalexey.vscode-rust
code --install-extension lextudio.restructuredtext
code --install-extension mariorodeghiero.vue-theme
code --install-extension michelemelluso.code-beautifier
code --install-extension mohsen1.prettify-json
code --install-extension ms-azuretools.vscode-docker
code --install-extension msjsdiag.debugger-for-chrome
code --install-extension msjsdiag.vscode-react-native
code --install-extension ms-kubernetes-tools.vscode-kubernetes-tools
code --install-extension ms-mssql.mssql
code --install-extension ms-python.python
code --install-extension ms-vscode.cpptools
code --install-extension ms-vscode.csharp
code --install-extension ms-vscode.Go
code --install-extension ms-vscode.powershell
code --install-extension ms-vscode-remote.remote-ssh-edit
code --install-extension ms-vscode-remote.remote-wsl
code --install-extension ms-vsliveshare.vsliveshare
code --install-extension nickdemayo.vscode-json-editor
code --install-extension octref.vetur
code --install-extension Pivotal.vscode-boot-dev-pack
code --install-extension Pivotal.vscode-concourse
code --install-extension Pivotal.vscode-manifest-yaml
code --install-extension Pivotal.vscode-spring-boot
code --install-extension pkosta2006.rxjs-snippets
code --install-extension quillaja.goasm
code --install-extension redhat.java
code --install-extension redhat.vscode-xml
code --install-extension redhat.vscode-yaml
code --install-extension rust-lang.rust
code --install-extension sdras.night-owl
code --install-extension sdras.vue-vscode-extensionpack
code --install-extension sdras.vue-vscode-snippets
code --install-extension shanoor.vscode-nginx
code --install-extension shd101wyy.markdown-preview-enhanced
code --install-extension tombonnike.vscode-status-bar-format-toggle
code --install-extension VisualStudioExptTeam.vscodeintellicode
code --install-extension vscjava.vscode-java-debug
code --install-extension vscjava.vscode-java-dependency
code --install-extension vscjava.vscode-java-pack
code --install-extension vscjava.vscode-java-test
code --install-extension vscjava.vscode-maven
code --install-extension vscjava.vscode-spring-boot-dashboard
code --install-extension vscjava.vscode-spring-initializr
code --install-extension wmaurer.vscode-jumpy
code --install-extension xabikos.JavaScriptSnippets
code --install-extension yzane.markdown-pdf
code --install-extension yzhang.markdown-all-in-one
code --install-extension ZakCodes.rust-snippets
```
