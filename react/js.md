# JavaSctipt

## ラッパーオブジェクト

- string 型 には`String`
- number 型 には`Number`

```bash
$ node
> const str1 = 'Serval';
> const str2 = new String('Serval');
> str1 === str2
false
> str1 === str2.valueOf()
true”
```

## デフォルト引数

引数 m に値が入っていない場合は、デフォルトの 2 が代入される

```js
const raise = (n, m = 2) => n ** m;

console.log(raise(2, 3)); // 8
console.log(raise(3)); // 9
```

## プロパティ名のショートハンド

- オブジェクトの中身を動的に設定

```js
const key = "bar";
const baz = 65536;
const obj1 = { foo: 256, [key]: 4096, baz: baz };
console.log(obj1); // { foo: 256, bar: 4096, baz: 65536 }

// プロパティ名のショートハンド
const obj2 = { baz };
console.log(obj2); // { baz: 65536 }
```

## 分割代入(Destructuring Assignment)

```js
const [n, m] = [1, 4];
console.log(n, m); // 1 4

const obj = { name: "Kanae", age: 24 };
const { name, age } = obj;
console.log(name, age); // Kanae 24”
```

## シャローコピー

スプレッド構文を使用してコピーすると、オリジナルも更新される  
しかし、これらはネストが 1 段階までしかコピーされない  
※ 以下の例では、`address.town`の値は hoge のまま

```js
const hoge = {
  name: "hoge",
  email: "hoge@com",
  address: { town: "hoge town" },
};

const fuga = { ...hoge, name: "fuga" };
fuga.email = "fuga@com";
fuga.address.town = "fuga town";

console.log(hoge);

// 出力結果
// {
//   name: 'hoge',
//   email: 'hoge@com',
//   address: {
//     town: 'fuga town' // ここはfugaのまま
//   }
// }
```

この問題を解決する手段
- 文字列に展開してからJSONにパース
- LodashライブラリのcloneDeep()を使う
- rfdcライブラリを使う

## ショートサーキット評価(短絡評価)

```js
// || の左辺がfalsyな値の場合は右辺に渡されるため、Hello!が入る
const hello = undefined || null || 0 || NaN || '' || 'Hello!';
// && の左辺がtruthyな値の場合は右辺に渡されるため、Chao!が入る
const chao = ' ' && 100 && [] && {} && 'Chao!';

console.log("normalhello", hello); // Hello!
console.log("normalchao", chao); // Chao!

true && console.log('1.', hello);   // 1. Hello!
false && console.log('2.', hello);  // (no output) &&で左辺がfalseのため
true || console.log('3.', chao);    // (no output) ||で左辺がtrueのため
false || console.log('4.', chao);   // 4. Chao!”
```

## Optional Chaining

1階層目を確認するときは`undefined`が返されるが、2階層目以降はErrorとなってしまう。  
そんな時に`.?`を使用すると、1階層目と同じように`undefined`を返す。  
if文を書かなくても済む。
```js
const obj = {};
obj.foo
// undefined

obj.foo.bar
// Uncaught TypeError: Cannot read property 'bar' of undefined

obj?.foo?.bar
// undefined
```

## Nullish Coalescing(Null 合体演算子)

- 左辺が`null`または`undefined`の時だけ右辺が評価される
- 0や空文字はそのまま評価されてしまうことに注意

```js
const users = [
  {
    name: "hoge",
    address: {
      town: "hoge Town",
    },
  },
  {
    name: "fuga",
    address: {},
  },
  null,
];

for (u of users) {
  // Nullish Coalescing
  const user = u ?? { name: "(Somebody)" };
  // Optional Chaining
  const town = user?.address?.town ?? "(Somewhere)";
  console.log(`${user.name} は ${town} に住んでいる`);
}

// 出力結果
// hoge は hoge Town に住んでいる
// fuga は (Somewhere) に住んでいる
// (Somebody) は (Somewhere) に住んでいる
```

