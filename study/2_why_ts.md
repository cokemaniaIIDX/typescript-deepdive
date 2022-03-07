# なぜTypeScriptを使うのか

## 1. 開発スピードの上昇

型を導入することでコンパイルという早めの段階でエラーを見つけることができる
コードを作って実際に動作させるよりも、コードを書いてる時点でエラーに気づければ、
修正もすぐにできるので、開発スピードが上がる

## 2. 型導入の障壁が少ない

- jsファイルはtsファイル

.jsのファイルを.tsに変えてコンパイルしても問題なく動く
そもそも型を導入するかどうかは開発者の意図にゆだねられている

- 暗黙的な方推論機能がある

![型推論](../images/type_inference.png)

```ts
var foo = 123;
foo = '456';

// 型 'string' を型 'number' に割り当てることはできません
```

- 明示的な型指定

```ts
var foo: number = 123;

var foo: number = '123';
// 型 'string' を型 'number' に割り当てることはできません
```

明示的な型指定を行うことで、
将来の開発者(もしくは未来の自分)にとってわかりやすいコードになる

また、開発者の理解とコンパイラの型チェックの理解を一致させることで
開発者の思う正しいアルゴリズムにコンパイラを強制できる

- 構造的な型

TypeScriptはJavaScript開発者の認知的負荷を小さくするために
ダックタイピングを第一級となっている←？？？ｗｗｗ

とにかく、↓の例みたいな感じで、 x と y を含めば なんでも引数として取れる関数を定義できるみたい

```ts
interface Point2D {
  x: number;
  y: number;
}

interface Point3D {
  x: number;
  y: number;
  z: number;
}

var point2D: Point2D = { x:0, y:10 }
var point3D: Point3D = { x:0, y:10, z:20 }
function iTakePoint2D(point: Point2D) { /* 何らかの処理 */}

iTakePoint2D(point2D); // 全く同じなので問題なし
iTakePoint2D(point3D); // zが追加されてるけどこれも問題なし
iTakePoint2D({ x:0 }); // yが存在しないのはエラー
```

- 型チェックでエラーでもJavaScriptは出力される

そもそもJSでは型チェックしないのでコンパイルエラーでもJSは出力される
この機能のおかげで既存のJSをTypeScriptに移しやすい

- declareによって既存JSライブラリでも型を利用できる

TypeScriptの目標は既存のJSライブラリでも安全に簡単に移行できること
TypeScriptではこれをdeclareでやる

```ts
$('.awsome').show(); // $(jquery)なんてものはないので エラー

↓

declare var $: any; // $を宣言する
$('.awesome').show(); // 問題なし

or

declare var $: {
  (selector: string): any; // さらに型も指定する
};
$(.'awesome'.show(); // 問題なし
$(123).show(); // エラー stringじゃないとダメ
```

`interface`とか`any`とかは後で説明してくれるらしい

- モダンなJSの機能をすぐに使える

例えばクラスは最近の機能らしい

```ts
class Point {
  constructor(public x: number, public y: number) {
  }
  add(point: Point) {
    return new Point(this.x + Point.x, this.y + point.y);
  }
}

var p1 = new Point(0, 10);
var p2 = new Point(10, 20);
var p3 = p1.add(p2); // { x:10, y:30 }
```

アロー関数も最近らしい

```ts
var inc = x => x+1;
```

# まとめ

型を導入することで様々なメリットを享受できるし、
最新のJSの機能も積極的に取り入れられているので、
TypeScriptを使わない手はない