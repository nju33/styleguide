## ファイル名

少英数字を`-`で繋いだものを使う。

```sh
# 👍
foo-bar.tsx

# 👎
FooBar.tsx
```

## コード

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
  default: {}
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

## コードをプッシュする前に

以下で整形・修正・確認してからプッシュします。

1. [prettier](https://github.com/prettier/prettier) で整形
2. [tslint](https://github.com/palantir/tslint) のエラーを修正
3. [tsc](https://github.com/Microsoft/TypeScript) でビルドができる状態か確認
4. テストが通るか確認
