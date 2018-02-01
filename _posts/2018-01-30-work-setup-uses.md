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
- LICEcap for gif recording <http://download.cnet.com/LICEcap/3000-2094_4-75984664.html>

mac defaults
- <https://www.tekrevue.com/tip/how-to-turn-off-multitouch-trackpad-look-up-in-os-x/>

command line
- iterm2
- <https://www.youtube.com/watch?v=dPYm0tb25O4> trash command
- <https://www.youtube.com/watch?v=qbNn5zJLZU0> z command


chrome
- react devtools <https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi/related?hl=en>
- redux devtools <https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en>
- video speed controller <https://chrome.google.com/webstore/detail/video-speed-controller/nffaoalbilbmmfgbnbgppjihopabppdk?hl=en>
- devtools dark theme <https://chrome.google.com/webstore/detail/devtools-theme-zero-dark/bomhdjeadceaggdgfoefmpeafkjhegbo> must enable though: <https://howchoo.com/g/mtu3mwu5mdg/tell-chrome-developer-tools-to-use-a-dark-theme>

github

- git clone from private organizations: <https://help.github.com/articles/authorizing-a-personal-access-token-for-use-with-a-saml-single-sign-on-organization for more information.> and then <https://stackoverflow.com/questions/18935539/authenticate-with-github-using-token>

git push https://tsiq-swyx:4b8561b1b3dd12ae83e0aacb214b7697c8b59f93@github.com/tsiq/iqos-ui.git
Username: tsiq-swyx
Password: 4b8561b1b3dd12ae83e0aacb214b7697c8b59f93

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
