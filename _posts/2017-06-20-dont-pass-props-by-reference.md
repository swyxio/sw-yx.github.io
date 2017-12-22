---
layout: post
date: 2017-06-20
categories: pup
title: Dont pass props by reference
tags: happy
---

I have been battling a particularly terrible bug with a nested react component for the past day and only just solved it. The problem essentially was that I had a component (A) that was nested in another component (B), and when I unmounted B (B1) and switched to another instance of B (B2) the values from A (that were valid in B1) were somehow persisting in B2.

The reason this was happening is that I was instantiating A by supplying props from B, and then within A I was modifying those props so it could be read back again by B. However this modified my defaultProps within B and _THOSE_ persisted when i switched from B1 to B2.

god damn.

---

3.00. my task now is to add deleting capability to the guestEditor.

---

3.15. guestEditor complete! for posterity here is the complete solution

```javascript
/* eslint-disable max-len, no-return-assign */

import React from 'react';
import { Button } from 'react-bootstrap';

class GuestEditor extends React.Component {
  constructor(props) {
    super(props);
    // jsonlog('GuestEditorthis',this)
    this.onChange = this.onChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
    this.state = { items: this.props.items, text: '' };
  }

  onChange(e) {
    this.setState({ text: e.target.value });
  }

  handleSubmit() {
    const newItem = this.state.items ? this.state.items : [];
    const text = this.state.text;
    newItem.push(text);
    const newText = '';
    this.setState({ items: newItem, text: newText });
  }
  handleDelete(item) {
    const newItem = this.state.items;
    newItem.splice(newItem.indexOf(item), 1);
    this.setState({ items: newItem });
  }
  render() {
    return (
      <div className="container">
        <div className="inside-box">
          <b>Featured guests:</b>
        </div>
        <ul>{this.state.items && this.state.items.map((item, i) => {
          return <li key={i}><span>{item}</span><Button onClick={() => this.handleDelete(item)}>x</Button></li>;
        })}
        </ul>
        <input
          type="text"
          onChange={this.onChange}
          name="addguest"
          value={this.state.text}
          placeholder="Add guest..."
        />
        <Button onClick={() => this.handleSubmit()}>Add</Button>
      </div>
    );
  }
}

export default GuestEditor;

```


my task now is to port the guests module to the segments module.

the subtask is to create a timepicker. the ones i have tried arent good so I will have to do it myself.

---

10.00. after a long break i am back at it again with renewed purpose after JR emailed me with 180+ user potential. this is potentially a thing. I am going to have to create my own timepicker.

---

timepicker code:
```javascript
function showtime(timeinseconds) {
  const min = parseInt(timeinseconds / 60, 10);
  const sec = parseInt(timeinseconds % 60, 10);
  const minstring = min < 10 ? `0${min}` : String(min);
  const secstring = sec < 10 ? `0${sec}` : String(sec);
  return `${minstring}:${secstring}`;
}

// https://facebook.github.io/react/docs/refs-and-the-dom.html
class TimeInput extends React.Component {
  constructor(props) {
    super(props);
    this.onMinChange = this.onMinChange.bind(this);
    this.onSecChange = this.onSecChange.bind(this);
    this.state = {
      minvalue: parseInt(this.props.value / 60, 10),
      secvalue: parseInt(this.props.value % 60, 10),
    };
  }

  onMinChange(e) {
    this.setState({ minvalue: Number(e.target.value) });
    this.props.onChange((Number(e.target.value) * 60) + this.state.secvalue);
  }
  onSecChange(e) {
    this.setState({ secvalue: Number(e.target.value) });
    this.props.onChange((this.state.minvalue * 60) + Number(e.target.value));
  }

  render() {
    return (
      <span>
        <input
          type="number"
          name="minutebox"
          min="0"
          max="59"
          step="1"
          style={{ width: '50px' }}
          value={this.state.minvalue}
          onChange={this.onMinChange}
        /> mins
        <input
          type="number"
          name="secondbox"
          min="0"
          max="59"
          step="1"
          style={{ width: '50px' }}
          value={this.state.secvalue}
          onChange={this.onSecChange}
        /> secs
      </span>
    );
  }
}
```


1.50 i am done with a basic mvp and am uploading it to heroku right now. just a note to self that my old instructions [here](https://github.com/sw-yx/sw-yx.github.io/blob/f0432977a8527db8895ff07b814d47105a663925/_posts/2017-03-14-Finished-Base-FCC.md) worked like a charm, in particular [this coshx labs instruction list](https://www.coshx.com/blog/2016/08/19/how-to-deploy-a-meteor-1-4-app-to-heroku/).

i will be hosting this at <https://gist-tribe.herokuapp.com>
