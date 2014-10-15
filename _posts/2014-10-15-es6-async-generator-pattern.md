---
layout: post
title: "Tech Breakfast - ECMAScript 6 Async pattern using generator functions“ 
category: articles
tags: [scala]
image:
  feature: async-all-things.jpeg
---

So how do generators help node's callback hell? Generator functions can suspend execution with the yield keyword, and pass values back and forth when resuming and suspending. This means that we can "pause" a function when it needs to wait on the result of a function, instead of passing a callback to it.

Isn't it fun trying to explain a language construct in English? How about we just dive in.

{% include speakerdeck.html deckId="c75c59f036ac0132564a062a9a25abfc" %}