## this

>this とは、その関数が実行されるコンテキストであるオブジェクトへの参照が格納されている「暗黙の引数」

- アロー関数を使用する
  - アロー関数は暗黙の引数としての this を持たず、this を参照すると関数の外のスコープの this の値がそのまま使われるため

- thisはクラス構文内でしか使わない

# webpack

webpackとは、モジュールバンドラ。
モジュールバンドラとは、
複数のファイルを１つにまとめて出力してくれるツールのこと。
ES Modules、CommonJS、AMD を含めたさまざまなモジュール構文をサポートしていて、使われている構文を自動で検出し適切に解釈してくれるので、
異なるモジュール構文が混在していてもおかまいなしに依存関係を解決してバンドルしてくれる。
ES Module形式の`import`を使用できるのも、webpackのおかげ。

```sample1.js
module.exports.sample1 = function() {
        alert("sample1.jsです。");
}
```

```sample2.js
module.exports.sample2 = function() {
        alert("sample2.jsです。");
}
```

```app.js
//まとめたいファイルをインポート
var sample1 = require(./sample1.js);
var sample2 = require(./sample2.js);

//インポートしたファイルの関数を実行できる
sample1.sample1();
sample2.sample2();
```

```webpack.config.js
module.exports = {
        entry: "./app.js",
        output: {
                path: "./",
                filename: "bundle.js"
        }
}

```bash
$ webpack
```

```
// bandle.jsが出力される
```

## ES Modules

ES modulesは、ES2015仕様において策定された、JavaScriptファイルから別のJavaScriptファイルを読み込む仕組みです。
ブラウザの世界では、JavsScriptファイルを読み込むためにはHTMLファイルの中に<script>タグを記述する必要がありました。ES modules仕様が実装された環境では、JavaScriptファイルの中にimport文という新しい構文を記述して、ほかのJavaScriptファイルの内容を読み込むことができます。読み込まれる側では、これも新しい構文であるexport文を使ってどの値を公開するのかを指定します。

## 関数型プログラミング

参照透過的な関数を組み合わせることで解決すべき問題に対処していく宣言型のプログラミング

プログラミング・パラダイムは2つに大別される
- 命令型プログラミング
  - 最終的な出力を得るために、状態を変化させる連続した文によって記述されるプログラミングスタイル
- 宣言型プログラミング
  - 手続き型プログラミング
  - オブジェクト指向プログラミング
  - 関数型プログラミング

```js
// 手続き型
{
  const octuples = [];

  for (let n = 1; n <= 100; n += 1) {
    if (n % 8 === 0) {
      octuples.push(n);
    }
  }

  console.log(octuples);
}

// 関数型
{
  const range = (start, end) => [...new Array(end - start).keys()].map((n) => n + start);
  console.log(range(1, 101).filter((n) => n % 8 === 0));
}
```

## 配列の反復処理

```js
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];

console.log(arr.map((n) => n * 2));           // [ 2, 4, 6, 8, 10, 12, 14, 16, 18 ]
console.log(arr.filter((n) => n % 3 === 0));  // [ 3, 6, 9 ]
console.log(arr.find((n) => n > 4));          // 5
console.log(arr.findIndex((n) => n > 4));     // 4
console.log(arr.every((n) => n !== 0));       // true
console.log(arr.some((n) => n >= 10));        // false”
```

- map() …… 対象の配列の要素ひとつひとつを任意に加工した新しい配列を返す
- filter() …… 与えた条件に適合する要素だけを抽出した新しい配列を返す
- find() …… 与えた条件に適合した最初の要素を返す。見つからなかった場合は undefind を返す
- findIndex() …… 与えた条件に適合した最初の要素のインデックスを返す。見つからなかった場合は -1 を返す
- every() ……「与えた条件をすべての要素が満たすか」を真偽値で返す
- some() ……「与えた条件を満たす要素がひとつでもあるか」を真偽値で返す

### reduce()とsort()

- reduce()
  - 第1引数nには、前回の関数の実行結果が入り、第2引数にはarrが順番に入る
  - nは関数の実行ごとに上書きされていく

```js
const arr = [1, 2, 3, 4, 5];

