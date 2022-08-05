# Code organization

Go code is organized in packages. A module can contain multiple packages. A workspace can contain multiple modules.

Previously, it was necessary to put all GO code inside GOPATH. Since Go 1.18, that is not needed or recommended anymore. Now, it is possible to create a go module anywhere or group many modules in a workspace that can be located anywhere on the system.

## Packages

The files inside a directory normally belong to one package. All the files that belong to one package have the line 

    package <package-name>

at the very top. Everything declared within a package is automatically accessible within all other files of the same package. Identifiers with a first capital letter are exported (e.g. `ExportedFunction`) while identifiers starting with a lower-case letter are not exported (e.g. `nonExportedFunction`).

The files in subdirectories are not part of the same package, even if they use the same package name. In order to use the exported identifiers from the subpackage or parent package, you have to import that package first. You can not import the subpackage into the parent package and the parent package into the subpackage though, as circular imports are forbidden by the compiler.

### Importing packages

Importing packages works using the import statement:

    import (
        "fmt"

        "<module-name>/<path-to-package>"
    )

This example imports the "fmt" package from the system library and the custom package that is located in the directory `<path-to-package>`. Note that the import path to the custom package is not actually a file path to the directory where the package is located but a combination of the module that the package is located in and the path to the package relative to the root of that module. That means that the package names are not used to import the packages, but just the directory names that they reside in.

If a package is nested within another package, you can import that package normally using the path to that package, e.g. `<module-name>/package-1/subpackage`.

You then use the exported name of the package to reference the exported members of that package. If the subpackage directory contained a package called `justAnotherSubpackage`, you could reference an exported identifier of that package using `justAnotherSubpackage.ExportedFunction()`.

You can provide an alias for a package by putting that alias before the import path:

    import (
        "fmt"

        thisIsAnAlias "<module-name>/<path-to-package>"
    )

When referencing the indentifiers exported by this package you then use this alias: `thisIsAnAlias.ExportedFunction()`.

## Modules

A go module is initialized using the `go mod init` command (inside the root directory of that module):

    go mod init <module-name>

Ideally, module-name is the adress of the github repo containing the module, for example `github.com/BimoBu/some-go-module`. That repo would then be used to obtain the module. For development purposes, it can be anything though, for example `example.com/some-go-module` or just `some-go-module`.

This commad creates a `go.mod` file in the directory that you call it from. That file specifies the name of the module, the required go version, required external modules and optional `replace` directives if you want to replace a module which would normally be imported via the module name with a local version of that module. When working with workspaces though, the replace directive is not needed in the `go.mod` files but can be put into the `go.work` file.

### Importing modules

To import a module (e.g. `github.com/google/uuid`), you have two options:

1. First import the module in your code (`import ( ... "github.com/google/uuid" ... )`) and then run `go mod tidy`
2. Run `go get github.com/google/uuid`

`go mod tidy` has the advantage that it imports all not yet required modules at once and also removes required modules that are not used anywhere.

If you want to import a local module, you still need to use the `import ( ... "<module-name>/<path-to-package>" ... )` syntax described above, so you do not import a module using the path to that module in the local file system. Normally, go would then try to get the module from the specified location in the `<module-name>`. To prevent that and just use the local version of the module, you need to use the `replace` directive that I have described earlier. It is a best practice though to use that `replace` directive not in the `go.mod` file of the module that you are currently working on as you would need to remove that `replace` directive when publishing your module. Instead, the workspace should be used to take care of that.   

## Workspaces

A go workspace is initialized using the go work init command (from the directory of that workspace):

    go work init <paths-to-modules>

So if you have a folder `workspace` that contains two modules `module-1` and `module-2`, the command to initialize the workspace would look like this:

    go work init ./module-1 ./module-2

The `go work init` command creates a `go.work` file that contains the following directives:

- `go`: the go version
- `use`: the modules that are contained in this workspace
- `replace`: the replace directive described earlier

To edit the workspace, you can use the `go work edit` command:

- `go work edit -use ./new-module`: add a new module to the workspace
- `go work edit -dropuse ./old-module`: remove a module from the workspace
- `go work edit -replace github.com/BimoBu/some-module=./some-module`: replace the external module `github.com/BimoBu/some-module` with the local module at `./some-module`
- `go work edit -dropreplace github.com/BimoBu/some-module`: remove the replace directive

