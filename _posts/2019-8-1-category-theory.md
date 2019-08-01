---
layout: post
title:  "Category Theory"
date:   2019-08-01 14:36:00 +0800
categories: fp 
---

## Category

Category is a labelled directed graph with associative composition and identity.

- ${f_{a\to b}  \implies  f\circ id_a = f = id_b \circ f }$
- ${g_{b→c},\ f_{a→b} \implies (g \circ f)_{a→c} }$
- ${h_{c→d},\ g_{b→c},\ f_{a→b}  \implies h \circ (g \circ f) = (h \circ g) \circ f = (h \circ g \circ f)_{a→d}}$

## Functor

Functor is a categorical strucure-preserving mapping between categoryies.

- ${x \mapsto Fx,\ f \mapsto Ff}$
- ${id_x \mapsto id_{Fx}}$
- ${g \circ f \mapsto Fg \circ Ff = F(g \circ f)}$
