# Just some notes, when I finish the go folder these are not needed

- Type assertion für interface: t, ok := i.(T)
    - Wenn ok weggelassen wird und der type nicht stimmt: panic
- Type von interface holen: v := i.(type)
    - Nur in Switch möglich (type switch)
    - V wird dann zu dem value und Typen, den i hält
- Generic functions: func funcName[T <constraint>](<params>) {}
- Generic types: type List[T <constraint>] struct {}
- Concurrency
    - Goroutine starten mit: go f(x, y, z)
    - Synchronisierung von goroutines über channel
    - Elemente im channel abfangen mit: for i := range c {}
    - close(c) beendet loop, keine Elemente kommen mehr
    - for { select { case <- c: … default: …}} 
    - sync.Mutex
- Mehrere Parameter mit: func funcName(params …int) {}
- Array zu mehreren Parametern umformen: funcName(array…)
- Struct embedding