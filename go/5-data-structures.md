# Structs

Structs are types that hold a set of named values.

Definition of a struct:

    type Vertex struct {
        X int
        Y int
    }

To create a variable with that type:

    v := Vertex{1, 2}

Here the order of fields is important. X now has the value 1 and Y has value 2. It is also possible to assign values directly using the names of the attributes:

    v := Vertex{Y: 2, X: 1}

Accessing attributes with dot notation:

    v.X = 4
    y = v.Y

When using a pointer to access a structs value, it is not necessary to explicitly dereference (`(*p).X`), but you can just write use `p.X` for assigning and reading values.

When exporting a struct by giving its name a capital first character, it is important to also give the attributes that should be exported a capital first letter.

# Slices

It is uncommon to use (fixed-size) arrays in go. Instead, slices are dynamically sized and more commonly used.

## Slicing an array

You can create a slice from an array:

    primes := [6]int{2, 3, 5, 7, 11, 13}
    var a []int = primes[1:4] // [3 5 7]

Creating a new slice from an existing slice works the same way.

You can omit the values for the high and low bounds when slicing. Then 0 is assumed for the low bound and the length of the slice/array for the high bound.

    s := []int{2, 3, 5, 7, 11, 13}
    s = s[1:4]  // [3 5 7]
	s = s[:2]   // [3 5]
	s = s[1:]   // [5]

## Slices as references to arrays

When changing slice created from an array, you also change the array (and all other slices from that array). Slices are references to arrays.

    var b []int = primes[0:3] // [2 3 5]
    a[0] = 99
    fmt.Println(a, b, primes) // [99 5 7] [2 99 5] [99 3 5 7 11 13]

## Initialization

Initializing a slice with a list of values:

    q := []int{2, 3, 5, 7, 11, 13}
    s := []struct {
		i int
		b bool
	}{
		{2, true},
		{3, false},
		{5, true},
		{7, true},
		{11, false},
		{13, true},
	}

## Length and capacity

The length of a slice is the number of elements it contains. The capacity is the number of elements of the underlying array, counting from the slices first element.

    s := []int{2, 3, 5, 7, 11, 13} // len(s)=6 cap(s)=6 [2 3 5 7 11 13]
	// Slice the slice to give it zero length.
	s = s[:0] // len(s)=0 cap(s)=6 []
	// Extend its length.
	s = s[:4] // len(s)=4 cap(s)=6 [2 3 5 7]
	// Drop its first two values.
	s = s[2:] // len(s)=2 cap(s)=4 [5 7]

## Using `make()` to create a slice

The `make` function takes a type and a number of integers as its parameters to create an instance with the given type (more on that later). When creating a slice, the first element is the type, the second is the length and the third the capacity of the slice (capacity can be omitted).

    b := make([]int, 2, 5) // len=2 cap=5 [0 0]

## Appending values to a slice

The `append` function is used to append values to a slice. If the underlying array is too small, a new array is allocated.

    var s []int
	// append works on nil slices (nil is the zero value for slices).
	s = append(s, 0)
	s = append(s, 1, 2, 3, 4)

If you want to append all elements of an array or slice to another slice, you can use the following syntax:

    s1 := []int{2, 3, 5}
    s2 := []int{7, 11, 13}
    s3 := append(s1, s2...)

This works because `append()` is a variadic (more on that on the chapter on functions) and the "unpack" syntax `s2...` turns the array `s2` into the individual elements, passing those to `append`. So `append(s1, s2...)` is functionally identical to `append(s1, s2[0], s2[1], s2[2])` as `s2` has three elements.

## Looping over slices

To loop over a slice without the traditional for loop, you can use the range form of the for loop:

    for i, v := range arr {
		// Some code using i and v
	}

Where i is the index of the element and v is a copy of the value of that element.

If you don't need to use the index, you can replace `i` with `_`, which tells the compiler that you don't use this variable, so that it doesn't create an error for an unused variable.

If you don't need the value but just the index, you can simply write

    for i := range arr {
		// Some code using i
	}

If you want to have the value by reference, this is one way to achieve that:

    for i := range arr {
        v := &arr[i]
		// Access arr[i] through the pointer v
	}

# Maps

Maps are used to map keys to values. The `make` function is used to create maps (here a map that maps strings on to ints).

    m := make(map[string]int)

The keys of the map are accessed with `[<key>]`

    m["Simon"] = 194
    height := m["Simon"] // 194

A map can also be initialized directly with predefined values.

    m := map[string]int{
        "Simon": 194,
        "Leandra": 164,
    }

When you map for example strings to structs, the type name can be omitted.

    type Vertex struct {
        Lat, Long float64
    }

    var m = map[string]Vertex{
        "Bell Labs": {40.68433, -74.39967},
        "Google":    {37.42202, -122.08408},
    }

Use the `delete` function to delete elements from a map.

    delete(m, "Google")

Use a two-value assignment to check if a key is present in the map.

    elem, ok = m[key]

`ok` is true if the map has the key. If it does not, elem has the zero value for the specific type. Here again, you can use `_` to discard the element if you only need the info if the map has the key: `_, ok = m[key]`

