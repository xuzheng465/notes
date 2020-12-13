# Set up



Chrome

`F12`

OR

`CMD + Opt + J`

Open Console



安装vim plug

在我的电脑上是

```bash
vi ~/.config/nvim/vim-plug/plugins.vim
```

```
call plug#begin('~/.config/nvim/autoload/plugged')
	" Other plugins
	Plug 'mattn/emmet-vim'

call plug#end()
```



# 变量

let声明的范围是**块作用域**， var声明的范围是**函数作用域**。



let在全局作用域中声明的变量不会成为window对象的属性。

```javascript
var name = 'Matt';
console.log(window.name); // 'Matt'
let age = 6;
console.log(window.age); // undefined
```



const 声明的作用域也是块。

如果const 变量引用的是一个对象，那么修改这个对象内部的属性并不违反const的限制

```javascript
cosnt person = {};
person.name = 'Matt'; // ok
```



# 数据类型

其中种简单数据类型，原始类型

Undedined:

​	The Undefined type has exactly one value, called **undefined**. Any variable that has not been assigned a value has the value undefined.

Null：

​	The Null type has exactly one value, called null

Boolean `true` and `false`

Number

String

Symbol:

​	The Symbol type is the set of all non-String values that may be used as the key of an Object property

Bigint?



复杂数据类型

Object： 无序名值对的集合。

ECMAScript中不能定义自己的数据类型。

`typeof`操作符下列字符串之一：

* “undefined” 表示值未定义； 

  * ```javascript
    typeof sdjflksdjfldks
    "undefined"
    ```

* "boolean" 表示值为布尔值
* “string”表示值为字符值
* “number”表示值为数值；
* object表示为对象或null；
* function表示值为函数

## Undefined类型

```javascript
let message;
// let age
console.log(message); // "undefined"
console.log(age); // 报错
```

声明了但未定义，值为undefined

未声明直接输出 报错

```javascript
let message;
// let age 未声明未定义
console.log(typeof message); // "undefined"
console.log(typeof age); // "undefined"
```

:exclamation::exclamation::exclamation:注意

即使未初始化变量被自动赋与undefined值，但我们仍然建议在声明变量的同时进行初始化。这样，当typeof返回undefined时，你就会知道那是因为给定的变量尚未声明，而不是声明了但未初始化。



## Null类型

Null只有一个值， null。

null值表示一个空对象指针。

在定义将来要保存对象的变量时，建议使用null来初始化。

这样只要检查这个变量的值是不是null就可以知道这个变量是否在后来被重新赋予了一个对象的引用。

## Number 类型

```javascript
console.log(0/0); // NaN
console.log(-0/+0); // NaN

console.log(5/0); // Infinity
console.log(5/-0); // -Infinity

console.log(NaN == NaN); // false
```

isNaN() 函数。该函数接收一个参数，可以是任意数据类型，然后判断这个参数是否“不是数值”。把一个值传给isNaN（）后该函数会尝试把它转换为数值。

可以转换：如字符串“10” 或布尔值

任何不能转换为数值的值都会导致这个函数返回true

isNaN(NaN) // true

isNaN(10) // false

isNaN("10") // false

isNaN("blue") // true

isNaN(true). // false







