---
layout: post
title:  "Promise in JS is not Functor"
date:   2019-02-08 15:50:00 +0800
categories: js 
---

## Category Theory
- Objects: The dots linked by arrows.
- Morphisms: The arrows between objects.
- Identity: The existence of an identity arrow for each object.
	- left identity: f:A→X, id<sub>X</sub>:X→X  ⇨ id<sub>X</sub>∘f = f
	- right identity: id<sub>X</sub>:X→X, g:X→B ⇨ g∘id<sub>X</sub> = g
- Compossition: The ability to compose the arrows associatively.
	- compose: f:A→B, g:B→C ⇨ g∘f:A→C
	- associative: f:A→B, g:B→C, h:C→D ⇨ h∘(g∘f) = (h∘g)∘f = h∘g∘f:A→D

## Category of sets(denoted as Set)
- Objects: Sets
- Morphisms: Total functions(defined output for all input(domain))
- Identity: Identity function(mathematics)
	- f(x)=x
- Compossition: Composition of functions
	- y=f(x), z=g(y) ⇨ z=g(f(x)), z=g∘f(x)
	
## Category in programming
- Objects: Types
	- Int, Char, String, Boolean, Double...
- Morphisms: Pure funcitons(Functions with no side-effect(can be tabulated) and returns a value)
- Identity: Identity function
	- function id(x){ return x } // Any→Any
- Compossition: Composition of functions
	- function plusTwo(x){return x+2} // Number→Number
	- function isEven(x){return x%2===0} // Number→Boolean
	- function isEven_after_plusTwo(x){return isEven(plusTwo(x))} // Number→Boolean


## Functors in category theory
Functors is a __structure-preserving__(identity, composition) transformation between categories.
- mapping object
	- X in C ⇨ F(X) in D
- mapping morphism
	- f in C ⇨ F(f) in D
- identity
	- id<sub>X</sub> in C ⇨ id<sub>F(X)</sub> in D
- composition
	- g∘f in C ⇨ F(g)∘F(f) = F(g∘f) in D


## Functors in programming(endofunctor)
Functors maps from a category to itself also called endofunctor.
Array in JS is a functor.
- mapping type
	- a ⇨ [a]
- mapping functions(fmap)
	- f:a→b ⇨ fmap(f):[a]→[b] 
	- fmap === Array.map
- identity: This law always holds because the identity function works on any type.
	- id<sub>a</sub> ⇨ id<sub>[a]</sub>
	- id === id<sub>a</sub> === id<sub>[a]</sub>
- composition
	- g∘f ⇨ fmap(g∘f) = fmap(g)∘fmap(f)
	- array.map(x=>g(f(x))) === arr.map(x=>g(x)).map(x=>f(x))

Promise in JS is not functor?
- mapping type
	- a ⇨ P<sub>a</sub>
- mapping funcitons
	- f:a→b ⇨ fmap(f):P<sub>a</sub>→P<sub>b</sub>
	- fmap === Promise.then
	- <span style="color:red">f:a→P<sub>b</sub> ⇨ fmap(f):P<sub>a</sub>→P<sub>b</sub></span> This makes composite failed !!
- identity
	- id<sub>a</sub> ⇨ id<sub>Promise<sub>a</sub></sub>
	- id === id<sub>a</sub> === id<sub>Promise<sub>a</sub></sub>
- composition
	- g∘f ⇨ fmap(g∘f) = fmap(g)∘fmap(f)
	- <span style="color:red">promise.then(x=>g(f(x)) === promise.then(x=>f(x)).then(x=>g(x))</span> Not satisfied !!

```
// The order of output might not be 
// the same as the following code!
// consider that
// f :: int -> Promise int
// f i = Promise i*2
const f1 = i => Promise.resolve(i*2)

// g :: int -> Promise int
// g i = Promise i-1
const g1 = i => Promise.resolve(i-1)

// Right hand side works.
// But on the left hand side, the composite failed 
// due to the type checking
p.then(x=>g1(f1(x)))
        .then(console.log)
        .catch(_=>console.error("LHS1 failed"))

p.then(x=>f1(x)).then(x=>g1(x))
        .then(console.log)
        .catch(_=>console.error("RHS1 failed"))

// conider that                                   
// f :: Promise i -> Promise i
// f Promise i = Promise i*2
const f2 = pi => pi.then(x=>x*2)

// g :: Promise i -> Promise i
// g Promise i = Promise i-1
const g2 = pi => pi.then(x=>x-1)

// Both sides mapping failed due to the flatten.
p.then(x=>g2(f2(x)))
        .then(console.log)
        .catch(_=>console.error("LHS2 failed"))

p.then(x=>f2(x)).then(x=>g2(x))
        .then(console.log)
        .catch(_=>console.error("RHS1 failed"))
```	

## References
- [Functor - Wikipedia](https://en.wikipedia.org/wiki/Functor)
- [What is an endofunctor? - Quora](https://www.quora.com/What-is-an-endofunctor)
- [Promise - JavaScript \| MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [javascript - Why are Promises Monads? - Stack Overflow](https://stackoverflow.com/questions/45712106/why-are-promises-monads)
- [JavaScript の Promise は モナドではない - Qiita](https://qiita.com/41semicolon/items/b75a9a26b118da72f9c7)