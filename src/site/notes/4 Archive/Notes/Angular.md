---
dg-publish: true
permalink: /4-archive/notes/angular/

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