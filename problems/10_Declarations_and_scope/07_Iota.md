# Iota

## Problem 1 

- https://play.golang.org/p/scG7p8Izdnn

```go
package main

import (
	"fmt"
)

const (
	a = 1
	b = 2
	c = iota
)

const (
	d = 2
	e = iota
)

func main() {
	fmt.Println(a*b*c*d*e)
}
```

<details>
<summary>
ANSWER
</summary>

- 8と表示される。

> Within a constant declaration, the predeclared identifier iota represents successive untyped integer constants. Its value is the index of the respective ConstSpec in that constant declaration, starting at zero. 

const宣言の中では、宣言済みの名前である`iota`は連続する型なしの整数のconstantを表します。その値は、対応する[ConstSpec](https://golang.org/ref/spec#ConstSpec)の番号です。番号は0から始まります。

ConstSpecがわかりにくいですが、

```EBNF
ConstDecl      = "const" ( ConstSpec | "(" { ConstSpec ";" } ")" ) .
ConstSpec      = IdentifierList [ [ Type ] "=" ExpressionList ] .
```

を見ると、

```go
const a = 1
const b, c = 2, 3
```

のようなコードにおいて、`a = 1` や `b, c = 2, 3`の部分のことを`ConstSpec`と呼ぶようです。

`const a = 1`のように、`ConstSpec`を含む定数宣言全体は`ConstDecl`(これがconstant declarationに相当)と定義されています。

次のように複数の`ConstSpec`を並べたものも一つの`ConstDecl`になります。

```go
const (
    c = 3
    d = 4
)
```

</details>


## Problem 2

- https://play.golang.org/p/nSa9ko6Il00

```go
package main

import (
	"fmt"
)

const (
	a    = 1
	b, c = 1 << iota, 1 << iota
	d    = 1 << iota
	e    = 1 << iota
)

func main() {
	fmt.Println(a * b * c * d * e)
}
```

<details>
<summary>
ANSWER
</summary>

- 128と表示される。

> Within a constant declaration, the predeclared identifier iota represents successive untyped integer constants. Its value is the index of the respective ConstSpec in that constant declaration, starting at zero. 

const宣言の中では、宣言済みの名前である`iota`は連続する型なしの整数のconstantを表します。その値は、対応する[ConstSpec](https://golang.org/ref/spec#ConstSpec)の番号です。番号は0から始まります。

```EBNF
ConstDecl      = "const" ( ConstSpec | "(" { ConstSpec ";" } ")" ) .
ConstSpec      = IdentifierList [ [ Type ] "=" ExpressionList ] .
```

問題の`ConstDecl`を見直してみると、次のように**4つ**の`ConstSpec`からなっていることがわかります。

```go
const (
	a    = 1 // iota = 0, a = 1 
	b, c = 1 << iota, 1 << iota // この行は1つのConstSpecであり、どちらのiotaも1となる。b == c == 2
	d    = 1 << iota // iota = 2, d = 4
	e    = 1 << iota // iota = 3, e = 8
)
```

よって`a~e`の積は、`1*2*2*4*8 = 128`となります。

</details>

