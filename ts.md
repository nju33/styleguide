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
