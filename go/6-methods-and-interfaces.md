# Methods 

Go does not have classes, but Methods can be defined on types using a receiver argument.

    type Vertex struct {
        X, Y float64
    }

    func (v Vertex) Abs() float64 {
        return math.Sqrt(v.X*v.X + v.Y*v.Y)
    }

Methods can be defined the same way on non-struct types.

You can only declare a method with a receiver whose type is defined in the same package as the method.

## Pointer receivers

If a method needs to change the value of its receiver, the receiver needs to be a pointer. Pointer receivers are more common than value receivers. Another benefit of pointer receivers is the performance; when using a method with a value receiver, the value gets copied before every call to the method. When using a pointer receiver, that is not necessary.

    type Vertex struct {
        X, Y float64
    }

    func (v *Vertex) Scale(f float64) {
        v.X = v.X * f
        v.Y = v.Y * f
    }

In general, all methods on a given type should have either value or pointer receivers, but not a mixture of both. (See next chapter)

When writing a method with a pointer receiver as a function, you need to give a pointer as an argument:

    var v Vertex
    ScaleFunc(v, 5)  // Compile error!
    ScaleFunc(&v, 5) // OK

When using methods with pointer receivers, the conversion to pointer is done automatically for convenience:

    var v Vertex
    v.Scale(5)  // OK
    (&v).Scale(10) // OK

The same principle applies in the reverse direction:

    var v Vertex
    fmt.Println(AbsFunc(v))  // OK
    fmt.Println(AbsFunc(&v)) // Compile error!

    fmt.Println(v.Abs()) // OK
    fmt.Println((&v).Abs()) // OK


# Interfaces

An interface is a type containing a set of method signatures.

    type Abser interface {
        Abs() float64
    }

In the last chapter we said that a type should only have value receiver or only pointer receiver methods, not a mixture. That is because when implementing the Abs() method with a value receiver (v Vertex), the Abs() method is not implemented by type *Vertex, but only Vertex. 

    func (v Vertex) Abs() float64 {
        return math.Sqrt(v.X*v.X + v.Y*v.Y)
    }

    var a Abser
    a = Vertex{2, 3} // OK
    a = &Vertex{2, 3} // Error

Same thing the other way around: 

    func (v *Vertex) Abs() float64 {
        return math.Sqrt(v.X*v.X + v.Y*v.Y)
    }

    var a Abser
    a = Vertex{2, 3} // Error
    a = &Vertex{2, 3} // OK

Interface implementation in Go is always implicit. 

# TODO continue