# How `Pick` and `Omit` Keep Your TypeScript Code DRY

## Introduction

In TypeScript, you'll often have one big interface and need smaller versions of it in different places. Instead of writing new interfaces from scratch (which leads to repetition), you can use `Pick` and `Omit` to create "slices" of an existing interface.

---

## The Problem — Repeating Yourself

Imagine you have a `User` interface:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
  createdAt: Date;
}
```

Now you need a type for a public profile (no password), and another for a login form (just email and password). Without utility types, you'd write separate interfaces:

```typescript
// Repeating properties — not DRY!
interface PublicProfile {
  id: number;
  name: string;
  email: string;
}

interface LoginForm {
  email: string;
  password: string;
}
```

If `User` ever changes, you have to update all these manually. That's messy.

---

## `Pick` — Take Only What You Need

`Pick<Type, Keys>` lets you select specific properties from an existing type.

```typescript
type LoginForm = Pick<User, "email" | "password">;

// Result:
// {
//   email: string;
//   password: string;
// }
```

Now `LoginForm` is always in sync with `User`. If the type of `email` changes in `User`, `LoginForm` updates automatically.

---

## `Omit` — Remove What You Don't Need

`Omit<Type, Keys>` is the opposite — it keeps everything *except* the keys you specify.

```typescript
type PublicProfile = Omit<User, "password">;

// Result:
// {
//   id: number;
//   name: string;
//   email: string;
//   createdAt: Date;
// }
```

This is great when you want most of the interface but need to exclude sensitive or irrelevant fields.

---

## A Real-World Example

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  stock: number;
  description: string;
}

// For displaying a product card — no stock info needed
type ProductCard = Pick<Product, "id" | "name" | "price">;

// For updating a product — no id needed (it comes from the URL)
type ProductUpdate = Omit<Product, "id">;
```

## Conclusion

`Pick` and `Omit` help you follow the DRY principle — Don't Repeat Yourself. Instead of copy-pasting interfaces and changing a few properties, you derive new types from a single source. When the master interface changes, all derived types stay up to date automatically.
