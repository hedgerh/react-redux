# Using Selectors
## Intro
A common pattern for accessing and computing data from state is with "Selectors."  Selectors are functions that take Redux state as an argument, and return data based on that state.  They provide a way to access state in a reusable and encapsulated manner.

Oftentimes, a piece of state needs to be used in multiple places across an app.  Rather than directly accessing that state in all of those places, you could create a selector function that returns the desired data.

```js
// instead of accessing state directly..
const mapStateToProps = state => ({
    example: state.here.is.an.example,
})

// .. use a selector!
const selectExample = state => state.here.is.an.example 
const mapStateToProps = state => ({ example: selectExample(state) })
```

The benefit to this is that it encapsulates the knowledge of where the given piece of state lives.  The data can be used in multiple places, but implementation details only ever need to be updated in one place.  Selectors are especially useful on bigger projects, where people or teams can get the data they need off of state without knowing exactly where it lives in the state.

## Improving Performance With Memoized Selectors
Selectors can also be used to improve performance.  Suppose you have to perform a computationally expensive operation:

```js
const mapStateToProps = state => {
  const { data } = state
  
  return {
    expensive: doASuperExpensiveCalculation(data),
  }
})
```
Here, the expensive operation will re-compute every time an action is dispatched that causes a store update, even though the input (and, therefore, the output) for it not changing.

To fix this, the expensive operation can be performed within what is known as a memoized selector function.  A memoized selector keeps track of the inputs it has received, as well as the computed results, and will only recompute when it has received input that is new.  Otherwise, it'll return a cached result.

To learn more, check out [Reselect](https://github.com/reduxjs/reselect), one of the most popular libraries for creating memoized selector functions.

## Links and References

**Tutorials**
- [Idiomatic Redux: Using Reselect Selectors for Encapsulation and Performance](https://blog.isquaredsoftware.com/2017/12/idiomatic-redux-using-reselect-selectors/)
