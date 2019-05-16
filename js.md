## 命名規則

### ファイル名

少英数字を`-`で繋いだものを使う。

```sh
# 👍
foo-bar.tsx

# 👎
FooBar.tsx
```

`index.*`に置けるのは`import`と`export`だけ。

```js
// 👍
export {AHelper} from './a-helper';
export {Foo} from './foo';
export default Foo;

// 👎
import {AHelper} from './a-helper';

export class Foo {
	/* ... */
}
export default Foo;
```

### ディレクトリ名

中のモジュールを 1 つにするような`/index\.[tj]sx?$/`を提供する場合**単数形**、各ファイルにアクセスするような形なら**複数形**にする。

```sh
# 👍
helper
  index.ts
  foo.ts
  bar.ts

# 👍
helpers
  foo.ts
  bar.ts

# 👎
helpers
  index.ts
  foo.ts
  bar.ts

# 👎
helper
  index.ts
  foo.ts
  bar.ts
```

### 変数名

変数などは、ローワーキャメルケース (lower camel case)を使う。

```ts
// 👍
const fooVal = 'foo';

// 👎
const foo_val = 'foo';
```

クラス名、React コンポーネントな関数に限りアッパーキャメルケース (upper camel case)を使う。

```ts
// 👍
class Foo {}

// 👍
class Button = () => <button>Button</button>;


// 👎
class button = () => <button>Button</button>;
```

## コード

### 変数定義の `const` `let` は１つに１つ

```ts
// 👍
const foo = 'foo';
const bar = 'bar';

// 👎
const foo = 'foo',
  bar = 'bar';
```

### `{ ... }` は省略しない

#### `if`

```ts
// 👍
if (bool) {
  /* ... */
}

// 👎
if (bool) /* ... */
```

`for` `while` も同様。

#### `switch`

```ts
// 👍
switch (leftHand) {
  case rightHand: {
    /* ... */
    break;
  }
  default: {
  }
}

// 👎
switch (leftHand) {
  case rightHand:
    /* ... */
    break;
  default:
}
```

#### Arrow Function

かなりシンプルならあり（判断難しいけど）

```ts
// 👍
const fn = () => {
  /* ... */
}

// 👍
const fn = obj => obj.id;

// 👎
const fn = () => /* ... */
```

### `;` を付ける

```ts
// 👍
const foo = 123;

// 👎
const foo = 123;
```

[Prettier の Semicolons](https://prettier.io/docs/en/options.html#semicolons)を`all`設定にする。

### メソッド

渡すだけのものはプロパティ初期化子で定義
実行するのもは単にメソッドとして定義

```js
// 👍
class A {
  foo() {
    /* ... */
  }

  process() {
    run(this.foo());
  }
}

// 👎
class A {
  foo() {
    /* ... */
  }

  process() {
    run(this.foo);
  }
}
```

```js
// 👍
class A {
  foo = () => {
    /* ... */
  };

  process() {
    run(this.foo);
  }
}

// 👎
class A {
  foo = () => {
    /* ... */
  };

  process() {
    run(this.foo());
  }
}
```

### エラー

起こりうるエラーは最初に定義する。

```js
// 👍
const fn = () => {
  const fooError = new FooError();

  await process().then(data => {
    const result = process(data);

    if (!result.status) {
      fooError.message = 'エラーですよ';
      throw fooError;
    }
  });
}

// 👎
const fn = () => {
  await process().then(data => {
    const result = process(data);

    if (!result.status) {
      throw new FooError('エラーですよ');
    }
  });
}
```

### サイクル定数

決められた順番で取得したい場合

```js
// 👍
function* getCycle(arr) {
  while (1) {
    yield* arr;
  }
}

const xxx = () => {
  const color = getCycle(['#fff', '#ccc']);

  return items.map(item => {
    return (
      <tr key={item.id} style={{color: color.next().value}}>
        <td>{item.name}</td>
      </tr>
    );
  });
};

// 👎
const xxx = () => {
  const colors = ['#ccc', '#fff'];

  return items.map((item, i) => {
    return (
      <tr key={item.id} style={{color: colors[i % 2]}}>
        <td>{item.name}</td>
      </tr>
    );
  });
};
```

### `"` より `'`

```ts
// 👍
const foo = 'value';

// 👎
const foo = 'value';
```

[Prettier の Quotes](https://prettier.io/docs/en/options.html#quotes)を`true`設定にする。

### 1 行は`80`文字まで

(`...`は 40 文字ぐらい）

```ts
// 👍
const foo = ['fooo...', 'barr...', 'bazz...'];

// 👎
const foo = ['fooo...', 'barr...', 'bazz...'];
```

[Prettier の Print Width](https://prettier.io/docs/en/options.html#print-width)を`80`設定にする。

### `[tab]` より `[space][space]`

```ts
// 👍
const foo = [
  [space][space]'foo',
];

// 👎
const foo = [
  [tab]'foo',
];
```

[Prettier の Tab Width](https://prettier.io/docs/en/options.html#tab-width)を`2`設定にする。
[Prettier の Tabs](https://prettier.io/docs/en/options.html#tabs)を`false`設定にする。

## JSX

### オブジェクトと同じルール

属性が１つなら 1 行、それ以外なら１つずつ改行して記述。（恐らく Prettier でもそうなるのでそれに従う）

```jsx
// 👍
<button type="button">button</button>
// 👎
<button
  type="button"
>button</button>

// 👍
<button
  type="button"
  onClick={() => {/* ... */}}
>button<button>
// 👎
<button type="button" onClick={() => {/* ... */}}>button<button>
```

## コードをプッシュする前に

以下で整形・修正・確認してからプッシュします。

1. [prettier](https://github.com/prettier/prettier) で整形
2. [tslint](https://github.com/palantir/tslint) のエラーを修正
3. [tsc](https://github.com/Microsoft/TypeScript) でビルドができる状態か確認
4. テストが通るか確認
