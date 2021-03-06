---
layout: post
title:  "Category Theory"
date:   2019-08-01 14:36:00 +0800
categories: fp 
---

## Category

Category is a labelled directed graph with associative composition and identity.
- identity
    - ${f_{a\to b}  \implies  f\circ id_a = f = id_b \circ f }$
- composition
    - ${g_{b→c},\ f_{a→b} \implies (g \circ f)_{a→c} }$
- associative
    - ${h_{c→d},\ g_{b→c},\ f_{a→b}  \implies h \circ (g \circ f) = (h \circ g) \circ f = (h \circ g \circ f)_{a→d}}$

## Functor

Functor is a categorical strucure-preserving mapping between categoryies.
- mapping for every dot and arrow
    - ${x \mapsto F(x)}$
    - ${f \mapsto F(f)}$
- identity preserving
    - ${F(id_x) = id_{F(a)}}$
- composition preserving
    - ${F(g) \circ F(f) = F(g \circ f)}$
