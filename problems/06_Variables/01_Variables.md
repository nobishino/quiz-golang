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

- [variableの値は、そのvariableに最後に代入された値となります。一度も代入されたことがない場合、値はその型のゼロ値となります。](https://golang.org/ref/spec#Variables)

> A variable's value is retrieved by referring to the variable in an expression; it is the most recent value assigned to the variable. If a variable has not yet been assigned a value, its value is the zero value for its type.

- var y は一度も代入されておらず、またyの型は`*int`です。`*int`のゼロ値は`nil`です。
- [nilのポインタ値に対してdereference `*` をすると、nil pointer dereferenceによりpanicが起こります。](https://golang.org/ref/spec#Address_operators)

>  If x is nil, an attempt to evaluate *x will cause a run-time panic.

- よって、`nil`である`y`に対して`*y`演算を行うと、panicが起こります。

</details>