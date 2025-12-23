
## ブロック式

- ブロックの最終行にセミコロンがないと、そのブロックが値を生む
- let 宣言はセミコロンが常に必要
- セミコロンの有る式の返り値は切り捨てられる

```rust
let msg = {
    let dandelion_control = puffball.open();
    dandelion_control.release_all_seeds(launch_codes);
    dandelion_control.get_status()
};
```

### let 宣言

- ローカル変数を宣言する
- 型と初期化式は省略できる
- セミコロンは必須
- 変数名は文字もしくはアンダースコアで始まらなければならない(数字は2文字目以降でしか使えない)

```rust
let name: type = expr;
```

let 宣言で、変数を初期化せずに宣言することもできる。

```rust
let name;
if user.has_nickname() {
    name = user.nickname();
} else {
    name = generate_unique_name();
    user.register(&name);
}
```

## if 式

- condition(条件)は、bool 型の式でなければならない
- 条件式の周りの括弧は必須ではなく、波括弧は必須
- if 式のすべてのブロックは同じ型の値を生成しなければならない

```rust
if condition1 {
    block1
} else if condition2 {
    block2
} else {
    block_n
}
```

```rust
let condition = true;
let number = if condition { 5 } else { 6 };
```



## match 式

- `_` はワイルドカードパターンで全てのものにマッチする(最後に書く必要がある)
- expr がブロックの場合には、分岐の最後のカンマは省略できる
- すべてのケースをカバーしていない match 式は許されない

```rust
match value {
    pattern = > expr,
    ...
}
```

```rust
let score = match card.rank {
    Jack => 10,
    Queen => 10,
    Ace => 11,
    _ => 0
};
```

## if let 式

- expr が pattern にマッチするなら block1 が実行され、マッチしなければ block2 が実行される

```rust
if let pattern = expr {
    block1
} else {
    block2
}
```

```rust
if let Some(cookie) = request.session_cookie {
    return restore_session(cookie);
}
```

if let 式は、以下の match 式と同じ

```rust
match expr {
    pattern = > { block1 }
    _ = > { block2 }
}
```


## while 式

- 戻り値は常に `()`

```rust
while condition {
    block
}
```

```rust
let mut number = 3;

while number != 0 {
    println!("{}!", number);
    number -= 1;
}
```



## while let 式

```rust
while let pattern = expr {
    block
}
```

## loop 式

```rust
loop {
    block
}
```

loop のボディ部では、`break` に値を与えると、その値が loop 式の値となる

```rust
let answer = loop {
    if let Some(line) = next_line() {
        if line.starts_with("answer: ") {
            break line;
        }
    } else {
        break "answer: nothing";
    }
};
```


## for 式

- 戻り値は常に()
- for ループで処理すると、move セマンティクスに従って、値が消費される

```rust
for pattern in iterable {
    block
}
```

```rust
let a = [10, 20, 30];
for element in a {
    println!("the value is: {}", element);
}
```



```rust
let strings: Vec<String> = ...
for s in strings {
    println!("{}", s);
}
println!("{} error(s)", strings.len()); // コンパイルエラー
```

```rust
for rs in &strings {
    println!("String {:?}", *rs);
}
```

## 参照

- `&T` : 変更不能な共有参照
ある値に対して複数の共有参照を持つことができるが、値を読み出すことしかできない

- `&mut T` : 排他的な可変参照
参照先の値を読み出し、変更することができる。
ある値に対してこの種の参照が存在する間は、その値に対する他の参照は共有参照であれ可変参照であれ作ることはできない。


## 配列

- 型 `[T; N]` は、型 T の N 個の値の配列を表す
- 配列のサイズは、コンパイル時に定まり、型の一部となる
- 新しい要素を追加したり、縮小したりすることはできない

```rust
let lazy_caterer: [u32; 6] = [1, 2, 4, 7, 11, 16];
let taxonomy = ["Animalia", "Arthropoda", "Insecta"];
```

