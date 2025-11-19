---
layout: news_item
title: 'Ainakan 17.0.4 Released'
date: 2025-05-22 21:39:44 +0200
author: oleavr
version: 17.0.4
categories: [release]
---

This release brings improvements to the Compiler implementation, underpinning
the ainakan-compile CLI tool, part of ainakan-tools. Here are the changes:

- Upgraded to **ainakan-compile 18**, now with TypeScript 5.8.3, latest ainakan-fs,
  etc.
- Fixed asset bundling logic on Windows, where virtual paths were stored with
  the wrong path separator.
