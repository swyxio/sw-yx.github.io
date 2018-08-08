---
layout: post
date: 2018-08-07
tags: react
feelings: neutral
title: finishing reactive react
comments: true
description: state in state
---

i start at netlify tomorrow! i had a great call with david wells today and i am still a little in disbelief that i was offered this great job. i really dont want to screw this up.

i took a little detour reading about operating systems and have more content to add. i redid my presentation from beautiful.ai to slides.com and have more freedom around it - in particular, being able to build my diagrams in the app is important.

i renamed creat to reactive-react. i am pretty much dropping the alternate universe thing - i think it is too ambitious for my first ever talk. 

i last left it in the middle of a refactor to stateMap. i need to finish it so that every component has local state based on the component instance. then i need to work on demos.

---

statemap worked well

i tried to do the timeslicing thing but that was a bit of a fail because victory requires react. and i dont really feel like doing a bunch of react-reactive-react compat work OR working in D3.

---

# time travel attempt

this works:

```js

class TimeTravel extends Component {
  states = []
  stateIndex = 0
  handler = createHandler(e => {
    this.states.splice(this.stateIndex++, 0, e.target.value)
    return e.target.value
  })
  undo = createHandler(e => 'UNDO')
  redo = createHandler(e => 'REDO')
  initialState = 'hello world'
  source($) {
    const source = merge(this.handler.$, this.undo.$, this.redo.$)
    const reducer = (acc, n) => {
      let newIndex
      switch (n) {
        case 'UNDO': 
          newIndex = this.stateIndex < 1 ? 0 : --this.stateIndex
          return this.states[newIndex]
        case 'REDO': 
          newIndex = this.stateIndex > (this.states.length - 1) ? this.states.length : ++this.stateIndex
          return this.states[newIndex]
        default:
          return n
      } 
    }
    return {source, reducer}
  }
  render(state, prevState) {
    return <div>
        <button onClick={this.undo}>undo</button>
        <button onClick={this.redo}>redo</button>
        <input value={state} onInput={this.handler}/>
        {state}
      </div>
  }
}
```

this doesnt work:

```js

class TimeTravel extends Component {
  handler = createHandler(e => {
    // this.states.splice(this.stateIndex++, 0, e.target.value)
    return e.target.value
  })
  undo = createHandler(e => 'UNDO')
  redo = createHandler(e => 'REDO')
  initialState = {
    value: 'hello world',
    history: {
      states: [],
      stateIndex: 0
    }
  }
  source($) {
    return this.combineReducer({
      value: () => {
        return {source: this.handler.$}
      },
      history: () => {
        const source = merge(this.undo.$, this.redo.$)
        const reducer = (acc, n) => {
          console.log({acc, n})
          return n
          // let newIndex
          // switch (n) {
          //   case 'UNDO': 
          //     newIndex = this.stateIndex < 1 ? 0 : --this.stateIndex
          //     return this.states[newIndex]
          //   case 'REDO': 
          //     newIndex = this.stateIndex > (this.states.length - 1) ? this.states.length : ++this.stateIndex
          //     return this.states[newIndex]
          //   default:
          //     return n
          // } 
        }
        return {source, reducer}
      }
    })
  }
  render(state, prevState) {
    return <div>
        <button onClick={this.undo}>undo</button>
        <button onClick={this.redo}>redo</button>
        <input value={state} onInput={this.handler}/>
        {state}
      </div>
  }
}
```
