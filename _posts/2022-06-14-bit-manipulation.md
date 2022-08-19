---
layout: post
title:  "Bit manipulation"
date:   2022-06-14 18:48:44 +0800
categories: leetcode algorithm
---
- power of 2
  - `!(x & (x-1)`
- least significant 1 bit
  - `x & -x`
- bitset
  - add: `bitset |= 1 << x`
  - has: `(bitset >> x) & 1`
  - remove: `bitset &= ~(1 << x)`
  - union: `bitsetA | bitsetB`
  - intersection: `bitsetA & bitsetB`
  - difference: `bitsetA - (bitsetA & bitsetB)`
  - iterate non-empty subset (reversed):
   `for(let subset = bitset; subset > 0; subset = (subset - 1) & bitset) { ... }`