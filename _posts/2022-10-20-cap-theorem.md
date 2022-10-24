---
layout: post
title:  "CAP theorem"
date:   2022-10-20 10:56:00 +0800
categories: cap
---
## CAP
- 網路分區容錯(Tolerance towards Network Partition)
- 一致性(Consistency)
- 可用性(Availability)

## 三項最多取兩項 (實務上並非全有全無，而是傾向)
CA without P: 系統在網絡分區的情況下，存在未定義的行為。
AP without C: 不保證數據一致性。(使用樂觀鎖，並處理不一致所造成的conflict)
CP without A: 數據只能在保證一致性的情況下才能被使用。(使用悲觀鎖等待所有節點一致)

## 近似(但仍可以在不同層級交疊使用)
CA: replica (一份資料複製多份放在各個節點)
- 在網路分區的情況下，各節點仍然可用，但一致性是未定義的)
CP: sharding (特定資料只放在特定節點)
- 節點壞掉時部分資料不可用
AP: 綜合

## References
- [Brewer’s CAP Theorem](https://edisciplinas.usp.br/pluginfile.php/2541318/mod_resource/content/1/TeoremaDeBrewer.pdf)
