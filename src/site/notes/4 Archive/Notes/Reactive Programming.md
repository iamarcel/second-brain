---
{"dg-publish":true,"permalink":"/4-archive/notes/reactive-programming/"}
---

up:: [[3 Resources/Programming\|Programming]]
tags:: RxJS [[4 Archive/Notes/Angular\|Angular]]

![everything is a stream](https://camo.githubusercontent.com/fdfeeb9e9714561310b7779fcc1ea637cdac16a7d3683bdd8ea633a3f05d7c0c/687474703a2f2f692e696d6775722e636f6d2f4149696d5138432e6a7067)

Everything is a stream of events.
Events are emitted asynchronously.
An **observer** listens to a stream by **subscribing** to it.

Streams are **immutable**.
Streams can be transformed into other streams.
This means computational dependencies are very explicit.

Why?

> Reactive Programming raises the level of abstraction of your code so you can focus on the interdependence of events that define the business logic, rather than having to constantly fiddle with a large amount of implementation details. Code in RP will likely be more concise.

A Promise is just an Observable with only a single emitted value.