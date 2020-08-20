## Calls

- https://golang.org/ref/spec#Calls

### Q1 nil function call

-[Playground](https://play.golang.org/p/WXfphaCpkES)

```go
package main

func main() {
	var f func()
	f()
}
```

<details>
<summary>
ANSWER
</summary>

- `Runtime panic`

</details>