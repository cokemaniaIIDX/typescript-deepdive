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