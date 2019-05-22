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


---

i also dont like this repetitive api...

```ts
import chalk from 'chalk'
import { Action, State, Requirement, Config } from './types'
import { prompt } from 'enquirer'

export const createRequiredFieldInConfig = (
  /** field name */
  fieldName: string,
  options: {
    /** message to the user to prompt for field */
    message?: string
    /** default reply if you have one to specify */
    initial?: string
  } = {}
) => {
  // may want to put more validation on the fieldName in future
  if (fieldName.includes(' '))
    throw new Error(`fieldName ${chalk.yellow(fieldName)} cannot have a space in it`)

  const fieldNameMissingRequirement: Requirement = {
    name: 'fieldNameMissingRequirement_' + fieldName,
    getter: async (config: Config) => config[fieldName],
    assert: async (field: any) => field === undefined,
  }
  const fieldNameExistsRequirement: Requirement = {
    name: 'fieldNameExistsRequirement_' + fieldName,
    getter: async (config: Config) => config[fieldName],
    assert: async (field: any) => field !== undefined,
  }
  const fieldNameMissingState: State = {
    uniqueName: 'fieldNameMissingState_' + fieldName,
    requirements: [fieldNameMissingRequirement],
  }
  const fieldNameExistsState: State = {
    uniqueName: 'fieldNameExistsState_' + fieldName,
    requirements: [fieldNameExistsRequirement],
  }
  const fieldNamePromptAction: Action = {
    uniqueId: 'fieldNamePromptAction_' + fieldName,
    requiredStates: [fieldNameMissingState],
    postExecuteState: fieldNameExistsState,
    execute: async config => {
      const question = {
        type: 'input',
        name: 'answer',
        message: options.message || `Enter ${chalk.yellow(fieldName)}: `,
        initial: options.initial,
      }
      const { answer } = await prompt(question)
      config[fieldName] = answer
    },
  }
  return {
    fieldNameMissingRequirement,
    fieldNameExistsRequirement,
    fieldNameMissingState,
    fieldNameExistsState,
    fieldNamePromptAction,
  }
}
```

so i am getting rid of the idea of requirements.

