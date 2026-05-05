---
title: "TypeScript 高级类型详解"
date: 2024-01-05
categories: [前端开发]
tags: [TypeScript, 类型系统]
excerpt: "TypeScript 的类型系统非常强大，本文详细介绍了条件类型、映射类型、模板字面量类型等高级特性，帮助你写出更安全、更优雅的类型定义。"
---

## 为什么需要高级类型

TypeScript 的类型系统非常强大，基本类型可以满足大多数场景。但在一些复杂的场景中，我们需要使用高级类型来更好地表达类型关系。

## 条件类型

条件类型允许我们根据类型关系选择不同的类型。语法类似于三元运算符：`T extends U ? X : Y`

```typescript
type NonNullable<T> = T extends null | undefined ? never : T;

type A = NonNullable<string | null>; // string
type B = NonNullable<number | undefined>; // number
```

## 映射类型

映射类型可以基于旧类型创建新类型：

```typescript
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};

type Partial<T> = {
    [P in keyof T]?: T[P];
};

type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
};

type Record<K extends keyof any, T> = {
    [P in K]: T;
};
```

## 模板字面量类型

模板字面量类型允许我们使用模板字面量语法来创建字符串类型：

```typescript
type EventName = `on${Capitalize<string>}`;
type CSSUnit = `${number}px`;
type HTTPMethod = 'GET' | 'POST' | 'PUT' | 'DELETE';

type Props = {
    onClick: () => void;
    onChange: (value: string) => void;
};

type EventHandlers = {
    [K in keyof Props as `handle${Capitalize<K>}`]: Props[K];
};
```

## infer 关键字

infer 关键字允许我们在条件类型中推断类型：

```typescript
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : any;

type A = ReturnType<() => string>; // string
type B = ReturnType<() => Promise<number>>; // Promise<number>

type Parameters<T> = T extends (...args: infer P) => any ? P : never;

type C = Parameters<(name: string, age: number) => void>; // [string, number]
```

## 分布式条件类型

当条件类型的左边是泛型时，会自动分发到联合类型：

```typescript
type ToArray<T> = T extends any ? T[] : never;

type A = ToArray<string | number>; // string[] | number[]
```

## 常用内置类型

TypeScript 提供了一些非常实用的内置类型：

```typescript
type Required<T> = { [P in keyof T]-?: T[P] };
type NonNullable<T> = T & {};
type Exclude<T, U> = T extends U ? never : T;
type Extract<T, U> = T extends U ? T : never;
type InstanceType<T> = T extends new (...args: any[]) => infer R ? R : any;
```

## 实战示例

### 获取对象中的函数类型

```typescript
type FunctionPropertyNames<T> = {
    [K in keyof T]: T[K] extends Function ? K : never;
}[keyof T];

type MixedType = {
    name: string;
    age: number;
    setName: (name: string) => void;
    setAge: (age: number) => void;
};

type FunctionProps = FunctionPropertyNames<MixedType>; // 'setName' | 'setAge'
```

### 深度只读

```typescript
type DeepReadonly<T> = {
    readonly [P in keyof T]: T[P] extends object ? DeepReadonly<T[P]> : T[P];
};
```

## 总结

掌握这些高级类型可以让你写出更安全、更优雅的类型定义。合理使用这些特性可以大大提升代码的可维护性。