# Bootcamp Assessment: CP01 - Debugging

<div align="right">
  <h4>Cohort: Ngahuru 2026</h4>
  <h4>Student Name: Young Ryou</h4>
</div>

---

## Part One: Independent Debugging

I will discuss the issue that occurred during the **TDD Bowling Kata** challenge, the solution applied, and the outcomes of this process. I felt confident since I had already completed a previous **Bowling Kata** challenge that was a similar version of this challenge. Nevertheless, I encountered a problem during the `scores a single strike frame` testing process.

### The Bug

The bug occurred during the `scores a single strike frame` test after having passed the `scores a spare frame test`. I coded the *strike logic* to pass the newly created test code. After that, I ran the `npm test`. While the `single strike frame` test passed, the previously successful `spare frame` test suddenly failed.

### My Process

To identify the issue, I read the `npm test` logs. According to the logs, the expected value for the `scores a spare frame test` was **13**, but the actual value was **10**, resulting in a failure.

To pinpoint the failure point, I temporarily added the strings `error1` and `error2` to the return values of the `if (isSpare(frame))` and `else` (normal) statements in the `scoreFrame` function. I found that the test, intended to be processed as a *spare*, was being calculated as a *normal* frame.

Based on this, I inferred that the function was returning `false` in the `if` statement, which should have been recognised as a *spare*. Therefore, I inspected the `isSpare` function, which I had modified while writing the *strike logic*.

```js
// in `isSpare` function
// Before
return frame[0] + frame[1] === 10
// After (with bug)
return !isStrike && frame[0] + frame[1] === 10
```

Since the only part I had changed was `!isStrike`, I suspected this was the most likely source of the problem and realised that the **argument** was missing.

### The Outcome

After realising the argument was missing, I changed the code to `!isStrike(frame)`, which resolved the issue. I realised that the core of the problem was a missing **invocation**. I had merely referenced the function instead of calling it. I felt that while this is a common mistake, it could be difficult to pinpoint the issue.

By utilising the **TDD** approach and intentionally inserting messages into the actual values, I was able to find the point of failure. This allowed me to confirm the value of information provided by test messages in a **TDD** environment.

---

## Part Two: Collaborative Debugging

I will discuss the issue that occurred during the `kata-data-structures` challenge, the solution applied, and the outcomes of this process. I was working with **Parkie** through a *pair programming* session. We encountered a problem during one of the tasks that was implementing the `getPropTypes.js` function.

### The Bug

The bug occurred while we were trying to implement the logic for the `getPropTypes` function, which is to return an array of types for each property value in an object. Therefore, I tried to iterate through the object using `.forEach()` to push the types into a new array. However, when running the `npm test`, the test failed with a `TypeError: obj.forEach is not a function`.

```js
// getPropTypes.js
export default function getPropTypes(obj) {
  const newArr = []
  // Error Point
  obj.forEach((type) => {
    newArr.push(typeof type)
  })
  return newArr
}
```

### The Collaboration

We worked together by following our specific roles *(Driver or Navigator)* in the pair programming setup and switching the role rotationally based on the time or functions.

During this part, I (Young) took the role of the **Driver**, focusing on the implementation and typing of the code, while Parkie acted as the **Navigator**, observing the overall logic and checking for errors.

When the `TypeError` appeared, we kept discussing how to solve this error. I shared my confusion about why the iteration was failing, and Parkie, as the Navigator, checked the test file and reviewed the data structure we were handling. We tried to solve the error together, and finally, we could focus on the fundamental data types we were working with.

### The Debugging Techniques

We utilised three specific techniques to identify and solve the problem:

1. **Reading Error Messages**: We analysed the terminal output where the `TypeError: obj.forEach is not a function` message. It was a critical point for identifying a **data type** error rather than a syntax error or others.

2. **Verbalising (Rubber Ducking)**: I explained my logic line-by-line to Parkie. In the middle of explaining that I was *"looping through the object"*, I caught my conceptual mistake of treating an object as an array.

3. **Referencing Documentation**: We read the **MDN Web Docs** for the `forEach()` function, and checked when and how to use it. We confirmed that objects are not directly iterable using the `.forEach()` function and found `Object.values()` as the correct solution to this error.

### The Outcome

The issue was resolved by using `Object.values(obj)` to extract the values into an array, then using the `.forEach()` function to push the types of each value into `newArr`. This fix allowed the test to pass successfully.

```js
// Before (with bug)
  obj.forEach((type) => {
    newArr.push(typeof type)
  })

// After (solved)
  Object.values(obj).forEach((type) => {
    newArr.push(typeof type)
  })
```

Through this process, I learned that collaborative debugging is highly useful. By sharing thoughts or ideas and communicating with each other, we were able to identify and learn from our own or each other's errors. This allowed us to solve problems faster than working alone, while solidifying my understanding of the core concepts.

---

## Requirements

- [x] I have described a bug I debugged independently - the problem and my debugging process
- [x] I have described a bug I debugged collaboratively - the problem, how we worked together, and our process
- [x] My descriptions show specific debugging techniques, not just "I fixed it"
- [x] Submit: Written or recorded descriptions of both debugging experiences
- [x] Declaration: I confirm that the content submitted is my original work

