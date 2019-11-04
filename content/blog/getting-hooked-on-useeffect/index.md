---
title: Getting Hooked on useEffect()
date: "2019-11-03T22:21:30+00:00"
description: "Batter up! Use the Effect Hook to heckle batters while counting balls, strikes, walks, strikeouts and full counts."
ogimage: "./strikeout.jpg"
---

[React Hooks](https://reactjs.org/docs/hooks-intro.html) were introduced a year ago and fundamentally changed the development of React components. State, once tied strictly to [class components](https://reactjs.org/docs/react-component.html), became available to stateless components via the useState() hook, and earned them their new moniker, _functional_ components.

Working hand-in-hand with **useState()** is the **useEffect()** hook. The Effect hook replaces all of what **componentDidMount**, **componentDidUpdate**, and **componentWillUnmount** did for us in class components. Coincidentally, there are three different ways to use the Effect hook that we'll learn about below.

## Code Along

We're going to implement **useState()** and **useEffect()** in a very simple baseball app. It's going to count balls and strikes, tell us whether a batter walks or strikes out (nobody ever hits in this game), alert us of a full count (3 balls and 2 strikes), and heckle our batters in the console.

First, create a new React app with `npx create-react-app effect-hook-app` or pull the whole repo from here: [https://github.com/keithburgie/effect-hook-baseball](https://github.com/keithburgie/effect-hook-baseball)

Then import React and include our State and Effect hooks.

~~~javascript{numberLines: true}
import React, { useState, useEffect } from "react";

function App() {

  const [count, setCount] = useState({
    strikes: 0,
    balls: 0
  });

  const [fullCount, setFullCount] = useState(false);

  return (
    <div>
      <h1>{count.balls} balls - {count.strikes} strikes </h1>
      <button onClick={throwPitch}>Throw Pitch</button>
      {fullCount && <span>Full count!</span>}
    </div>
  );
}

export default App;
~~~

## useState()
Convention for useState() is a deconstructed array. The first word is the variable we will use to access our state, and the second word is a function to update that state. Convention is to use "set" +"myState".

Whatever we put inside useState's parentheses will be the default state. It can be any data type or data. Above, we've used a nested object for **count**, the same way we would in the state of a class component, and a boolean false for **fullCount**.

~~~javascript{numberLines: true}
const [state, setState] = useState('') // empty string
const [state, setState] = useState([]) // empty array
const [state, setState] = useState({}) // empty object
const [state, setState] = useState(0) // a number
const [state, setState] = useState(false) // a boolean
~~~

Now we need a few functions to update our pitch count. We'll make use of the Effect hook to actually call these functions. You'll notice that **setCount()** looks a lot like **this.setState()** from a class component. 

~~~javascript{numberLines: true}
  function throwPitch() {
    // Returns a 0 or 1
    Math.floor(Math.random() * 2) ? strike() : ball();
  }

  function strike() {
    // Use setCount to update count.strikes
    setCount({ 
      ...count, [count.strikes]: count.strikes++ 
    });
  }

  // Use setCount to update count.balls
  function ball() {
    setCount({ 
      ...count, [count.balls]: count.balls++ 
    });
  }

  // After a walk or strikeout, reset both states
  function resetCount() {
    setCount({ strikes: 0, balls: 0 });
    setFullCount(false)
  }
~~~

## useEffect()

The Effect hook runs on [side effects](https://en.m.wikipedia.org/wiki/Side_effect_(computer_science)). In our use case, a re-render is a side effect. So, every time our state changes, and the code inside our return statement renders, it will call **useEffect()**.

There are three ways to write this function:

~~~javascript{numberLines: true}
useEffect(() => {}, [])
useEffect(() => {}, [state])
useEffect(() => {})
~~~

\#1 will run only once. It works like componentDidMount.
\#2 will run any time there is a change to the included state.
\#3 will run on _any_ side effect. It will fire on _every_ render.

To make use of the first one, let's welcome ourselves to the game. A great real world use-case for this version would be to call an API and set returned data as our initial state.
~~~javascript{numberLines: true}
  useEffect(() => {
    window.alert("Take me out to the ball game...");
  }, []);
~~~

We'll use version \#2 to watch our pitch count. Every time count updates, this function will run our checks. If any of the statements return true, they will run.
~~~javascript{numberLines: true}
  // Called on changes to count
  useEffect(() => {
    if (count.balls === 4) {
      window.alert("Batter walks.");
      resetCount();
    }
    if (count.strikes === 3) {
      window.alert("Strikeout!");
      resetCount();
    }
    if (count.balls === 3 && count.strikes === 2) {
      setFullCount(true)
    }
  }, [count]);
~~~

### Important! 

At this point I have to call attention to our use of **fullCount** and **setFullCount**. It seems like I could have just added a "full" property to the **count** state. If we did that, we'd have trouble. 

**Using **useEffect()** to update the state it is watching will cause an infinite loop.** 

If on 3 balls and 2 strikes we were to update a property called **count.full**, the same **useEffect()** function would be called and the same state would be updated over and over.

Finally, let's use the last example to heckle batters. With every pitch, we'll log a heckle to the console. In cases where there are two events, such as a strikeout, walk, or full-count, it will log two messages because it will run twice.
~~~javascript{numberLines: true}
useEffect(() => {
    const heckles = [
      "Hey batta batta batta swing batta batta!",
      "Everybody move in!",
      "You're getting less hits than an Amish website!",
      "I've seen better cuts at a deli!",
      "I've seen better bats in a cave!",
      "No Batter, No Batter...",
      "Didn't you just sell me a hotdog?"
    ]
    console.log(heckles[Math.floor(Math.random() * heckles.length)])
  });
~~~

All together now, with comments, a little styling, and the clearing of heckles between each pitch:
~~~javascript{numberLines: true}
import React, { useState, useEffect } from "react";

function App() {
  // Initial state = {strikes: 0, balls: 0}
  const [count, setCount] = useState({
    strikes: 0,
    balls: 0
  });

  // Initial state = false
  const [fullCount, setFullCount] = useState(false);

  // Returns a 0 or 1
  function throwPitch() {
    // 1 = true = strike()
    // 0 = false = ball()
    Math.floor(Math.random() * 2) ? strike() : ball();
  }

  function strike() {
    // Add one to strike count
    setCount({ ...count, [count.strikes]: count.strikes++ });
    console.clear()
  }

  function ball() {
    // Add one to ball count
    setCount({ ...count, [count.balls]: count.balls++ });
    console.clear()
  }

  function resetCount() {
    // Reset everything after walk or strikeout
    setCount({ strikes: 0, balls: 0 });
    setFullCount(false)
    console.clear()
  }

  // Called once on page load, like componentDidMount()
  useEffect(() => {
    window.alert("Take me out to the ball game...");
  }, []);

  // Called on changes to [count]
  useEffect(() => {
    if (count.balls === 4) {
      window.alert("Batter walks.");
      resetCount();
    }
    if (count.strikes === 3) {
      window.alert("Strikeout!");
      resetCount();
    }
    if (count.balls === 3 && count.strikes === 2) {
      setFullCount(true)
    }
  }, [count]);

  // Called on ALL renders
  useEffect(() => {
    const heckles = [
      "Hey batta batta batta swing batta batta!",
      "Everybody move in!",
      "You're getting less hits than an Amish website!",
      "I've seen better cuts at a deli!",
      "I've seen better bats in a cave!",
      "No Batter, No Batter...",
      "Didn't you just sell me a hotdog?"
    ]
    // Strike or ball logs one heckle
    // Strikeout, walk, or full count logs two heckles
    console.log(heckles[Math.floor(Math.random() * heckles.length)])
  });

  return (
    <div>
      <h1>{count.balls} balls - {count.strikes} strikes</h1>
      <button onClick={throwPitch}>Throw Pitch</button>
      {fullCount &&
        <em style={{ 
            color: "red", marginLeft: "10px", fontWeight: "bold" 
        }}>Full count! (very exciting)</em>}
    </div>
  );
}

export default App;
~~~