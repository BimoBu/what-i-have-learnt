# Functions

The `main` function in the `main` package is the entry point to every go program, so this function gets called first when executing the program.

Functions are composed of a list of statements without semicolons (`;`) at the end of each line.

You declare a function using the `func` keyword, followed by the name of the function, the parameter list, the return type and finally the implementation:

    func add(x int, y int) int {
	    return x + y
    }

If it is a `void` function, the return type is omitted as with main:

    func main() {
        // ...
    }

The main function cannot take any arguments, if any command line arguments are passed to a cli written in go, these arguments are accessed via the `os` package as described in [this article](https://golangdocs.com/command-line-arguments-in-golang).

You can omit the types of subsequent parameters if they are the same:

    func add(x, y int) int {
	    return x + y
    }

## Variadic functions

Sometimes, you don't want the number of parameters to be fixed to a predefined amount. Then you can define the function like so:

    func funcName(params ...paramType) returnType {}

Inside the function, you can access params like you would a `slice` or `array` (more on that in data structures). That means that you can get the number of passed params by using `len(params)`, index each parameter with `params[index]` and loop over them just like a slice or array.

## Multiple return values

You can also return multiple values from a function:

    func swap(x, y string) (string, string) {
        return y, x // the order of the returned values is important
    }

Then you can acces those values as follows:

    func main() {
        a, b := swap("hello", "world")
        fmt.Println(a, b) // world hello
    }

## Functions as values

As functions can be stored in variables.

    sqrt := func(x float64) float64 {
		return math.Sqrt(x)
	}

That means that they can also be given as a parameter to another function.

    func compute(fn func(float64, float64) float64) float64 {
        return fn(3, 4)
    }

And they can also be returned from a function.

    func createAddFunc() func(int) int {
        return func(x, y int) int {
            return x + y
        }
    }

## Closures

When you create an (enclosing) function that returns a function and use variables in the returned function that are declared in the enclosing function, that is called a closure. That way, the returned function can maintain a state that only it can access. For example, a function could count the number of times it has been called.

    func createClosure() func() {
        numberOfCalls := 0;

        return func() {
            numberOfCalls++;
            fmt.Println(numberOfCalls)
        }
    }

You can then use that function like so:

    func main() {
        closure := createClosure()
        closure() // prints 1
        closure() // prints 2
        closure() // prints 3
    }

# Variables 

You can declare variables using the `var` keyword, followed by the name of the variable, followed by the type:

    var age int

It is possible to declare many variables with the same type at once:

    var age, height, weight int

When declaring a variable like this, it is assigned the zero-value of that type (0 for numeric, false for boolean, empty string for strings).

When immediately initializing a variable with a value, the type can be omitted as it is inferred from that value and you can initialize variables with many types at once:

    var age, height, weight, male = 17, 184, 70, true

Another possibility to declare many variables at once is to group them into a block:

    var (
        ToBe   bool       = false
        MaxInt uint64     = 1<<64 - 1
        z      complex128 = cmplx.Sqrt(-5 + 12i)
    )

The `:=` shorthand can be used (only inside a function!) to declare and initialize variables with an inferred type:

    func main() {
        var i, j int = 1, 2
        k := 3
        c, python, java := true, false, "no!"

        fmt.Println(i, j, k, c, python, java) // 1 2 3 true false no!
    }

# Types

The basic types in go are:

- bool
- string
- int  int8  int16  int32  int64
- uint uint8 uint16 uint32 uint64 uintptr
- byte // alias for uint8
- rune // alias for int32, represents a Unicode code point
- float32 float64
- complex64 complex128

The syntax to convert a type is `<type>(value)`:

    i := 42
    f := float64(i)
    u := uint(f)

Assignments need to be explicit. For example, you cannot do `f = i` without the conversion to `float64`. Same goes for arithmetic operations.

When inferring the type of a variable, it is important to use the correct format:

    i := 42             // int
    j := i              // int
    f := 3.142          // float64
    c := 0.867 + 0.5i   // complex128

# Constants 

Constants are declared using the `const` keyword. The `:=` syntax is unavailable for them. 

Constants can only have character, string, boolean, or numeric values, so no more complex types.

Grouping constants into blocks also works the same as with variables:

    const (
        big = 1 << 100
        small = big >> 99
    )
