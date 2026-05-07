# Why `unknown` is Safer Than `any` in TypeScript

## Introduction

In TypeScript, when we don't know the type of a value, we might reach for `any`. But there's a better option: `unknown`. This post explains the difference and why `unknown` is the safer choice.

---

## The Problem with `any`

When you use `any`, TypeScript basically turns off type checking for that value. You can do anything with it — call it as a function, access properties, assign it anywhere — and TypeScript won't complain.

```typescript
let value: any = "hello";

value.toUpperCase(); // OK
value();            // OK — but this will crash at runtime!
value.foo.bar.baz; // OK — but totally unsafe
```

This is why `any` is called a **"type safety hole"** — it lets bugs slip through that TypeScript is supposed to catch.

---

## `unknown` — The Safe Alternative

`unknown` also accepts any value, but TypeScript **won't let you use it** until you check what type it actually is.

```typescript
let value: unknown = "hello";

value.toUpperCase(); //Error! TypeScript refuses this.
```

You have to narrow the type first:

```typescript
if (typeof value === "string") {
  value.toUpperCase(); //Now TypeScript is happy
}
```

---

## What is Type Narrowing?

Type narrowing means checking the type of a value so TypeScript can figure out what it is inside that block.

```typescript
function printValue(value: unknown): void {
  if (typeof value === "string") {
    console.log("String:", value.toUpperCase());
  } else if (typeof value === "number") {
    console.log("Number:", value.toFixed(2));
  } else {
    console.log("Unknown type");
  }
}
```

Inside each `if` block, TypeScript knows the exact type and gives you the correct autocomplete and safety.

## Conclusion  

Use `unknown` instead of `any` when you're handling data you're not sure about — like API responses or user input. It forces you to check the type before using the value, which makes your code much safer. 