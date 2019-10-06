---
title: De-constructor({ing}) React Class Components
date: "2019-10-05T20:00:00+00:00"
description: "This week, my mission was to understand why React's documentation is loaded with examples of class components that use a constructor."
---

This week, my mission was to understand why [React's documentation](https://reactjs.org/docs/handling-events.html) is loaded with examples of class components that use a constructor, when my own experience to date had suggested the the constructor was just extra code. I have searched StackOverflow and asked other developers to explain it to me but had yet to be convinced.

In JavaScript, the `constructor()` method is used to create and initialize an object within a class. In React, it is often used as the space to set initial state and to bind other class methods.

The constructor can make use of the `super()` method to inherit from the constructor of its parent class, which, in a React class component, is `React.Component` itself. We can call `props` within both methods to make use of props sent by the component that is calling upon it.

Then, we can use `this` to denote the instance of our object, set its state, and bind methods. All together, we get the very familiar looking code below:

```javascript{numberLines: true}
class Example extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      data: props.data,
      boolean: false,
    }
    this.toggleBoolean = this.toggleBoolean.bind(this)
  }

  toggleBoolean() {
    this.setState({
      boolean: !this.state.boolean,
    })
  }

  render() {
    return <button onClick={this.toggleBoolean}>{this.state.data}</button>
  }
}
```

I have a few problems with this code:

1. It's redundant to set our state from passed props when we can use those props directly as `this.props.data` on line 20.
2. We will never update `this.state.data` from within this class, because it will always be overwritten by props.
3. We can eliminate the confusing `this.bind(this)` syntax with an arrow function on line 11.
4. `this` returns exactly the same object without a constructor, because React uses [the public class fields syntax](https://babeljs.io/docs/en/babel-plugin-proposal-class-properties).

Shortened up, it looks like this, and works exactly the same:

```javascript{numberLines: true}
class Example extends React.Component {
  state = {
    boolean: false,
  }

  toggleBoolean = () => {
    this.setState({
      boolean: !this.state.boolean,
    })
  }

  render() {
    return <button onClick={this.toggleBoolean}>{this.props.data}</button>
  }
}
```

In this small example we've saved 5 lines, or about 20% of our code by removing the constructor and using an arrow function. Now, with such a simple state and only one method, we may as well just switch to a functional component with the [React Hook](https://reactjs.org/docs/hooks-intro.html), `useState()`.

```javascript{numberLines: true}
const Example(props) {
  const [boolean, setBoolean] = React.useState(false)

  return (
    <button onClick={() => setBoolean(!boolean)}>
      {props.data}
    </button>
  )
}
```

This code works exactly the same as the code at the top, but it's easier to read and one-third the length.

I'm not comfortable saying that the `constructor()` is useless, because React has it all over their own docs, and those guys are smart, but after a full day of coding and testing, even using _bad_ practices and trying to break things, I can't find a reason to use it.

Turns out, [@donavan](https://twitter.com/donavon) agreed with me two years ago, and he's got a single-name Twitter handle like Oprah: [The constructor is dead, long live the constructor!](https://hackernoon.com/the-constructor-is-dead-long-live-the-constructor-c10871bea599)
