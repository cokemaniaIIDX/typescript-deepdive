# TypeScriptはJavaScript

なので、むしろきれいで安全なJavaScriptが書ければそれはきれいで安全なTypeScriptだということ
TypeScriptはきれいなJavaScriptを書くための方法を標準化したもの

# TypeScriptはJavaScriptを改善する

TypeScriptはJavaScriptでは罷り通ってる意味不明な仕様をエラーとして表示してくれる

```ts
[] + []; // JSは "" そ返す(意味不明) tsはエラー

[] + {}; // 0
{} + []; // [object Object]
{} + {}; // NaN [object Object]
"こんにちは" - 1; // NaN

function add(a,b) {
  return
    a + b; // 到達不可能
}
```

TypeScriptはJavaScriptであるからこそ、
開発者はJavaScriptに精通している必要もある

## 等価演算子について

JavaScriptの仕様は以下の通り
`==`は型変換を行ってしまうので、等価演算する場合は常に`===`,`!==`を使うようにする

もちろん、Typescriptでは==でもエラー起こすので通常は心配する必要はない

```js
// == は型変換を行う
console.log(5 == "5"); // string→number に変換されて true になってしまう
console.log(5 === "5"); // さすがにfalse ===は型もちゃんと見る

console.log("" == "0"); // さすがにfalse
console.log(0 == ""); // 0も""も falsy なので true になってしまう

// === を使えば安心
console.log("" === "0"); // false
console.log(0 === ""); // false
```

## 構造の等価について

`==`,`===`では構造の比較はできない

```js
console.log({a:123} == {a:123}); // false
console.log({a:123} === {a:123}); // false
```

こういう場合は↓のようにする

```js
import * as deepEqual from "deep-equal";

console.log(deepEqual({a:123},{a:123})); // True

// もしくは

type IdDisplay = {
  id: string,
  display: string
}
const list: IdDisplay[] = [
  {
    id: 'foo',
    display: 'Foo Select'
  },
  {
    id: 'bar',
    display: 'Bar Select'
  },
]

const fooIndex = list.map(i => i.id).indexOf('foo');
console.log(fooIndex); // 0
```

## リファレンスについて

- 変更はすべての参照に影響する

```js
var foo = {};
var bar = foo;

foo.baz = 123;
cosole.log(bar.baz); // 123
```

- 比較は参照に対して行われる

```js
var foo = {};
var bar = foo;
var baz = {};

console.log(foo === bar); // true
console.log(foo === baz); // false
```

## null と undefined

- undefined : 初期化されていない
- null : 現在利用できない

### どちらであるかのチェック

null と undefined をあまり意識しなくてもよい
`==`, `!=` を使って両方まとめてチェックすることができる

```js
console.log(undefined == undefined); // true
console.log(null == undefined); // true

console.log(0 == undefined); // false
console.log('' == undefined); // false
console.log(false == undefined); // false

function foo(arg: string | null | undefined) {
  if (arg != null) {
    // 何らかの処理 引数argはnullとundef以外の文字列
  }
}
```

### root level の undefined チェック

`== null`を使うべきだけど、rootレベルのものでは利用してはいけない
ReferenceErrorが発生してよくないことが起こるらしい
そういう時はtypeofを使うらしい

```js
if (typeof someglobal !== 'undefined') {
  console.log(someglobal)
}
```

### undefined を明示的に利用しないようにする

```js
function foo() {
  // 何らかの処理
  return {a:1, b:2};
  // else
  return {a:1, b:undefined}
}
```

↑こういうの(b:undefinedみたいなの)はよくない
↓代わりにこういう感じにする

```js
funciton foo(): (a:number, b?:number) {
  // if
  return {a:1, b:2};
  // else
  return {a:1};
}
```

### ノードスタイルのコールバック

(err, somethingElse) => { something } みたいなのは、エラーがなければ`err`に`null`を設定して呼ばれる

```js
fs.readFile('someFile', 'utf8', (err,data) => {
  if (err) {
    // 何らかのエラー処理
  } else {
    // エラーなしの場合
  }
});
```

APIを作成するときは、できるだけ`null`を避ける
できるだけ`Promise`を返すようにする
そうするとerrがnullかどうかを気にする必要がなくなる

### 値の有効性を表す意味でundefinedを利用しない

```ts
function toInt(str:string){
  return str ? parseInt(str) : undefined;
}
```

↑これはよくない
↓こうする

```ts
function toInt(str: string): { valid: boolean, int?: number} {
  const int = parseInt(str);
  if (isNaN(int)) {
    return { valid: false };
  }
  else {
    return { valid: true, int };
  }
}
```

