# Introduction to Scheme

The examples are from [Scheme and the Art of Programming](https://www.cs.unm.edu/~williams/cs357/springer-friedman.pdf) book.

Scheme is a small (# of features) yet powerful language. It artfully uses abstraction to manage complexity and achieve generality. All objects may be named; all objects are first class.

Data in Scheme and symbolic, numerical, or logical. An expression can be a symbol, or a number, or a list.

## Numbers and Symbols

```
> 10
10
> ten
. . ten: undefined;
 cannot reference an identifier before its definition
> (define ten 10)
> ten
10
```
In this example "ten" is a variable to which a meaning (value) is given. We say the variable is __bound__ to that value.

```
(define var expr)
```
The define expression is made up of the "define" keyword, a variable name "var", and an expression "expr".

We can ask Scheme not to evaluate "ten" but to print its literal value - ten by quoting the symbol.
```
> (quote ten)
ten
> 'ten
ten
> 'abc13
abc13
> '12
12
```

We can assign a variable a value that is the literal value of a symbol.
```
> (define Robert 'Rob)
> Robert
Rob
```

## Arithmetic on Numbers
To perform the arithmetic operations on numbers, Scheme uses *prefix* notation.
```
> (+ 3 4)
7
> (* 3 (- 12 5))
21
> (+ 2 (/ 30 15))
4
> (+ 1 2 3 4 5)
15
```

## Lists
A list is a collection of items, which can be numbers, symbols, or lists. Scheme provides a procedure (constructor) to build list one element at a time. An empty list is denoted as `()`.
```
> (cons 1 '())
(1)
```
This expression encloses three things in parentheses: the variable **cons**, which tells the name of the procedure we are applying, the **1** and the **'()'**, which are the two operand the procedure is operating on.

```
> (define ls1 (cons 1 '()))
> ls1
(1)
> (cons 2 ls1)
(2 1)
> ls1
(1)
> (define ls2 (cons 2 ls1))
> ls2
(2 1)
> ls1
(1)
> (define c 'three)
> (cons c ls2)
(three 2 1)
```

```
> (1 2)
. . application: not a procedure;
 expected a procedure that can be applied to arguments
  given: 1
  arguments...:
```
We tried to treat the list as an application but its first item is an number 2.

```scheme
> '((2 1) three 2 1)
((2 1) three 2 1)
> '(a b (c (d e)))
(a b (c (d e)))
> (cons '(a b) '(c (d e)))
((a b) c (d e))
```
We can enter a list of items that is to be taken literally. If the expression is parentheses is quoted, Scheme assumes that each item is to be taken literally.

## Taking Lists Apart
We use two selector procedures, `car` and `cdr` to decompose lists. Both of them take a non-empty list as the operand.
```scheme
> (car '(a b (c (d e))))
a
> (car '(1 2 3 4))
1
> (car '(a b c d))
a
> (car '((1) (2) (3) (4)))
(1)
> (car '(1))
1
> (car '((1 2)))
(1 2)
> (car '(()))
()
> (car (car '((1 2))))
1
```
When its argument is a non-empty list, `car` returns the first item in the list.

```
> (cdr '(1 2 3 4))
(2 3 4)
> (cdr '(a (b c) (d e f)))
((b c) (d e f))
> (cdr '((ant hill) (bee hive) (wasp nest)))
((bee hive) (wasp nest))
> (cdr '(1))
()
> (cdr '((1 2)))
()
> (cdr '(()))
()
```
When its argument is a non-empty list, `cdr` returns a list, which is the argument list with its first item removed.

```
> (car (cdr '(a b c d)))
b
> (define list-of-names '((Jane Doe) (John Jones)))
> (car (cdr (car list-of-names)))
Doe
> (cadar list-of-names)
Doe
```
When procedures `car` and `cdr` are applied in succession a number of times, they can be abbreviated, e.g. `(cadr '(a b c)')` is equivalent to `(car (cdr '(a b c)')))`.
