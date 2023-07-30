#software #go

reference
https://www.youtube.com/watch?v=GtsSzbs-xb8

# Common Problems

1. Slices don’t always deep copy the array.

```go
a := []int{0, 0}
a = append(a, 0)  // [0, 0, 0]
b := a[:]         // [0, 0, 0]
a = append(a, 1)  // [0, 0, 0, 1]
b = append(b, 9)  // [0, 0, 0, 9]
fmt.Println(a[3]) // 9 <- footgun!

c := []int{0, 0}
c = append(c, 0)
d := c[:len(c):len(c)] // deep copy
c = append(c, 1)
d = append(d, 9)
fmt.Println(c[3]) // 1 <- correct  
```

1. Pointers can change address.

```go
func main() {
	var value int
	println(&value) // 0xc000040768
	f(10000)
	println(&value) // 0xc00015ff68
}

func f(i int) {
	if i--; i == 0 {
		return
	}
	f(i) 
}

/* Go doesn't have a dedicated stack; 
The recursions stack run in the heap,
so if you need more stack, the program
will copy the stack to a larger memory
slice and adjust all the pointers
*/
```

1. Don’t add an entry/ entries to a map while you’re iterating over it. The program may panic as it may try to iterate over and transform the newly added entry.
2. Close over variable

```go
for i := 0; i <= 9; i++ {
	go func() {
		fmt.Println(i) // close over variable
	}()
}
time.Sleep(9)
// result: print "10" 10 times

// go vet will give warning
```

1. Don’t give the return value the same name as the variables inside the function.

```go
func f() (cancel func()) {
	_, cancel = context.WithCancel(context.TODO())
	cancel2 := func() {
		cancel()
		println("cancelled")
	}
}

func main() {
	f()()
	// f() returns cancel instead of cancel2 and calls itself
	// runtime: goroutine stack exceeds 1000000000-byte limit
}
```

1. Locking with mutex doesn’t guarantee rollback when functions can panic. Prefer creating a `startTransaction()` method that creates a backup of the previous state and lock the data, `commitTransaction()` that is put under all the functions to change a state variable to `true`, and defer `endTransaction()` to release the lock if commit is `true`, otherwise rollback to backup.

# Recommendations

1. code review
2. pair programming
3. tooling
4. write test immediately
5. fuzzing
6. use `go vet` to avoid surprises