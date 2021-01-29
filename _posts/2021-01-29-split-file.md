---
layout: post
title:  "split file"
date:   2021-01-29 16:05:03 +0800
categories: linux split
---
# Split file by size
```bash
split -b 10M -d --additional-suffix=.part foo bar
cat bar*.part > foo
```
- b: split every N bytes
- a: generate suffixes of length N (default 2)
- d: use numeric suffixes
- additional-suffix=SUFFIX: append additional static SUFFIX