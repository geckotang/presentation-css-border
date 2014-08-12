## borderの書き方論

[@GeckoTang](http://twitter.com/GeckoTang)

---

### borderプロパティの書き方どうしてますか

---

### 無難にこう

```html
<div class="block">The quick brown fox jumps over the lazy dog.</div>
```

```css
.block {
  border: 1px solid tomato;
}
```

ショートハンド使って全方位指定する。[fiddle]()

---

### では、一部打ち消すときはどうしますか

---

### 方法1. 全指定後、一部消す(1)

```html
<div class="block">The quick brown fox jumps over the lazy dog.</div>
```

```css
.block {
  border: 1px solid tomato;
  border-bottom: 0;
}
```

とかよくやるし [fiddle](http://fiddle.jshell.net/geckotang/gd7qnswt/11/show/light/)

```css
.block {
  border: 1px solid tomato;
  border-bottom: none;
}
```

とかもよくやってた [fiddle](http://fiddle.jshell.net/geckotang/gd7qnswt/12/show/light/)

------

### ショートハンドでの0とnoneの違い

border: 0;とborder: none;は違います。

#### border-bottom-width: 0;扱い

```css
.block {
  border: 0;
}
```

#### border-bottom-style: none;扱い

```css
.block {
  border: none;
}
```

<hr>
border-(top || right || bottom || left)とかも同じ扱いです

参考： [border:0 と border:none は何が違うのか](http://unformedbuilding.com/articles/border-0-or-border-none/)

------

### そしてIE7においてnoneは危険

もともとborderを持ってるようなinput要素とかの場合

```html
<input class="block">
```

```css
.block {
    border-bottom: none;
}
```

したときに、border-bottomは消えません。 [fiddle]((http://fiddle.jshell.net/geckotang/gd7qnswt/5/show/light/)

---

### 方法2. 全指定後、一部消す(2)

```html
<div class="block">The quick brown fox jumps over the lazy dog.</div>
```

```css
.block {
  border: 1px solid tomato;
  border-bottom-width: 0;
}
```

ちゃんとborder-bottom-widthと明示する。

---

### 方法3. 必要な部分しか指定しない(1)

```html
<div class="block">The quick brown fox jumps over the lazy dog.</div>
```

```css
.block {
  border-top: 1px solid tomato;
  border-right: 1px solid tomato;
  border-left: 1px solid tomato;
}
```

うーん...色とか線種が同じなら、これはめんどい。

---

### 方法4. 必要な部分しか指定しない(2)

```html
<div class="block">The quick brown fox jumps over the lazy dog.</div>
```

```css
.block {
  border-width: 1px 1px 0 1px;
  /*border-width: 1px 1px 0;*/
  border-style: solid;
  border-color: tomato;
}
```

個人的にはこれ。

---

### 方法1と3はあんまり好きじゃない

#### [方法1. 全指定後、一部消す(1)](#/4)

```css
.block {
  border: 1px solid tomato;
  border-bottom: 0;
}
```

#### [方法3. 必要な部分しか指定しない(1)](#/6)

```css
.block {
  border-top: 1px solid tomato;
  border-right: 1px solid tomato;
  border-left: 1px solid tomato;
}
```

なぜ好きじゃないのか

------

### 好きじゃない理由

ショートハンドで指定して、消しているお陰で、省略されたプロパティも初期化されてるから。

例えば、Modifierなどでborder-bottomを再度与える場合とかにめんどい。

------

#### 方法1の場合

```html
<div class="block">The quick brown fox jumps over the lazy dog.</div>
<div class="block  block--type-1">The quick brown fox jumps over the lazy dog.</div>
```

```css
.block {
  border: 1px solid tomato;
  border-bottom: 0;
}
.block--type-1 {
  border-bottom-width: 1px;
}
```

では復帰できないのです。[fiddle](http://fiddle.jshell.net/geckotang/gd7qnswt/7/show/light/)

border-bottom: none; だったとしても復帰できません。

------

##### 方法1で復帰させるには

```css
.block {
  border: 1px solid tomato;
  border-bottom: 0;
}
.block--type-1 {
  border-bottom: 1px solid tomato;
}
```

全部指定する必要がある。[fiddle](http://fiddle.jshell.net/geckotang/gd7qnswt/6/show/light/)

------

#### 方法3の場合

```html
<div class="block">The quick brown fox jumps over the lazy dog.</div>
<div class="block  block--type-1">The quick brown fox jumps over the lazy dog.</div>
```

```css
.block {
  border-top: 1px solid tomato;
  border-right: 1px solid tomato;
  border-left: 1px solid tomato;
}
.block--type-1 {
  border-bottom: 1px solid tomato;
}
```

全部指定する必要がある。[fiddle](http://fiddle.jshell.net/geckotang/gd7qnswt/8/show/light/)

------

#### 方法2や4なら？

```html
<div class="block">The quick brown fox jumps over the lazy dog.</div>
<div class="block  block--type-1">The quick brown fox jumps over the lazy dog.</div>
```

```css
.block {
  /*方法2
  border: 1px solid tomato;
  border-bottom-width: 0;
  */
  /*方法4*/
  border-width: 1px 1px 0 1px;
  border-style: solid;
  border-color: tomato;
}
.block--type-1 {
  border-bottom-width: 1px;
}
```

方法2も4も、線の幅を0にしていただけなので、簡単。[fiddle](http://fiddle.jshell.net/geckotang/gd7qnswt/10/show/light/)

---

## まとめ

- ショートハンドを書くときは「指定していない部分が初期化している」ということを理解して書きましょう。
- 本当は、必要なプロパティにだけ指定するのが綺麗ですね。
- ショートハンドで指定して、個別に指定することもありでしょう。

```css
.block {
  background: url('path/to/file');
  background-size: 10px 10px;
  transition: 1s all linear;
  transition-delay: 0.5s;
}
```

とかとか。
---

## ご清聴ありがとうございました。
