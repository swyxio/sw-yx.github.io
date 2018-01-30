---
layout: post
date: 2018-01-30
tags: misc
feelings: neutral
title: work setup uses
comments: true
description: what i did to setup at work
---

things to install

- <http://matthewpalmer.net/rocket/>
- <https://tinytake.com/tinytake-download>
- <https://www.spectacleapp.com/>
- iterm2
- wesbos.com/uses
- vscode insiders

mac defaults
- <https://www.tekrevue.com/tip/how-to-turn-off-multitouch-trackpad-look-up-in-os-x/>

command line
- iterm2
- <https://www.youtube.com/watch?v=dPYm0tb25O4> trash command
- <https://www.youtube.com/watch?v=qbNn5zJLZU0> z command


github

- git clone from private organizations: <https://help.github.com/articles/authorizing-a-personal-access-token-for-use-with-a-saml-single-sign-on-organization for more information.>

vscode settings

```js
{
    "workbench.colorTheme": "Cobalt2",
    "editor.fontFamily": "Operator Mono, Menlo, Monaco, 'Courier New', monospace",
    "editor.fontSize": 10,
    "editor.lineHeight": 15,
    "editor.letterSpacing": 0.5,
    "files.trimTrailingWhitespace": true,
    "editor.fontWeight": "400",
    "prettier.eslintIntegration": true,
    // this isn't really underline-thin but we hack it to be a thicker cursor
    "editor.cursorStyle": "line",
    "editor.cursorBlinking": "blink",
    // Very important: Install this plugin: https://github.com/be5invis/vscode-custom-css
    // you'll need to change this to an absolute path on your computer
    "vscode_custom_css.imports": [
      "/Volumes/Macintosh HD/Users/wesbos/.vscodestyles.css"
    ],
    "editor.renderWhitespace": "all",
    "terminal.integrated.shell.osx": "zsh",
    "terminal.integrated.fontFamily": "Inconsolata for Powerline",
    "window.zoomLevel": 2,
    "editor.tabSize": 2,
  }
```