console.log(arr.reduce((n, m) => n + m));　       // 15
```

- sort()
  - 引数として既定の比較関数を渡す(以下3つのルールを守る)
    - 1. 第1引数が第2引数より優先度が高い（前に来る）場合、-1を返す
    - 2. 第1引数が第2引数より優先度が低い（後に来る）場合、1を返す
    - 3. 第数と第2引数の優先度が同じ（ソートの必要がない）場合、0 を返す（※省略可）
- sort()は破壊的メソッドなので注意すること！(sortを実行すると、もとの配列の順番がかわってしまう)
- しかしその場合は、slice()を間に挟むとシャーロコピーを作成してくれる(以下2つ目の例) 

```js
// もとのarrも変更されてしまっている
const arr = [1, 2, 3, 4, 5];

console.log(arr.sort((n, m) => n > m ? -1 : 1));  // [ 5, 4, 3, 2, 1 ]
console.log(arr) // [ 5, 4, 3, 2, 1 ]
```

```js
// slice()を挟んだ例
// もとのarrの順番は変更されていない
const lst = [5, 7, 1, 3];

console.log(lst.slice().sort((n, m) => n < m ? -1 : 1));  // [1, 3, 5, 7]
console.log(lst);   // [5, 7, 1, 3]
```

### Array.prototype.forEach() メソッドと for...of 文

基本的には`map()`や`find()`で足りるはずなので使用しない方がいいが、どちらかといえば`forEach`を使用する。

```js
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];

arr.forEach((n) => {
  if (n % 2 === 0) {
    console.log(`${n} is even`);
  }
});
```

```js
const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];

for (let n of arr) {
  if (n % 2 !== 0) {
    console.log(`${n} is odd`);
  }
}
```

### includes()

- 配列操作のメソッドではないが、使用頻度も高いメソッド
- 配列の中に指定した値があればtrueなければfalseを返す

```js
const arr = [1, 2, 3, 4, 5];

console.log(arr.includes(5));   // true
console.log(arr.includes(8));   // false
```

## オブジェクトの反復処理

オブジェクトが持つメソッドを使って、いったん配列を形成する方法がAirbnbスタイルガイドでは推奨されている

```js
const user = {
  id: 3,
  name: 'Bobby Kumanov',
  username: 'bobby',
  email: 'bobby@maple.town',
};

console.log(Object.keys(user));
// [ 'id', 'name', 'username', 'email' ]

console.log(Object.values(user));
// [ 3, 'Bobby Bear', 'bobby', 'bobby@maple.town' ]

console.log(Object.entries(user));
// [
//   [ 'id', 3 ],
//   [ 'name', 'Bobby Kumanov' ],
//   [ 'username', 'bobby' ],
//   [ 'email', 'bobby@maple.town' ]
// ]
```

# クロージャ

- 以下の例はグローバル変数`count`に依存している
```js
let count = 0;

const increment = () => {
  return count += 1;
};
```

- クロージャに書き換える
- なぜ変数`count`は関数の外でも値を保持していられる？
  - 内側の関数 increment() の引数でもなく increment() 自身のローカル変数でもない変数のことを"自由変数（Free Variable）"という
  - JavaScript では関数のスコープは『レキシカルスコープ（Lexical Scope）29』といって、その定義時に決定され固定される。だから自由変数を参照する内側の関数がエンクロージャによって返され外のスコープで生きている限り、その自由変数も GC に回収されず状態を保っていられる
  - 

```js
const counter = () => {
  let count = 0;

  const increment = () => {
    return count += 1;
  };

  return increment;
};

const hoge = counter();

console.log(hoge());
console.log(hoge());
console.log(hoge());

// 出力結果
// 1
// 2
// 3
```