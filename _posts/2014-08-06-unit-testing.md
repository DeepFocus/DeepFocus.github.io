---
layout: post
title: "Tech Breakfast #19 - Unit Testing and Dependency Injection Techniques"
category: articles
tags: [unit testing, dot.net]
image:
  feature: unit-testing.png
---

##Unit Testing and Dependency Injection Techniques

We’ll use Asp.Net to make it more real Also:

- StructureMap for IoC/DI.
- NSubstitute for faking dependencies.
- NLog as a 3rd party dependency example.

Render a page / partial / widget:

- Downloads latest currency rate from web service.
- Serves a cached data before it expires.
- If a web service call fails, log an error (possibly by email)
- Should return null model if unable to get result.
(view will render N/A state)

We will refactor our code to make it testable. 

If you want to play with code use this [repo](https://github.com/aboichev/UnitTestingDemo)


{% include speakerdeck.html deckId="a8aed940ffb70131a0065a62ea9b4ee9" %}
