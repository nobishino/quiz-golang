# Channel Types

https://play.golang.org/p/y3-pHLbiiOA

## Q1

```go
package main

import (
	"fmt"
)

func main() {
	ch := make(chan int, 2)
	ch <- 1
	ch <- 1
	<-ch
	fmt.Println(len(ch))
}
```