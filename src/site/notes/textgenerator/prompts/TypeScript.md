---
{"dg-publish":true,"permalink":"/textgenerator/prompts/type-script/"}
---

tags:: [[3 Resources/Programming\|Programming]]

# Features
## String Template Literal Types
```ts
endpoint: `http://api.app.com/user/${number}`
```

## Function Overloads
- Define multiple implementations
- Define the overlapping types
- Implement the method

## Conditional Types
```ts
type Return<T> =
  T extends "a" ? A :
  T extends "b" ? B : 
  never;
```