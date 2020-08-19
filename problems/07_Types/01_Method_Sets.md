# Method Sets 

## Q1

- [playground](https://play.golang.org/p/oI--TbLjQxG)

```go
package main
 
 import "fmt"
 
 type Setter interface {
 	Set(int)
 }
 
 type T struct {
 	x int
 }
 
 func (t *T) Set(x int) {
 	t.x = x
 }
 
 func main() {
 	var i Setter
 	t := T{}
 	i = t
 	i.Set(3)
 	fmt.Println(t.x)
 }
```

<details>
<summary>ANSWER</summary>

- `build error`

- https://golang.org/ref/spec#Method_sets

> The method set of any other type T consists of all methods declared with receiver type T.
	
- https://golang.org/ref/spec#Interface_types

>A variable of interface type can store a value of any type with a method set that is any superset of the interface. 

</details>