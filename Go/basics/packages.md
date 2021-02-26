# package



all package files should be in the same directory



You can use `package clause` in a `file` to let *Go* Know that which `package` that file belongs to.



Each Go package has its own scope.

For example, declared `funcs` are only visible to the files belong to the main package.







### Package kinds

There are two kinds of packages in Go: `Executable` and `Library`

#### Executable package

An `executable go program` should belong to `package main`.



#### Library package

created for reusability



| Library                     | Executable              |
| --------------------------- | ----------------------- |
| created for **reusability** | created for **running** |
| non-executable              | executable              |
| importable                  | non-importable          |
| can have any name           | **name should be main** |
| **no func main**            | func main               |



