## ユーティリティやヘルパーなどから付与される、値は100%渡ってくるが自分からは渡さない引数については Partial 化した後、１行で書く

```ts
const myFu = (foo: string) => { };
const decorate = (fn1: (foo: string) => void) => {
    /**
     * bar はモジュール側で付けられた引数やパラメーター
     */
    return (fn2: (foo: string, bar?: number) => void) => {
        return ((...args: [string, number]) => {
            fn2(...args);
        }) as typeof fn2
    }
};

// 👍
decorate(myFu)((foo, bar) => {
    if (bar === undefined) throw new TypeError('expect bar is number');

    const numberBox: number = bar;
})('foo');

// 👎
decorate(myFu)((foo, bar) => {
    const numberBox: number = bar!;
})('foo');
// 👎
decorate(myFu)((foo, bar) => {
    if (bar === undefined) {
      throw new TypeError('expect bar is number');
    }

    const numberBox: number = bar;
})('foo');
```

## 長いインターフェース名の物はそのファイルでのみ`type`に置いて使う

３つ以上の単語からなるインターフェースが対象

```ts
// 👍
export interface FooBarBazQuxElement {
  /* ... */
}
export type Element = FooBarBazQuxElement;

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

## 下位成果物（ライブラリ）では namespace は使わない

Babel 関連でハマる可能性がある為（最新の [Babel](@babel/plugin-transform-typescript) では対応済みだが、未対応だった時期がある）。

最上位（例えばアプリケーション本体など）では使用可。

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
export namespace ModuleName {
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
