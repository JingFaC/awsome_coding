# slice


``` go
runtime.go

type slice struct {
    array unsafe.Pointer
    len   int
    cap   int
}
```

首先在go中没有`引用`传递,slice是一个`struct`,所以如果一个函数传递一个slice,实际是一份copy。
新构造的slice和实参拥有指向同一个底层数组的`指针`,所以在函数内部更改slice是会对底层数组进行修改的；
如果发生扩容,则函数内部的slice会将pointer替换为新数组的地址。

```go
package main

import "fmt"

func test_slice(a []int) {
	a = append(a, 10)
}

func main() {
	slice_a := make([]int, 0, 2)
	slice_a = append(slice_a, 1)

	test_slice(slice_a)

	fmt.Println("slice a:", slice_a)
	slice_b := slice_a[:2]

	fmt.Println("slice b:", slice_b)
}
```

考虑上面的情况,输出是:
```json
slice a: [1]
slice b: [1 10]
```

```go
package main

import "fmt"

func test_slice(a []int) {
	a = append(a, []int{1, 2, 3, 4}...)
}

func main() {
	slice_a := make([]int, 0, 2)
	slice_a = append(slice_a, 1)

	test_slice(slice_a)

	fmt.Println("slice a:", slice_a)
	slice_b := slice_a[:2]

	fmt.Println("slice b:", slice_b)
}
```
考虑上面的情况,输出是:
```json
slice a: [1]
slice b: [1 0]
```