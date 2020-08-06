# Variables

- https://golang.org/ref/spec#Variables

## Problem 1 

- [Playground](https://play.golang.org/p/FQ-lNJ_GgPM)

```go
package main

import (
	"fmt"
)

func main() {
	var x int = 1
	var y *int
	var z [1]int
	z[0] += x
	*y = x + z[0]
	fmt.Println("y =", y)
}
```

<details>
    <summary>
        ANSWER
    </summary>

- Runtime panic
- 参考Spec:
  - https://golang.org/ref/spec#Variables
  - https://golang.org/ref/spec#Address_operators

- [式の中で変数を参照すると、変数の値を得ることができて、その値は、その変数に最後に代入された値となります。一度も代入されたことがない場合、得られる値はその型のゼロ値となります。](https://golang.org/ref/spec#Variables)

> A variable's value is retrieved by referring to the variable in an expression; it is the most recent value assigned to the variable. If a variable has not yet been assigned a value, its value is the zero value for its type.

- var y は一度も代入されておらず、またyの型は`*int`です。`*int`のゼロ値は`nil`です。
- [yがnilである時、 `*y` を評価すると、nil pointer dereferenceによりpanicが起こります。](https://golang.org/ref/spec#Address_operators)

>  If x is nil, an attempt to evaluate *x will cause a run-time panic.

- よって、問題のコードでは`nil`である`y`に対して`*y`を評価しているので、panicが起こります。

</details>