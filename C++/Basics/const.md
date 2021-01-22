# const

**A way to commit to the compiler, you won't change something**

- When declaring a local variable
  - `int const zero = 0;`
- As a function parameter
  - `int foo(int const i)`
  - `int something(Person const& p)`
- Modifier on a member function
  - `int GetName() const;`



**Use const after**



`const int ci = 3;`



Good readability



## const with indirection

* You can declare that the **pointer points at something const**

`int const * cpI`

* Then you can't use it to change the value of the target

不能用`cpI` 去改变它指向的那个值。

:crossed_swords: `*cpI = 2;`



* You can declare the pointer to be const

`int * const cPi;`

那么这个`cPi`就不能再指向别的地方，它所指的值可以改变。



:crossed_swords: `cPi = &something;`



* both const; const in both ways

`int const * const crazy;`







