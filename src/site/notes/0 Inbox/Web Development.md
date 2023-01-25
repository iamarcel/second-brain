---
{"dg-publish":true,"permalink":"/0-inbox/web-development/"}
---

# Frameworks and Tools

## [Remix](https://remix.run/)
> Remix is a full stack web framework that lets you focus on the user interface and work back through web standards to deliver a fast, slick, and resilient user experience. People are gonna love using your stuff.

Incredibly fast. Progressive enhancement in the React space.

Runs distributed. Uses the Fetch API.

The layout is defined by *nested routes* which means that all required API calls are known at the beginning instead of needing to wait for each nested component to be rendered. That means all requests can happen in parallel.

Add in prefetch link tags and it's already loaded beforehand.