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

from 

```tsx
// for now, user is responsible for getting and providing all the configs
// in future we will handle fetching and caching
export type Config = {
  [key: string]: any
}

// for Requirements to get values
export type Getter = (config: Config) => Promise<number | string | boolean>
// States and actions can call Asserter with Getter output to check truthiness
export type Asserter = (getterOutput: any) => Promise<Boolean>

// // failed attempt at making overloaded functions
// export function Asserter(getterOutput: string): Promise<Boolean>
// export function Asserter(getterOutput: number): Promise<Boolean>
// export async function Asserter() {
//   return false
// }

// a unitary requirement that the user defines
export type Requirement = AsyncRequirement // | SyncRequirement
export type ValidatedRequirement = Requirement & { isValid: boolean }
export type AsyncRequirement = {
  name: string
  description?: string
  getter: Getter
  assert: Asserter
}

// a state is just a "dumb" collection of requirements
export type ValidatedState = State & { isValid: boolean }
export type State = {
  uniqueName: string
  description?: string
  requirements: Requirement[]
}

/**
 * an Action is the primary unit of the state machine
 *
 * see field comments for what each does
 *  */
export type Action = {
  uniqueId: string
  description?: string
  /** "BEFORE": prerequisite states to check */
  preStates: State[]
  /** "DURING": the meat of the action to execute once */
  execute: (config: Config) => Promise<void>
  /** "AFTER": a state that should be fulfilled after execute runs */
  postState?: State
  /** if postExcuteState isn't fulfilled, should we repeat this Action?
   *
   * default falsy to blame the developer for postExecuteState not being fulfiled */
  repeatable?: boolean
  /** if requirements aren't fulfiled, what to do */
  failure?: Function
}

// /**
//  * a Healing Action is designed to explicitly take a state to another state
//  *
//  *  */
// export type HealingAction = {
//   uniqueId: string
//   description?: string
//   /** prerequisite state to check */
//   preState: State
//   /** the meat of the action to execute once */
//   execute: (arg?: any) => Promise<void>
//   /** the requirements that will be fulfilled after execute runs */
//   postExecuteRequirements?: Requirement[]
//   /** if requirements aren't fulfiled, what to do */
//   failure?: Function
// }
```

to

```ts
// for now, user is responsible for getting and providing all the configs
// in future we will handle fetching and caching
export type Config = {
  [key: string]: any
}

// a state is just a "dumb" collection of requirements
export type ValidatedState = State & { isValid: boolean }
export type State<T = any> = {
  stateId: string
  description?: string
  value: (config: Config) => Promise<T>
  assert: (getterOutput: T) => Promise<Boolean>
}

/**
 * an Action is the primary unit of the state machine
 *
 * see field comments for what each does
 *  */
export type Action<T> = {
  actionId: string
  description?: string
  /** "BEFORE": prerequisite states to check */
  preStates: State[]
  /** "DURING": the meat of the action to execute once */
  execute: (config: Config, value: T) => Promise<void>
  /** "AFTER": a state that should be fulfilled after execute runs */
  postState?: State
  /** if postExcuteState isn't fulfilled, should we repeat this Action?
   *
   * make truthy to repeat executing until postState is fulfiled
   * default falsy to blame the developer for postState not being fulfiled */
  repeatable?: boolean
}
```


----

this lets me get rid of code like this

```ts

/**
 * iterate through an array of requirements to check they are valid (by calling .assert)
 *
 * return array of validatedRequirements that you can easily call `.every` on to check.
 * intentionally don't run `every` for you so you can iterate through
 */
const validateRequirements = async (reqs: Requirement[], config: Config) => {
  const validator = async (req: Requirement) => req.assert(await req.getter(config))
  return await Promise.all(
    reqs.map(async req => ({ ...req, isValid: await validator(req) } as ValidatedRequirement))
  )
}
```

---

i fought really hard to keep the preStates an array... but this is impossible to type:

```ts
/**
 * an Action is the primary unit of the state machine
 *
 * see field comments for what each does
 *  */
export type Action<PreStateType = any, ExecuteType = any, PostStateType = any> = {
  actionId: string
  description?: string
  /** "BEFORE": prerequisite states to check */
  preStates: State<PreStateType>[]
  /** "DURING": the meat of the action to execute once */
  execute: (config: Config, value: ExecuteType) => Promise<void>
  /** "AFTER": a state that should be fulfilled after execute runs */
  postState?: State<PostStateType>
  /** if postExcuteState isn't fulfilled, should we repeat this Action?
   *
   * make truthy to repeat executing until postState is fulfiled
   * default falsy to blame the developer for postState not being fulfiled */
  repeatable?: boolean
}

```
