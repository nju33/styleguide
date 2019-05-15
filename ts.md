## 長いインターフェース名の物はそのファイルでのみ`type`に置いて使う

３つ以上の単語からなるインターフェースが対象

```ts
// 👍
export interface FooBarBazQuxElement {
  /* ... */
}
type Element = FooBarBazQuxElement;

export interface HogeFugaPiyoObject {
  element: Element
}

export interface HogeFugaPiyo2Object {
  element: Element
}

// 👎
export interface FooBarBazQuxElement {
  /* ... */
}

export interface HogeFugaPiyoObject {
  element: FooBarBazQuxElement
}

export interface HogeFugaPiyo2Object {
  element: FooBarBazQuxElement
}

```

## ソースでは namespace は使わない

`babel`関連でハマる可能性がある為

```ts
// 👍
interface AElement {
  id: string
}

interface Foo extends AElement {
  id: 'foo';
}

interface Bar extends AElement {
  id: 'bar';
}

interface Baz extends AElement {
  id: 'baz'
}

export interface ModuleName {
  AElement: AElement;
  Foo: Foo;
  Bar: Bar;
  Baz: Baz
}

const bar: ModuleName['Bar'] = {id: 'bar'};

// 👎
namespace ModuleName {
  export interface AElement {
    id: string
  }

  export interface Foo extends AElement {
    id: 'foo';
  }

  export interface Bar extends AElement {
    id: 'bar';
  }

  export interface Baz extends AElement {
    id: 'baz'
  }
}

const bar: ModuleName.Bar = {id: 'bar'};
```
