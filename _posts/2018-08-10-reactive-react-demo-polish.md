i had an app component

```js

class App extends Component {
  initialState = "counter"
  router = createHandler(e => e.target.name)
  source($) {
    return this.router.$
  }
  render(state) {
    return <div className="container">
              <header>
                <ul className="nav">
                        <li>
                          <button name="counter" onClick={this.router}>Counter</button>
                        </li>
                        <li>
                          <button name="timer" onClick={this.router}>Timer</button>
                        </li>
                    </ul>
                    <h5>reactive-react</h5>
                    <p class="subtitle">{state}</p>
              </header>
              {/* <h2>This site is a starting point</h2>
              <p>From this point we should already have:</p> */}
              {/* <Counter name="Counter Count:"/>
              <Timer /> */}
            </div>
  }
}

```

and i wanted to pull out the navli's to refactor for more concise code:

```js
function NavLi({pageName, onClick}) {
  return <li>
        <button name={pageName} onClick={onClick}>{pageName}</button>
      </li>
}
```

but this caused an infinite loop for god knows what reason. so i abandoned the consiseness
