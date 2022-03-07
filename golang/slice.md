# slice


``` go
runtime.go

type slice struct {
    array unsafe.Pointer
    len   int
    cap   int
}
```

������go��û��`����`����,slice��һ��`struct`,�������һ����������һ��slice,ʵ����һ��copy��
�¹����slice��ʵ��ӵ��ָ��ͬһ���ײ������`ָ��`,�����ں����ڲ�����slice�ǻ�Եײ���������޸ĵģ�
�����������,�����ڲ���slice�Ὣpointer�滻Ϊ������ĵ�ַ��

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

������������,�����:
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
������������,�����:
```json
slice a: [1]
slice b: [1 0]
```