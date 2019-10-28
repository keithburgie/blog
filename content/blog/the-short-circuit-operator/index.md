---
title: The Short-Circuit Operator && Why It's Great for JSX
date: "2019-10-27T22:21:30+00:00"
description: "Times have gotten so tough that the logical AND operator had to go out and get another job as the best damn IF statement you've ever seen."
ogimage: "./short-circuit.png"
---

React applications are rife with opportunities for conditional rendering. If a user is logged in, send them to this route. If this action is called, use this reducer. If the data exists, render this list.

Outside of a component's return function, anything goes. Inside that statement though, in your JSX, you can no longer use if/then or switch statements and must rely on the ternary statement:

```javascript{numberLines: true}
return (
  <div>
    // thing_is_true ? do_if_true() : do_if_false()

    { boolean === true
      ? <p>Say yes!</p>
      : <p>Say no.</>
    }

    { data.isLoaded()
      ? <Component data={data} />
      : <img src="loading.gif" />
    }
  </div>
)
```

I personally love the ternary statement, but, as concise as it is, sometimes it's still more than we need. It requires an "else" clause that we don't always have. In those cases, it's common to see something like this:

```javascript{numberLines: true}
return <div>{state.items > 0 ? <List items={items} /> : ""}</div>
```

If else, do nothing? Seems like we don't really need that else.

## Enter, the Short-Circuit Operator

You're likely familiar with the JavaScript logical operators `&&`, `||`, and `!`. And, Or, and Not.

```javascript{numberLines: true}
let i = 5

// Example A
if (i > 4 && i < 6) {
  return true
}

// Example B
if (i > 6 || i < 7) {
  return true
}
```

In Example A, `i` must be greater than 4 or the function will return false. It will never test `i < 6`.

In Example B, `i` is not greater than 6, and would return false in example A. Using the `||` operator allows it to test the right side of the statement, and return true as `i` is less than 7.

It's with this prior knowledge that the **short-circuit operator** may cause confusion the first time you see it. It's actually extremely simple.

```javascript{numberLines: true}
return (
  <div>
    // there_is_data && print_that_data()
    {data.isLoaded() && <Component data={data} />}
  </div>
)
```

The short-circuit operator: if the left side is true, do what's on the right. It is the if/then statement to the ternary's if/then/else.
