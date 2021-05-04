# TypeScript

- ※型推論 ...ローカル変数を初期値付きで宣言する場合、明示的に型をしていしなくても`var`などで代用できる文法

## setting

- ローカル環境は`ts-node`が一般的

```bash
ts-node -v
```

表示されない場合は、以下のコマンドでインストール

```bash
npm install -g typescript ts-node
```

TypeScriptではコンパイラオプションに`noImplicitAny`が指定されていないと、
引数の型定義がなくても暗黙のうちにany型があてがわれてコンパイルが通ってしまうので、
設定ファイルの`tsconfig.json`でそのオプションを有効にすること

```tsconfig.json
{
  ...
  "noInplicitAny": true,
}
```

## 型アノテーション

変数名: 型 で定義
```ts
let n: number = 3;
// 配列(以下2つどちらの書き方もできるが、前者が推奨される)
const numArr: number[] = [1, 2, 3]
const numArr: Array<number> = [1, 2, 3]; // ジェネリクス
```

## interface

- Goの構造体のようなもの？
- オブジェクトの型に名前をつけることができる -> interface
- `readonly`は書き換え不可のプロパティになる
- 末尾に`?`をつけると、省略可となる

```ts
interface Color {
  readonly rgb: string;
  opacity: number;
  name?: string;
}

const turquoise: Color = { rgb: '00afcc', opacity: 1 };
turquoise.name = 'Turquoise Blue';
turquoise.rgb = '03c1ff';  // error TS2540: Cannot assign to 'rgb' because it is a read-only property.”
```

より柔軟に定義する

```ts
interface Status {
  level: number;
  maxHP: number;
  maxMP: number;
  [attr: string]: number; // インデックスシグネチャ(keyはstringとnumberのみ)
}

const myStatus: Status = {
  level: 99,
  maxHP: 999,
  maxMP: 999,
  attack: 999,
  defense: 999,
};

```

## Enum型


```ts
enum Pet {Cat, Dog, Rabbit}
console.log(Pet.Cat, Pet.Dog, Pet.Rabbit);
// 0 1 2
```

```ts
enum Pet {
  Cat = "Cat",
  Dog = "Dog",
  Rabbit = "Rabbit",
}

let Tom: Pet = Pet.Cat;
Tom = "Dog"; // Error(同じ型でないため)
```

## リテラル型

- 配列リテラルとかオブジェクトリテラルのような"式としてのリテラル"とは関係ない
- (文字列)リテラルは任意の文字列以外許さない

```ts
let Tom: "Cat" | "Dog" | "Rabbit"; 

Tom = "Cat";
console.log(Tom); // Cat
Tom = "Hamster";
console.log(Tom); // Error
```

## タプル型

- 順番や要素数に制約を設けられる型

```ts
const charAttrs: [number, string, boolean] = [1, 'patty', true];
// レストパラメーターも使用できる
const spells: [number, ...string[]] = [7, 'heal', 'sizz', 'snooz'];
```

## any型

- いかなる型の値でも受け付ける
- Goの空interfaceのようなもの

```ts
let val: any = 100;
console.log(val);

val = "vax";
console.log(val);

val = null;
console.log(val);
```

## unknown型

- ver3.0から追加された
- anyの型安全板
- 任意の型の値を代入できる点はanyと同じだが、それ自体は何のプロパティ等も持たない

## never型

- 何も代入できない型

```ts
const greet = (friend: 'Serval' | 'Caracal' | 'Cheetah') => {
  switch (friend) {
    case 'Serval':
      return `Hello, ${friend}!`;
    case 'Caracal':
      return `Hi, ${friend}!`;
    case 'Cheetah':
      return `Hiya, ${friend}!`;
    default: {
      const check: never = friend;
    }
  }
};

console.log(greet('Serval'));    // Hello, Serval!
```

## 関数の型定義

- 引数の型は必ず指定する必要がある
- 何も返さない関数の戻り値は`void`となる

