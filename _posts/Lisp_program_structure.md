# LISP - Program Structure

# LISP - 程序结构

LISP expressions are called symbolic expressions or s-expressions. The s-expressions are composed of three valid objects, atoms lists and strings.

LISP表达式被称为符号表达式或S-表达式. S表达式有3个有效的对象组成: 原子，表和字符串.

Any s-expression is a valid program.

任何一个S表达式都是一个有效的程序

LISP programs run either on an interpreter or as compiled code.

LISP程序运行在解释器或编译后的代码

The interpreter checks the source code in a repeated loop, which is also called the read-evaluate-print loop (REPL). It reads the program code, evaluates it, and prints the values returned by the program.

解释器在一个循环中检查源代码，它也被称为交互式解释器(REPL),它读取程序代码-执行它-并输出程序的返回值.

## A Simple Program

## 一个简单的程序

Let us write an s-expression to find the sum of three numbers 7,9 and 11. To do this, we can type at the interpreter prompt.

让我们来写一个求7+9+11值的S表达式.我们可以在解释器里面输入这个表达式

```
(+ 7 9 11)
```

LISP返回的值:

```
27
```

If you would like to run the same program as a compiled code, then create a LISP source code file named myprog.lisp and type the following code in it.

如果你想以已编译的代码运行这个程序, 你应该创建一个LISP源代码文件并把代码写在里面.

```
(write (+ 7 9 11))
```

When you click the Execute button, or type Ctrl+E, LISP executes it immediately and the result returned is 

当你点击执行按钮或者使用Ctrl+E快捷键时，它会立刻执行它并返回结果,就像这样:

```
27
```

## LISP Uses Prefix Notation

## LISP使用前缀表示法

You might have noted that LISP uses prefix notation.

你可能会注意到LISP使用前缀表示法

In the above program the + symbol works as the function name for the process of summation of the numbers.

在上面的程序中加法运算符以函数方式执行，并求和

In prefix notation, operators are written before their operands.For example, the expression,

在前缀表示法中，运算符都写在它的操作数之前，像这样：

```
a * ( b + c ) / d
```

will be written as :

使用前缀表示法可以写成这样:

```
(/ (* a (+ b c) ) d)
```

Let us take another example, let us write code for converting Fahrenheit temp of 60oF to the centigrade scale

让我们常试其他例子, 写一个华氏温度转换摄氏温度的一段代码 

The mathematical expression for this conversion for this conversion will be

这是温度转换的数学表达式

```
(60 * 9 / 5) + 32
```

Create a source code file named main.lisp and type the following code in it.

创建一个名为main.lisp的源代码文件并把下面的代码输入:

```
(write(+ (* (/ 9 5) 60) 32))
```

When you click the Execute button, or type Ctrl+E, LISP executes it immediately and the result returned is :

当你点击执行按钮或者使用Ctrl＋e快捷键时, LISP会马上执行它并返回结果:

```
140
```

## Evaluation of LISP Programs
## LISP程序评估

Evaluation of LISP programs has two parts:

评估LISP程序有两部分:

* Translation of program text into Lisp objects by a reader program

* 一个阅读程序翻译程序文本至LISP对象

* Implementation of the semantics of the language in terms of these objects by an evaluator program

* 实现语义的一个评估者程序

The evaluation process take the following steps :

评估过程有以下几步:

* The reader translates the strings of characters to LISP object or s-expressions.

* 阅读程序翻译字符串至LISP对象或S表达式

* The evaluator defines syntax of Lisp forms that are built from s-expressions. This second level of evaluation defines a syntax that determines which s-expressions are LISP forms.
 
* 第一部分评估者定义LISP语句格式为S表达式, 第二级别评估定义了LISP语句是哪个S表达式.

* The evaluator works as a function that take a valid LISP form as an argment and returns a value. This is the reason why we put the LISP expression in parenthesis, because we are sending the entire expression form to the evaluator as arguments.

* 评估者像函数一样工作, 我们把表达式像参数一样输入解释器.它从一个有效的LISP语句中获得参数然后返回一个值. 这就是为什么我们把LISP表达式放在括号里.

## The 'Hello World' Program
## helloworld 程序

Learning a new programming language dosen't really take off until you learn how to greet the entire world in that language right!

学习一门新的编程语言，直到你学习如何用该语言来迎接进入世界，才真正入门

So please create new source code file named main.lisp and type the following code in it.

所以请创建一个名为main.lisp的源代码文件,并输入以下内容:

```
(write-line "Hello World)
(write-line "Iam at 'Tutorials Point'! Learning LISP")
```

When you click the Execute button, or type Ctrl+E, LISP executes it immediately and the result returned is:

当你点击执行按钮或使用Ctrl+E 快捷键时, LISP将立刻执行它并返回结果:

```
Hello World
I am at 'Tutorials Point'! Learning LISP
```










