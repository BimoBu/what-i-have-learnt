# Pointers

A pointer is a variable that holds the memory adress of a value. The pointer to a value with type `T` has the type `*T`. The zero value of a pointer is `nil`.

Pointers to values are generated using the `&` operator.

    var p *int
    i := 42
    p = &i

To access the underlying value of a pointer, use the `*` operator.

    fmt.Println(*p) // read i through the pointer p
    *p = 21         // set i through the pointer p

