# Map Types

https://golang.org/ref/spec#Map_types

## Q1

https://play.golang.org/p/8uyJ1cCU4l9

```go
package main

import (
	"fmt"
)

func main() {
	m1 := map[int]int{}
	fmt.Println(m1 == nil)
}
```

<details>
<summary>Answer</summary>

`false`

- https://golang.org/ref/spec#Comparison_operators

仕様書的にはこっちの内容でした。mapのkeyになれる型はどんな型か、という意味ではmapに関連する話だと思います。

> The equality operators == and != apply to operands that are comparable. 

`==`演算子は比較可能であるようなオペランドに適用できます。

> Slice, map, and function values are not comparable. However, as a special case, a slice, map, or function value may be compared to the predeclared identifier nil. 

Slice, map, functionの値は比較可能ではありません。ただし、slice, map, functionの値は、事前宣言された名前である`nil`と比較することができます。

</details>

## Q2 

https://play.golang.org/p/izCVg6bXZIG

```go
package main

import (
	"fmt"
)

func main() {
	m := make(map[[]int]int)
	nums := []int{1,2,3}
	m[nums] = 6
	fmt.Println(m[nums])
}
```

<details>
<summary>Answer</summary>

build error

```
./prog.go:8:12: invalid map key type []int

Go build failed.
```

https://golang.org/ref/spec#Map_types

> The comparison operators == and != must be fully defined for operands of the key type; thus the key type must not be a function, map, or slice. 

(mapの)key型に対しては、比較演算子`==`と`!=`が完全に定義されていなければなりません。従って、`function`, `map`, `slice`はkey型にはなれません。

「完全に定義」とは、その型の全ての値の組`x,y`について`x == y`と`x != y`が定義される、という意味だと思います。比較可能性の記述から、これらの型は`nil`とだけ比較可能なので、`==`と`!=`はfully definedではなく、mapのkeyの型になることができません。

よって、このコードはbuild errorとなります。
</details>

## Q3

https://play.golang.org/p/RO66rrOj-lR

```go
package main

import (
	"fmt"
)

func main() {
	m1 := map[int]int{}
	var m2 map[int]int
	fmt.Println(m1 == m2)
}
```

<details>
<summary>Answer</summary>

build error

```
./prog.go:12:17: invalid operation: m1 == m2 (map can only be compared to nil)

Go build failed.
```

- https://golang.org/ref/spec#Comparison_operators

> Slice, map, and function values are not comparable. However, as a special case, a slice, map, or function value may be compared to the predeclared identifier nil. 

Slice, map, functionの値は比較可能ではありません。ただし、slice, map, functionの値は、事前宣言された名前である`nil`と比較することができます。

この記述を見ると、"predeclared indentifierのnilとだけ比較できる"と限定していることがわかります。
map型のzero valueは`nil`ですが、これは事前宣言された名前としての`nil`ではなく、`m1`と`m2`は比較可能ではありません。

よってこのコードはbuild errorとなります。

</details>

## Q4

```go
package main

import (
	"fmt"
)

func main() {
	m := map[interface{}]int{}
	m[1] = 1
	m["2"] = 2
	fmt.Println(m[1] + m["2"] + m[3] + m[m[1]])
}
```

<details>

> If the key type is an interface type, these comparison operators must be defined for the dynamic key values; failure will cause a run-time panic.

- https://golang.org/ref/spec#Comparison_operators

> Interface values are comparable. Two interface values are equal if they have identical dynamic types and equal dynamic values or if both have value nil.

</details>

## Q5

https://play.golang.org/p/rxg-0l0lEhO

```go
package main

import (
	"fmt"
)

func main() {
	m := make(map[interface{}]int)
	var a [3]int
	var b [3]int
	m[a] = 1
	fmt.Println(m[b])
}
```

<details>
answer: 1

- https://golang.org/ref/spec#Comparison_operators

> Array values are comparable if values of the array element type are comparable. Two array values are equal if their corresponding elements are equal.

slice型と異なり、array型の値は比較可能です。2つの配列の値は、対応する要素がそれぞれ等しいときに等しいです。

上のコードの`a, b`はいずれも`[3]int{0, 0, 0}`のような配列なので、`a==b`は`true`です。よって、出力は1となります。

</details>

## Q6

https://play.golang.org/p/jn8_pZar43L

```go
package main

import (
	"fmt"
)

func main() {
	m := map[chan int]int{}
	m[nil] = 1
	fmt.Println(m[nil])
}
```
