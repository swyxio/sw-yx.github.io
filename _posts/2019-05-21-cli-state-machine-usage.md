---
layout: post
date: 2019-05-21
tags: netilfy
feelings: sick
title: cli state machine usage
comments: true
description: state machine usage
---

at first you start off by assuming a folder exists.

```ts
const functionsDir = flags.functionsDir
```

then you run into situations where you need to ensure the directory exists before you run the code, so this happens


```ts
const functionsDir = ensureFunctionDirExists.call(this, config);


/* get functions dir (and make it if necessary) */
function ensureFunctionDirExists(config) {
  const functionsDir = config.build && config.build.functions;
  if (!functionsDir) {
    this.log(`${NETLIFYDEVLOG} No functions folder specified in netlify.toml`);
    process.exit(1);
  }
  if (!fs.existsSync(functionsDir)) {
    this.log(
      `${NETLIFYDEVLOG} functions folder ${chalk.magenta.inverse(
        functionsDir
      )} specified in netlify.toml but folder not found, creating it...`
    );
    fs.mkdirSync(functionsDir);
    this.log(
      `${NETLIFYDEVLOG} functions folder ${chalk.magenta.inverse(
        functionsDir
      )} created`
    );
  }
  return functionsDir;
}
```

certain things could be better about this:

- i exit when nothing is specified.
- i have to write code to check and make folder if it doesn't exist
- then i return the dirname

