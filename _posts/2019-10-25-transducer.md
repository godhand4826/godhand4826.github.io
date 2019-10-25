---
layout: post
title:  "Transducer"
date:   2019-10-25 17:36:00 +0800
categories: fp 
---
```js
const R = require('ramda')
const { filter, reduce, map } = R
const { compose, allPass } = R
const { inc, length, includes, add } = R
const even = n => n % 2 == 0

var arr = ["cat", "bird", "pig", "dog", "fish"]
// We know that the function in mutiple map,
// we can compose to decrease the iteration.
console.log(
    map(inc, map(length, arr)),
    map(compose(inc, length))(arr)
)
// Function in mutiple filter can compose in another way
console.log(
    filter(includes('g'), filter(includes('i'), arr)),
    filter(allPass([includes('g'), includes('i')]))(arr)
)
// But it seem not works well between map and filter.
console.log(map(inc, filter(even, map(length, arr))))
console.log("something like compose(map(inc), filter(even), map(length))(arr)")

// Transducers is just composition between map and filter.

// Let's take a look at map and filter. (NOT real implementation)
// map = (f, arr) => arr.map(f)
// map :: (a -> b) -> [a] -> [b]
// f :: a -> b
//
// filter = (pred, arr) => arr.filter(pred)
// filter :: (a -> Bool) -> [a] -> [a]
// pred :: a -> Bool

// The signature is so different, how can I compose them?

// First, we know we can use reduce to implement map and filter.
// reduce = (reducer, initVal, arr) => arr.reduce(reducer, initVal)
// reduce :: (a -> b -> a) -> a -> [b] -> a
// recucer :: a -> b -> a 
const append = (arr, e) => [...arr, e]
// append :: [a] -> a -> [a]
console.log(
    map(length, arr),
    reduce((a, c) => append(a, length(c)), [], arr)
)
console.log(
    filter(includes('i'), arr),
    reduce((a, c) => includes('i')(c) ? append(a, c) : a, [], arr)
)
// Let's create some High-Order-Functions to create reducer,
// and pass it to reduce.
const mapReducer = f => (a, c) => append(a, f(c))
// mapReducer :: f -> reducer
// mapReducer :: (a -> b) -> ([b] -> [a] -> [b])
const filterReducer = pred => (a, c) => pred(c) ? append(a, c) : a
// filterReducer :: pred -> reducer
// filterReducer :: (a -> Bool) -> ([a] -> a -> [a])

// The reduce-version works well.
console.log(
    map(length, arr),
    reduce(mapReducer(length), [], arr),
)
console.log(
    filter(includes('i'), arr),
    reduce(filterReducer(includes('i')), [], arr)
)

// Surprisingly, "append" is also a reducer!!
// const append = (arr, e) => [...arr, e]
// append :: reducer
// append :: [a] -> a -> [a]
// We can extract 'append'
const mapping = f => r => (a, c) => r(a, f(c))
// mapping :: f -> reducer -> reducer'
// mapping :: (a -> b) -> (c -> b -> c) -> (c -> a -> b)
const filtering = pred => r => (a, c) => pred(c) ? r(a, c) : a
// filtering :: pred -> reducer -> reducer'
// filtering :: (a -> Bool) -> (b -> a -> b) -> (b -> a -> b)

// Still works as normal.
console.log(
    map(length, arr),
    reduce(mapping(length)(append), [], arr)
)
console.log(
    filter(includes('i'), arr),
    reduce(filtering(includes('i'))(append), [], arr)
)

// In this version, while mapping and filtering are partial applied,
// they become (reducer -> reducer') which is composable.

// Notice the order 
const xform = compose(mapping(length), filtering(even), mapping(inc))
// xform :: reducer -> reducer'
console.log(
    map(inc, filter(even, map(length, arr))),
    reduce(xform(append), [], arr)
)

// To reduce a large array, transducer is your friend.
const bigArr = new Array(100000).fill(1).map(() => Math.round(Math.random() * 10))
const xform2 = compose(filtering(even), mapping(inc))
console.time("normal")
console.log(reduce(xform2(add), 0, bigArr))
console.timeEnd("normal")

console.time("transducer")
console.log(reduce(add, 0, map(inc, filter(even, bigArr))))
```
