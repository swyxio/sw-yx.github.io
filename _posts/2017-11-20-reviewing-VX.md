---
layout: post
date: 2017-11-20
tags: happy
categories: react-review vx
title: reviewing vx
---

@sxywu is another of my inspirations and mentioned that i should check out vx <https://twitter.com/swyx/status/930656969811267584>

and the creator @hshoff dropped by to say hi so i am very happy about that.

here is the repo <https://github.com/hshoff/vx> and i am reviewing it as of v0.0.1 which is somewhat alarming but so be it.

the docs link to this getting started piece which is GREAT: <https://medium.com/vx-code/getting-started-with-vx-1756bb661410>

# Getting started with VX

The basic docs will get you pretty far:

1. `yarn add @vx/axis @vx/gradient @vx/group @vx/mock-data @vx/scale @vx/shape d3-array`
2. do this

```javascript
import React from "react";
import { AxisLeft, AxisBottom } from "@vx/axis";
import { LinearGradient } from "@vx/gradient";
import { Group } from "@vx/group";
import { scaleTime, scaleLinear } from "@vx/scale";
import { extent, max } from "d3-array";
import { AreaClosed } from "@vx/shape";
import { appleStock } from "@vx/mock-data";
const width = 750;
const height = 400;
const margin = {
  top: 60,
  bottom: 60,
  left: 80,
  right: 80
};
const xMax = width - margin.left - margin.right;
const yMax = height - margin.top - margin.bottom;
const x = d => new Date(d.date); // d.date is unix timestamps
const y = d => d.close;
const data = appleStock;

const xScale = scaleTime({
  range: [0, xMax],
  domain: extent(data, x)
});
const yScale = scaleLinear({
  range: [yMax, 0],
  domain: [0, max(data, y)]
});

const chart = () => (
  <svg width={width} height={height}>
    <Group top={margin.top} left={margin.left}>
      <LinearGradient from="#fbc2eb" to="#a6c1ee" id="gradient" />
      <AreaClosed
        data={data}
        xScale={xScale}
        yScale={yScale}
        x={x}
        y={y}
        fill={"url(#gradient)"}
      />
      <AxisBottom
        scale={xScale}
        top={yMax}
        label={"Years"}
        stroke={"#1b1a1e"}
        tickTextFill={"#1b1a1e"}
      />
      <AxisLeft
        scale={yScale}
        top={0}
        left={0}
        label={"Close Price ($)"}
        stroke={"#1b1a1e"}
        tickTextFill={"#1b1a1e"}
      />
    </Group>
  </svg>
);
export default chart;

```

and we have pretty pictures!