### 引数と戻り値を別々に定義する方法
```ts
// 宣言文で定義
{
  function add(n: number, m: number): number {
    return n + m;
  }
  console.log(add(2, 4));   // 6
}

// function キーワードによる関数式で定義
{
  const add = function(n: number, m: number): number {
    return n + m;
  };
  console.log(add(5, 7));   // 12
}

// アロー関数式で定義
{
  const add = (n: number, m: number): number => n + m;
  console.log(add(8, 1));   // 9

  const hello = (): void => {
    console.log('Hello!');
  };
  hello();                  // Hello!
}
```

引数と戻り値を一緒に定義する方法

```ts
// interfaceとして呼び出し可能オブジェクトを定義し、それを関数式に適用したもの
{
  interface NumOp {
    (n: number, m: number): number;
  }

  const add: NumOp = function (n, m) {
    return n + m;
  };
  const subtract: NumOp = (n, m) => n - m;

  console.log(add(1, 2));       // 3
  console.log(subtract(7, 2));  // 5
}

// アロー型アノテーションでインラインで行う
{
  const add: (n: number, m: number) => number = function (n, m) {
    return n + m;
  };
  const subtract: (n: number, m: number) => number = (n, m) => n - m;

  console.log(add(3, 7));       // 10
  console.log(subtract(10, 8)); // 2
}
```

### ジェネリクス

- ジェネリックプログラミング ... データの型に束縛されないよう**型を抽象化**してコードの再利用性を向上させつつ、静的型付け言語の持つ型安全性を維持するプログラミング手法

- `T`は「型引数」... 関数に渡す引数と同じで、任意の型を <> によって引数として渡すことで、その関数の引数や戻り値の型に適用できるようになる。
- 最初の例では T は型推論によって number、次の例では string になってる


```ts
const toArray = <T>(arg1: T, arg2: T): T[] => [arg1, arg2];

console.log(toArray(8, 3)); // [ 8, 3 ]
console.log(toArray('foo', 'bar')); // [ 'foo', 'bar' ]
```

## class

- 継承は子クラスが親クラスに強く依存する

- アクセス修飾子
  - public ... 自クラス、子クラス、インスタンスすべてからアクセス可能。デフォルトではすべてのメンバーがこの public になる
  - protected ... 自クラスおよび子クラスからアクセス可能。インスタンスからはアクセス不可
  - private ... 自クラスからのみアクセス可能。子クラスおよびインスタンスからはアクセス不可

```ts
class Rectangle {
  // プロパティ初期化
  // コンストラクタに引数のないクラスでは、コンストラクタを省略できる
  // readonly修飾子をつけることで。メンバー変数を変更不可にする
  readonly name = 'rectangle';
  sideA: number;
  sideB: number;

  constructor(sideA: number, sideB: number) {
    this.sideA = sideA;
    this.sideB = sideB;
  }

  getArea = (): number => this.sideA * this.sideB;
}
```

## 継承よりも合成

```ts
// 継承
class Square extends Rectangle {
  readonly name = 'square';
  side: number;

  constructor(side: number) {
    super(side, side);
  }
}

// 合成
class Square {
  readonly name = 'square';
  side: number;

  constructor(side: number) {
    this.side = side;
  }

  getArea = (): number => new Rectangle(this.side, this.side).getArea();
}
```

## Null の安全性

以下の例では、stringにnull, numberにundefinedが格納できてしまっている  

```ts
const foo: string = null;
const bar: number = undefined;
```

これらの問題を解決するには、`tsconfig.json`ファイルに追記する

```tsconfig.json
  ...
  strictNullChecks": true
```

## インポート / エクスポート

- TypeScriptでは拡張子を書くとエラーになる

```ts
import bar from './bar';
```
- 以上のimport文で読み込みをする場合、次の順にモジュールを探索していく
- 最初に見つかったものが読み込まれ、全部ヒットしなかった時点でエラーを返す

1. src/bar.ts
2. src/bar.tsx
3. src/bar.d.ts
4. src/bar/package.json の types または typings プロパティで設定されている型定義ファイル
5. src/bar/index.ts
6. src/bar/index.tsx
7. src/bar/index.d.ts


