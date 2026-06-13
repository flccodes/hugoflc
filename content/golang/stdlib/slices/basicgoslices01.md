---
title: "Basic about Go Slices"
date: 2026-03-17T11:52:12-03:00
draft: false
tags: ["Go", "Array", "Slice", "Type", "Data Structure"]
---

### An Introduction to Slices

This discussion approaches `ARRAY`, `SLICE`, `Slice internals`, `Growing slices` and `A possible “gotcha”` from the source available in: [Go Slices: usage and internals](https://go.dev/blog/slices-intro)

#### ARRAY

First of all to understand `SLICE`it's necessary understand `ARRAY`in Go, since the slice type is an abstraction that comes from Go's array type. Thus the `ARRAY` `[5]int` has the type `[5]int` which represents an `ARRAY`of ten integers elements, where the array’s size is fixed and its length is part of its type.

```go
// Create a variable of the type [5]int
var array1 [5]int
	fmt.Println(array1)
	fmt.Printf("\nType: %T Len: %d\n", array1, array1)
	for i, v := range array1 {
		fmt.Printf("Index: %d Value: %d\n", i, v)
	}
```

Results:

```bash
$ go run slices1.go
[0 0 0 0 0]

Type: [5]int Len: [0 0 0 0 0]
Index: 0 Value: 0
Index: 1 Value: 0
Index: 2 Value: 0
Index: 3 Value: 0
Index: 4 Value: 0
```
For ilustration: 

```go
// Create a variable of the type [3]int
var array2 [3]int

```

The `array2`has `Type: [3]int Len: [0 0 0]`. As Go's arrays are values, the difference between `array1` and `array2` is their *type*. Further Go's arrays do not have a pointer to the first array element, like `C Language`, because in `Go Language` an array variable denotes the entire array. Finally it is import highlighting that always assign or pass around an array value will be make a copy of its elements.

The code line `fmt.Printf("Array1 address: %p, Array2 address: %p\n", &array1, &array2)` shows the two variables memory addresses: 

`Array1 address: 0x10a360ac6120`
`Array2 address: 0x10a360ac2150`

```go
// Function getArray1 returns a pointer to array1
func getArray1(a [5]int) *[5]int {
	ptr_a := &a
	return ptr_a
}

fmt.Printf("First - Array1 address within a function: %p\n", getArray1(array1))
// Output: First - Array1 address within a function: 0x302165118000

fmt.Printf("Second - Array1 address within a function: %p\n", getArray1(array1))
// Output: Second - Array1 address within a function: 0x302165118030


// The code line below shows the memory address of the pointer to array1 created within a function
fmt.Printf("Array1 address within a function: %p\n", getArray1(array1))

// Assigning the array2 to a variable

ptr_array2 := &array2
fmt.Printf("\nType: %T Len: %d ptr_Array2 address: %p\n", ptr_array2, ptr_array2, ptr_array2)
// Output: Type: *[3]int Len: &[0 0 0] ptr_Array2 address: 0x30216504e150

var array2Var [3]int
array2Var = array2
fmt.Printf("\nArray2Var - Type: %T Len: %d Array2Var address: %p\n", array2Var, array2Var, &array2Var)
// Output: Array2Var - Type: [3]int Len: [0 0 0] Array2Var address: 0x30216511a018

```
As showed the memory addreses of the variables `array1` and `array2` changed after the call within a function and assigned to other variable.

#### SLICE




#### Slice Internals



#### Growing slices (the copy and append functions)



#### A possible “gotcha



As aditional sources the article points the further reading: [Effective Go](https://go.dev/doc/effective_go.html) that  contains an in-depth treatment of [slices](https://go.dev/doc/effective_go.html#slices) and [arrays](https://go.dev/doc/effective_go.html#arrays), and the Go [language specification](https://go.dev/doc/go_spec.html) defines [slices](https://go.dev/doc/go_spec.html#Slice_types) and their [associated](https://go.dev/doc/go_spec.html#Length_and_capacity) [helper](https://go.dev/doc/go_spec.html#Making_slices_maps_and_channels) [functions.](https://go.dev/doc/go_spec.html#Appending_and_copying_slices)
