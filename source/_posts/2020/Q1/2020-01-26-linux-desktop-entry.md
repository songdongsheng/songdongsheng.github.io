---
title: Linux Desktop Entry
excerpt: Linux Desktop Entry
date: 2020-01-26 11:19:25
tags:
  - Linux
categories: [Operating system, Linux]
---

# Linux Desktop Entry

## Desktop entry location

```bash
${HOME}/.config/autostart/google-chrome.desktop
${HOME}/.local/share/applications/jetbrains-idea.desktop
/usr/share/applications/google-chrome.desktop
/usr/share/applications/firefox-esr.desktop
```

## Google Chrome

```
[Desktop Entry]
Type=Application
Name=Google Chrome
GenericName=Web Browser
Comment=Access the Internet
Exec=/usr/bin/google-chrome-stable %U
StartupNotify=true
Terminal=false
Icon=google-chrome
Categories=Network;WebBrowser;
MimeType=text/html;text/xml;application/xhtml_xml;image/webp;x-scheme-handler/http;x-scheme-handler/https;x-scheme-handler/ftp;
Actions=new-window;new-private-window;

[Desktop Action new-window]
Name=New Window
Exec=/usr/bin/google-chrome-stable

[Desktop Action new-private-window]
Name=New Incognito Window
Exec=/usr/bin/google-chrome-stable --incognito
```

## Mozilla Firefox ESR

```
[Desktop Entry]
Type=Application
Name=Firefox ESR
GenericName=Web Browser
Comment=Browse the World Wide Web
Exec=/usr/lib/firefox-esr/firefox-esr %u
StartupNotify=true
Terminal=false
Icon=firefox-esr
Categories=Network;WebBrowser;
MimeType=text/html;text/xml;application/xhtml+xml;application/xml;application/vnd.mozilla.xul+xml;application/rss+xml;application/rdf+xml;image/gif;image/jpeg;image/png;x-scheme-handler/http;x-scheme-handler/https;
```
