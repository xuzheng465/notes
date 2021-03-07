#### Differences between Variables and Constants



First,  you aren’t allowed to use `mut` with constants. Constants aren’t just immutable by default—they’re **always** immutable.

You declare constants using the `const` keyword instead of the `let` keyword, and the type of the value *must* be **annotated**. 



`Constants` can be declared **in any scope**, including the global scope, which makes them useful for values that many parts of code need to know about.

z