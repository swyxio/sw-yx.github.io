---
layout: post
date: 2017-11-19
tags: happy
categories: react nivo
title: reviewing react dataviz - Nivo
---

Sacha Greif is one of my inspirations in coding+design and he is compiling the State of JS 2017 results here. 

[His preview at NordicJS](https://www.youtube.com/watch?v=FZw1j_tTSag) also dropped a mention on [nivo.rocks](http://nivo.rocks) and that piqued my interest.

I did an interview recently where I needed to pull in a dataviz component and my lack of knowledge around the area cost me a bunch of time in research. Even worse, my eventual choice was so finnicky I never even got the viz up and running in time.

So I mentioned this on twitter to @sxywu, another inspiration: <https://twitter.com/swyx/status/930656969811267584> who also shouted out to vx.

# Review of Nivo 0.30

So I decided to try out nivo this weekend and document my journey along the way. I will be writing some instructions for people who are following along but if I had to teach everything it would be a very long post so pardon if I skip some details.

1. Starting off with the react-babel template from Glitch: <https://glitch.com/~react-babel>
2. add `nivo` and `axios`
3. we're gonna hit up the SWAPI api for data: <https://swapi.co/documentation#species>

## Hello Bar Chart

The data comes as an array of objects: <https://swapi.co/api/species/?format=json> and we want the species name vs average lifespan. That's the source data.

The [nivo bar example](http://nivo.rocks/#/bar) also wants an array of objects, with a `country` label and a `field: value` situation. alright, can't be too hard.

```javascript
export default class App extends React.Component {
  constructor() {
    super()
    this.state = {data: []}
  }
  componentDidMount() {
    axios.get('https://swapi.co/api/species/?format=json').then(x => {
      const parseddata = x.data.results.map(({name, average_lifespan}) => ({name, average_lifespan}))
      this.setState({data: parseddata})
    })
  }
  render () {
    return (
      <div>
         <Bar
          data={this.state.data}
          keys={[
              "average_lifespan"
          ]}
          indexBy="name"
        />
      </div>
    )
  }
}
```

ok. that should work. right?

```
Warning: Failed prop type: The prop `width` is marked as required in `withPropsOnChange(defaultProps(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(pure(Bar)))))))))`, but its value is `undefined`.
    in withPropsOnChange(defaultProps(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(pure(Bar))))))))) (created by defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(pure(Bar)))))))))))
    in defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(pure(Bar)))))))))) (created by withPropsOnChange(defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(pure(Bar))))))))))))
    in withPropsOnChange(defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(pure(Bar))))))))))) (created by defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(pure(Bar)))))))))))))
    in defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(pure(Bar)))))))))))) (created by withPropsOnChange(defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(pure(Bar))))))))))))))
    in withPropsOnChange(defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(pure(Bar))))))))))))) (created by defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(pure(Bar)))))))))))))))
    in defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(defaultProps(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(withPropsOnChange(pure(Bar)))))))))))))) (created by App)
    in div (created by App)
    in App
```

wtf?

[File an issue, move on.](https://github.com/plouc/nivo/issues/89)

After digging through the [nivo docs source code](https://github.com/plouc/nivo-website) i found that they were using a thing called a ResponsiveBar, which is not buggy. swap that out, and it works:

```javascript
import { ResponsiveBar as Bar } from "nivo";
```

![image](https://user-images.githubusercontent.com/6764957/32996695-00847258-cd54-11e7-8f32-3ea1b3e71ba8.png)

Alright now to add some labels.

```javascript
          <Bar
            data={this.state.data}
            keys={["average_lifespan"]}
            indexBy="name"
            axisBottom={{
              orient: "bottom",
              tickSize: 5,
              tickPadding: 5,
              tickRotation: 0,
              legend: "name",
              legendPosition: "center",
              legendOffset: 36
            }}
            axisLeft={{
              orient: "left",
              tickSize: 5,
              tickPadding: 5,
              tickRotation: 0,
              legend: "average_lifespan",
              legendPosition: "center",
              legendOffset: -40
            }}
          />
```

this is copied straight from the site. Should be good enough!

![image](https://user-images.githubusercontent.com/6764957/32996695-00847258-cd54-11e7-8f32-3ea1b3e71ba8.png)

nope. this is the worst of all errors, a silent error. at this point i give up, 2 hours in. [Filed the issue](https://github.com/plouc/nivo/issues/90)

the docs look amazing but i think there is too much that is lacking for this to be a mature library. 

# Review of Nivo v0.30

### Docs: 2/5

the docs bias towards power users and don't have basic examples before building up to bigger things. for example there is no Getting Started. also some evidence suggesting that the API is out of date.

### Functionality: 3/5

it looks like it has a lot of things that could be useful, including animation.

### Beginner friendliness

1/5 see above.

this is pre v1 so of course expectations are low.
