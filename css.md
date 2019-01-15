## ネスト構造について

### [親を参照できる `&` について](https://github.com/nju33/styleguide/blob/master/css.md#%E8%A6%AA%E3%82%92%E5%8F%82%E7%85%A7%E3%81%A7%E3%81%8D%E3%82%8B--%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)

### ネスト構造 - メディアクエリ

```ts
//* styled-components

styled.div`
  padding-left: 10px;
  
  @media screen and (min-width: 768px) {
    padding-left: 20px;
  }
`;
```

```scss
div {
  padding-left: 10px;
  
  @media screen and (min-width: 768px) {
    padding-left: 20px;
  }
}
```

以外の理由でネスト構造にはしない。

## 親を参照できる `&` について

**Sass** や **Styled Components** の場合のみですが、疑似要素と場合だけ使います。  
また、**Styled Components** の場合だけ、ルートスコープでのみ使えます。

```ts
//* styled-components

styled.form`
  & input {
    &:hover {
      /* ... */
    }
    
    &:focus {
      /* ... */
    }
  }
`;
```

```scss
form input {
  &:hover {
    // ...
  }

  &:focus {
    // ...
  }
}
```

## 変数について

最初にすべて定義します。

```scss
$button-bg-color: orange;
$button-bg-color_hover: darken($button-bg-color, 15%);

button {
  background: $button-bg-color;

  &:hover {
    background: $button-bg-color_hover;
  }
}
```

**Styled Components** の場合も、宣言の中で **polished** を使わない。

## コメントアウトについて

**Sass** の場合は、`/* ... */` じゃなく`//` 記法を使います。

### ブロック単位

`/* ... */` で全部囲むこと。

```css
/* body {
  color: orange;
} */
```

### 宣言単位

一行ずつ `/* ... */` で囲むこと。  
2 行一気にコメントアウトしない。

```css
body {
  font-size: 15px;
  line-height: 1.6;
  /* color: #292929; */
  /* background: #f8f8f8; */
}
```
