# Loops

The only looping construct in go is the `for` loop: 

    for i := 0; i < 10; i++ {
		// Some code here
	}

For all control flow structures, the parantheses () around the expression are optional but the braces {} are required.

The equivalent of a while loop can be written like this:

    for condition {
		// Some code here
	}

Infinte loop:

    for {
        // Some code here
	}

# If

The default syntax for if statements:

    if condition {
		// Some code here
	}

Just like in a `for` statement, the if statement can start with a variable declaration:

    if v := math.Pow(x, n); v < lim {
		// Some code using v
    }

The variable declared in the if statement is only in scope inside the if statement and possible else statements.

    if v := math.Pow(x, n); v < lim {
		// Some code using v
	} else {
		// Some code using v
	}

# Switch

In a switch case, only the selected case is run, unlike in other languages where you have to explicitly `break` at the end of the case to prevent the next cases to also run

    switch os := runtime.GOOS; os {
    case "darwin":
        fmt.Println("OS X.")
    case "linux":
        fmt.Println("Linux.")
    default:
        // freebsd, openbsd,
        // plan9, windows...
        fmt.Printf("%s.\n", os)
	}

# Defer

A defer statement defers the execution of a function until the surrounding function returns.

    func deferringFunction() {
        defer fmt.Println("world")
        fmt.Println("hello")
    }

This example prints "hello world".

The deferred call's arguments are evaluated immediately, but the function call is not executed until the surrounding function returns.

Deferred function calls are pushed onto a stack. When a function returns, its deferred calls are executed in last-in-first-out order: 

    func main() {
        fmt.Println("counting")

        for i := 0; i < 10; i++ {
            defer fmt.Println(i)
        }

        fmt.Println("done")
    }

This prints:

    counting
    done
    9
    8
    7
    6
    5
    4
    3
    2
    1
    0

    