![image](https://user-images.githubusercontent.com/6764957/32998319-290cad62-cd68-11e7-8dec-74ef6cc05312.png)

That's a hell of a lot further than i got with `nivo` yesterday. Isn't it nice to have a Getting Started that works?

# Hello Bar Chart

ok so now lets look at Star Wars character lifespans. We did this yesterday on the SWAPI:

```javascript
  componentDidMount() {
    const apiarray = Array(3)
      .fill(0)
      .map((_, i) =>
        axios
          .get("https://swapi.co/api/species/?format=json&page=" + (i + 1))
          .then(x => x.data.results)
      );
    Promise.all(apiarray).then(resultarr => {
      const x = [].concat.apply([], resultarr);
      const parseddata = x
        .filter(x => x.average_lifespan !== "unknown")
        .map(({ name, average_lifespan }) => ({
          name,
          average_lifespan: Number(average_lifespan)
        }));
      this.setState({ data: parseddata });
    });
  }
```

I just want to put this into a bar chart. How hard can that be? Let's see.

### Basic Bar Chart example from README

following the README.md: <https://github.com/hshoff/vx>. this gives me:

![image](https://user-images.githubusercontent.com/6764957/32998560-37aad694-cd6a-11e7-9ef9-041b909c906b.png)

But there is no axis. Same issue as I had yesterday. Lets apply what we learned from the Getting Started page and add the axis:

```javascript
<Group key={`bar-${i}`}>
  <AxisBottom
    scale={xScale}
    top={yMax}
    label={"Letters"}
    stroke={"#1b1a1e"}
    tickTextFill={"#1b1a1e"}
  />
  <Bar
    x={xPoint(d)}
    y={yMax - barHeight}
    height={barHeight}
    width={xScale.bandwidth()}
    fill="#fc2e1c"
  />
</Group>
```

![image](https://user-images.githubusercontent.com/6764957/32998654-040cc062-cd6b-11e7-84d6-4e56dd50ce1d.png)

Good god! it works! Ok! now let's adapt our star wars data.

### Bar Chart with SWAPI data!

```javascript
import React from "react";
import { Group } from "@vx/group";
import { AxisLeft, AxisBottom } from "@vx/axis";
import { Bar } from "@vx/shape";
import { scaleLinear, scaleBand } from "@vx/scale";
import axios from "axios";

// Define the graph dimensions and margins
const width = 900;
const height = 500;
const margin = { top: 20, bottom: 20, left: 20, right: 20 };

// Then we'll create some bounds
const xMax = width - margin.left - margin.right;
const yMax = height - margin.top - margin.bottom;

// We'll make some helpers to get at the data we want
const x = d => d.name;
const y = d => d.average_lifespan;

export default class App extends React.Component {
  constructor() {
    super();
    this.state = { data: [] };
  }
  componentDidMount() {
    const apiarray = Array(3)
      .fill(0)
      .map((_, i) =>
        axios
          .get("https://swapi.co/api/species/?format=json&page=" + (i + 1))
          .then(x => x.data.results)
      );
    Promise.all(apiarray).then(resultarr => {
      const x = [].concat.apply([], resultarr);
      const parseddata = x
        .filter(x => x.average_lifespan !== "unknown")
        .map(({ name, average_lifespan }) => ({
          name,
          average_lifespan: Number(average_lifespan)
        }));
      this.setState({ data: parseddata });
    });
  }
  render() {
    const data = this.state.data;
    // And then scale the graph by our data
    const xScale = scaleBand({
      rangeRound: [0, xMax],
      domain: data.map(x),
      padding: 0.4
    });
    const yScale = scaleLinear({
      rangeRound: [yMax, 0],
      domain: [0, Math.max(...data.map(y))]
    });

    // Compose together the scale and accessor functions to get point functions
    const compose = (scale, accessor) => data => scale(accessor(data));
    const xPoint = compose(xScale, x);
    const yPoint = compose(yScale, y);
    return (
      <svg width={width} height={height}>
        {data.map((d, i) => {
          const barHeight = yMax - yPoint(d);
          return (
            <Group key={`bar-${i}`}>
              <AxisBottom
                scale={xScale}
                top={yMax}
                label={"Species"}
                stroke={"#1b1a1e"}
                tickTextFill={"#1b1a1e"}
              />
              <Bar
                x={xPoint(d)}
                y={yMax - barHeight}
                height={barHeight}
                width={xScale.bandwidth()}
                fill="#fc2e1c"
              />
            </Group>
          );
        })}
      </svg>
    );
  }
}
```

awesome!

![image](https://user-images.githubusercontent.com/6764957/32998878-a0c8ff8c-cd6c-11e7-8575-02689e02e676.png)

I try to add a left axis:

```javascript
<AxisLeft
  scale={yScale}
  top={0}
  left={0}
  label={"Years"}
  stroke={"#1b1a1e"}
  tickTextFill={"#1b1a1e"}
/>
```

and this fails silently which is no good but I am a bit happier than i was with nivo.

# Review of vx v0.0.1

### Docs: 2/5

[The docs](https://vx-demo.now.sh/docs) contain basic examples and dont have the interactivity of nivo's examples, but at least they give just enough to work with (although the code shown is not the same as the charts shown which is mildly annoying. where are the docs' source? could not find). [Some docs](https://vx-demo.now.sh/static/docs/vx-legend.html) are still not yet filled out.

### Functionality: 4/5

I was going to say that this doesnt seem to have hover label support but <https://vx-demo.now.sh/areas> seems to implement something like it using an `onMouseMove` event handler. this is clunky compared to `nivo` but i can live with that. the gallery is fanatastic and they even have funky stuff i'll probably never use like <https://vx-demo.now.sh/voronoi>
Overall i have no functionality concerns (probably because there is a lot of mapping over the d3 api) and I just care more about making it up the learning curve right now. The community is actively working on [the Roadmap](https://github.com/hshoff/vx/blob/master/ROADMAP.md).

### Beginner friendliness: 4/5

The presence of a progressive Getting Started guide is good, as well as the content available on the [blog](https://medium.com/vx-code) and the small list of "In the wild" implementations in the [README](https://github.com/hshoff/vx). The maintainers seem to be responsive on twitter and on GH issues. The slack community is part of the d3 community which is very clever.
I am only not giving a perfect score because this is pre v1 and there arent a bunch of community written tutorials around it yet but that will come with time.

this is pretty good for a v0.0.1 and i am looking forward to revisiting as they execute on their [roadmap](https://github.com/hshoff/vx/blob/master/ROADMAP.md)