`[V; N]` とすると、長さNの配列を V の値で初期化した配列が入手できる(Rustには、初期化されていない配列を作る記法はない)。
以下はすべての要素が `true` の 10 の bool配列。

```rust
let mut sieve = [true; 10];
```

## ベクタVec<T>

- ヒープ上に確保されるサイズを変更することができる型Tの配列

```rust
let mut primes = Vec::new();
pal.push(2);
pal.push(3);
pal.push(5);
```

`vec!` マクロは、新しい空のベクタを作成して要素を追加するのと等価

```rust
let mut primes = vec![2, 3, 5];
primes.push(7);
```

## スライス [T]

- 配列やベクタのある領域を指す

TODO


## シャドーイング

- 定義した変数と同じ名前の変数を新しく宣言できる
- 新しい変数は、前の変数を覆い隠す

```rust
let x = 5;
let x = x + 1;
```

## 範囲(range)

- `..` 演算子は範囲(range)を生成する
- `std::iter::IntoIterator` を実装している

```rust
for i in 0..20 {
    println!("{}", i);
}
```


## 型エイリアス

`type` キーワードを使って既存の型に別の名前を付けることができる。

```rust
type Bytes = Vec<u8>;

fn decode(data: &Bytes) {
}
```


## 発散する関数(divergent function)

- 通常の意味で終了しない式には特別な型 `!` が割り当てられる

```rust
fn exit(code: i32) -> ! {
}
```

## タプル (tuple)

- 個々の要素が異なる型を持つことができる

```rust
let pair = (1, true);
println!("Pair is {:?}", pair);
println!("{}", pair.0);
println!("{}", pair.1);
```

## ユニット型 （unit type）

- 0要素のタプル `()`
- 意味のある値を渡す必要がないにもかかわらず、コンテクストが何らかの型を要求する場合にユニット型を用いる。



## Box

- ヒープ上に値を確保する
- `Box<T>` はヒープ上の 型T の値へのポインタ
- `Box::new(v)` を呼ぶと、ヒープ上にメモリを確保し、値vをそこに移動し、そのヒープ空間を指すBoxを返す
-  移動(move)されていない限り、スコープから外れるとメモリは即座に解放される

```rust
let t = (12, "eggs");
let b = Box::new(t);
```

## static定数

- 定数を導入するにはconstキーワードを用いる
- 型を必ず指定する必要がある
- 定数は慣例として、すべて大文字で単語間はアンダースコアで区切る
- 必要なら `pub` がつく

```rust
const ROOM_TEMPERATURE: f64 = 20.0;
```

## Result

- Rustには例外がない
- 失敗する可能性のある関数は返り値の型で扱う

```rust
fn get_weather(location: LatLng) -> Result<WeatherReport, io::Error>
```

```rust
match get_weather(hometown) {
    Ok(report) => {
        display_weather(hometown, &report);
    }
    Err(err) => {
        println!("error querying the weather: {}", err);
    }
}
```

## ? 演算子

- 成功した場合には、`Result` を解いて中の成功値を取り出す
- 失敗した場合には、呼び出し元の関数から即時にリターンし、呼び出し連鎖の上流の関数にエラーを渡す
  -`?` は返り値が `Result` 型の関数内部でしか利用できない。

```rust
let weather = get_weather(hometown)?;
```

`?`は、`Option` 型に対しても同様に使うことができる。値がNone だった場合には、その時点でリターンする。


## main()でのエラー処理

```rust
fn main() {
    if let Err(err) = calculate_tides() {
        print_error(&err);
        std::process::exit(1);
    }
}
```

TODO


## etc


変数はその値を所有する。
その変数が宣言されたブロックから、プログラム実行の制御が離れたときに、変数はドロップされる。
それに伴って値もドロップされる。




- 値を 1 つの所有者から別の所有者に移動することができる
  - これによって、ツリーを構築し、変更し、分割することができるようになる
- 整数、浮動小数点数、文字などの非常に単純な型には、所有権のルールが適用されない
  - これらの型は Copy 型と呼ばれる
