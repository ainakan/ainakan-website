---
layout: news_item
title: 'Ainakan 16.0.16 Released'
date: 2023-04-19 13:37:37 +0200
author: oleavr
version: 16.0.16
categories: [release]
---

Seeing as TypeScript 5.0 was released last month, and ainakan.Compiler was still
at 4.9, we figured it's time to upgrade it. So with this release we're now
shipping 5.0.4. The upgrade also revealed a couple of bugs in our V8-based
runtime, and that our embedded ainakan-gum typings were slightly outdated.

Enjoy!

### Changelog

- gumjs: Fix task deadlock during V8 Isolate teardown.
- gumjs: Fix undefined behavior during V8 snapshot creation.
- compiler: Bump ainakan-compile.
- compiler: Use the proper @types/ainakan-gum.
