---
{"dg-publish":true,"permalink":"/4-archive/notes/angular/"}
---

Tags:: [[3 Resources/Programming\|Programming]]

# Change Detection

## Zone Pollution

When using non-Angular elements, e.g., charts, **initialize them outside of the Angular zone**. That way their event handlers won’t trigger Angular change detection, as Angular patches the event handlers.

## Out of Bounds Change Detection

When an event within a component with OnPush occurs, Angular will detect that component for changes even if it hasn’t received new inputs.

To increase performance, refactor child components to sibling components.

## Recalculation of referentially transparent expressions

Referentially transparent :: if an expression in a template can be changed with its value when its values don’t change.

This means we don’t have to recalculate if parameters don’t change.

**Create a pipe** to fix this. It’s `pure: true` by default but for readability it’s nice to add it to the `@Pipe` declaration.

# Reactivity
## What is Reactivity?
#wip

## Guidelines
### Avoid Subscribing
Avoid the `subscribe` method as much as possible. In Angular, you can almost always use the `AsyncPipe` instead.

### Avoid Callbacks
Something like an `onClickButton` method in the component invites imperative code, so avoid it altogether. Instead, just call `next()` on a stream collecting the button clicks.

### Always Use `OnPush`
> Marcel Samyn's First Law of Angular Rendering Performance
> *"Every component that doesn't use OnPush is fucked."*
> – Marcel Samyn

## Organizing Side Effects into a Single Observable
ProTip: organize all side effects as `tap` operators in a stream, so they're "hidden away in reactivity." Then you can subscribe to all of them at once with a single `takeUntil`:

```typescript
merge([
  renderValue$,
  updateState$
]).pipe(takeUntil(destroy$)).subscribe()
```

Then you have a *single subscription* in your file and all the side effects grouped together.

# Performance Tips
## Bootstrap in Provider
Often we do a bootstrapping network call when the relevant component first renders. This can take a while, so write a provider that starts the loading on the relevant service already.