- 標準ライブラリに、参照カウントポインタ型の Rc と Arc が用意されている
  -これらを用いると、一定の制限下で 1 つの値に対して複数の所有者を設けることができる
- 値への「参照の借用」ができる
  - 参照は所有権のないポインタで、生存期間が限定されている


Rustでは、ほとんどすべての型が、変数への値の代入、関数への引数の受け渡し、関数からの返り値の返却の際にコピーされず、移動 （move）される。

Copy 型の値を代入すると、値は移動されず、コピーされる
単純なビット単位のコピーだけで事足りる型だけがCopyとなり得る
標準的なCopy 型としては、すべての機械語レベルの整数や浮動小数点数などの数値型、charとbool 型などがある。
Copy 型のタプルや固定長の配列もCopy 型になる。


デフォルトではstructとenum 型はCopy ではない。

構造体のすべてのフィールドがCopy 型なのであれば、構造型もCopy 型にすることができる。
これには、構造体の定義の上に属性#[derive(Copy, Clone)]を付けるだけでよい。

```rust
#[derive(Copy, Clone)]
struct Label { number: u32 }
```

## RcとArc

その値を使っているすべてのものが使い終わるまで生きていてほしいような値のために、参照カウントのポインタ型`Rc`と`Arc`がある。

Arcという名前はアトミックな参照カウント (atomic reference count)で、複数のスレッド間で直接共有しても安全なようにできている

Rc 型は、高速だがスレッド安全でないコードで参照カウントを管理する

スレッド間でのポインタの共有を行わないのであれば、Rc 型を使えばよい。

Rc<T>をクローンすると、T の値はコピーされず、同じものを指すポインタが作られ、参照カウントがインクリメントされる
Rcポインタに所有される値は不変となる

```rust
use std::rc::Rc;

let s: Rc<String> = Rc::new("abc".to_string());
let t: Rc<String> = s.clone();
```

最後に残った Rc がドロップされるとヒープ上のメモリもドロップされる


## 参照 (reference)

Rustには、所有権を持たないポインタ型、参照 （reference）がある


参照は参照先よりも長生きしてはいけない。プログラマは、参照が参照先よりも長生きすることがないことを、プログラム中で明確にしなければならない。これを強調するために、Rustはある値に対する参照を作ることを借用 （borrowing）と呼ぶ。
借りたものは、いつかは所有者に返さなければならない。

参照には2種類ある

- 共有参照（shared reference）
  - 参照先を読むことはできるが変更することはできない
  - ある値に対する共有参照は、同時にいくらでも持つことができる
  - &e は、値 e に対する共有参照を返す。e の型が T であれば、&e の型は &T（「ref T」と発音する）となる
  - 共有参照は Copy 型である。

- 値への可変参照（mutable reference）
  - 値を読み出すことも変更することもできる
  - 同じ値に対する可変参照と、他の参照（共有参照でも可変参照でも）とは同時に使用することはできない
  - &mut e は、値 e に対する可変参照を返す。この参照の型は &mut T（「refmute T」と発音する）となる
  - 可変参照は Copy 型ではない

値の所有権を移動するような方法で関数へ値を渡すことを、値渡し （pass by value）と呼び、関数に値の参照を渡すことを、参照渡し （pass by reference）と呼ぶ

参照は決してnullになることはない

```rust
let mut y = 32;
let m = &mut y; // &mut y はy への可変参照
*m += 32;       // 明示的にm を参照解決してy の値を変更。y の新しい値を見る
assert!(*m == 64);
```
Rustでは参照は明示的に& 演算子で作られ、参照解決も* 演算子で明示的に行われる


Rustでは.演算子が、必要に応じて暗黙に左のオペランドを参照解決するようになっている
Rustでは参照を作る際、参照解決する際には& 演算子と* 演算子を明示的に用いるが、例外として.演算子は借用と参照解決を暗黙に行う
`.` 演算子は何段でも参照解決を行って目的の値を取り出す
`.` 演算子と同様に、比較演算子も、参照の連鎖を「見通す」ことができる。



132

