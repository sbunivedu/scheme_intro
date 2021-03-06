# Introduction to Scheme

The examples are from [Scheme and the Art of Programming](https://www.cs.unm.edu/~williams/cs357/springer-friedman.pdf) book.

Scheme is a small (# of features) yet powerful language. It artfully uses abstraction to manage complexity and achieve generality. All objects may be named; all objects are first class.

Data in Scheme and symbolic, numerical, or logical (boolean). An expression can be a symbol, or a number, or a list.

## Numbers and Symbols

```scheme
> 10
10
> ten
. . ten: undefined;
 cannot reference an identifier before its definition
> (define ten 10)
> ten
10
```
In this example "ten" is a variable - a *symbol* to which a meaning (value) is given. We say the variable is __bound__ to that value.

```scheme
(define var expr)
```
The define expression is made up of the "define" keyword, a variable name "var", and an expression "expr".

We can ask Scheme not to evaluate "ten" but to print its literal value - ten by quoting the *symbol*. There are two ways to quote a symbol.
```scheme
> (quote ten)
ten
> 'ten
ten
> 'abc13
abc13
> '12
12
```
Whether a symbol is bound to some value or not, when a quoted symbol is entered, the literal symbol is returned. It is not necessary to quote numbers, for the value of a number as an expresssion is the number itself.

We can assign a variable a value that is the literal value of a symbol.
```scheme
> (define Robert 'Rob)
> Robert
Rob
```

## Arithmetic on Numbers
To perform the arithmetic operations on numbers, Scheme uses *prefix* notation.
```scheme
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
```scheme
> (cons 1 '())
(1)
```
This expression encloses three things in parentheses: the variable **cons**, which tells the name of the procedure we are applying, the **1** and the **'()'**, which are the two operand the procedure is operating on.

```scheme
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

```scheme
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

```scheme
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

```scheme
> (car (cdr '(a b c d)))
b
> (define list-of-names '((Jane Doe) (John Jones)))
> (car (cdr (car list-of-names)))
Doe
> (cadar list-of-names)
Doe
```
When procedures `car` and `cdr` are applied in succession a number of times, they can be abbreviated, e.g. `(cadr '(a b c)')` is equivalent to `(car (cdr '(a b c)')))`.

```scheme
> (cons 1 2)
(1 . 2)
> (cons 'a 'b)
(a . b)
> (cons 'a '(b))
(a b)
> (car '(a . b))
a
> (cdr '(a . b))
b
> (cons 'a (cons 'b '()))
(a b)
> '(a . (b))
(a b)
```

Up to now we have assumed that the second argument to `cons` is a list. If it is not a list, we can still apply `cons`; the result is not a list but rather a *dotted pair*, which is written as a pair of objects, separated by a dot and enclosed in parentheses. The `car` of a dotted pair is the first object in the pair and `cons` gives the second object. A list is composed of dotted pairs, whose `cdr` is always a list (an empty list is a list).

## Predicates
Scheme uses `#t` to denote true and `#f` to denote false. They are constant boolean values. They form a separate type of data to give us five distinct types: numbers, symbols, booleans, pairs (including lists), and procedures. Let's look at several predicates that apply to these five data types.
```scheme
> (define num 35.4)
> (define twelve 'dozen)
> (number? -45.67)
#t
> (number? '3)
#t
> (number? num)
#t
> (number? twelve)
#f
> (number? 'twelve)
#f
> (number? (+ 2 3))
#t
> (number? #t)
#f
> (number? (car '(15.3 -31.7)))
#t
> (number? (cdr '(15.3 -31.7)))
#f
> (symbol? 15)
#f
> (symbol? num)
#f
> (symbol? 'num)
#t
> (symbol? twelve)
#t
> (symbol? 'twelve)
#t
> (symbol? #f)
#f
> (symbol? (car '(banana cream)))
#t
> (symbol? (cdr '(banana cream)))
#f
> (boolean? #t)
#t
> (boolean? (number? 'a))
#t
> (boolean? (cons 'a '()))
#f
> (pair? '(Ann Ben Carl))
#t
> (pair? '(1))
#t
> (pair? '())
#f
> (pair? '(()))
#t
> (pair? '(a (b c) d))
#t
> (pair? (cons 'a '()))
#t
> (pair? (cons 3 4))
#t
> (pair? 'pair)
#f
> (list? '(1 2 3))
#t
> (null? '())
#t
> (null? (cdr '(cat)))
#t
> (null? (car '((a b))))
#f
```

There is also a predicate `procedure?` which tests whether its argument is a procedure.
```
> (procedure? cons)
#t
> (procedure? +)
#t
> (procedure? 'cons)
#f
> (procedure? 10)
#f
```

There are other data types, such as strings, characters, vectors, and streams. A common question we ask is whether two objects are the same. Scheme offers several different predicates to test the sameness of its arguments.
```
> (= 3 (/ 6 2))
#t
> (= (/ 12 3) (* 2 3))
#f
> (= (car '(-1 ten 543)) (/ -20 (* 4 5)))
#t
> (= (* 2 100) 20)
#f
```

When both objects are numbers, we can use `=` to test whether they represent the same *number*.

```
> (define Garfield 'cat)
> (eq? 'cat 'cat)
#t
> (eq? Garfield 'cat)
#t
> (eq? Garfield Garfield)
#t
> (eq? 'Garfield 'cat)
#f
> (eq? (car '(Garfield cat)) 'cat)
#f
> (eq? (car '(Garfield cat)) 'Garfield)
#t
```
The predicate `eq?` tests the sameness of *symbols*. It returns `#t` if the two arguments are identical in all respects; otherwise it returns `#f`. Symbols have the property that they are identical if they are written with the same characters in the same order.

```
> (define ls-a (cons 1 '(2 3)))
> (define ls-b (cons 1 '(2 3)))
> (define ls-c ls-a)
> (eq? (cons 1 '(2 3)) (cons 1 '(2 3)))
#f
> (eq? ls-a (cons 1 '(2 3)))
#f
> (eq? ls-a ls-b)
#f
> (eq? ls-a ls-c)
#t
```
Each application of `cons` constructs a new and distinct pair. Two pairs constructed with separate applications of cons will always test `#f` using `eq?` even if the pairs they produce look alike.

When we want to include *numbers*, *symbols*, and *booleans* in the types of objects the predicate tests for sameness, we use the predicate *eqv?*.
```
> (eqv? (+ 2 3) (- 10 5))
#t
> (eqv? 5 6)
#f
> (eqv? 5 'five)
#f
> (eqv? 'cat 'cat)
#t
> (eqv? 'cat 'kitten)
#f
> (eqv? (car '(a a a)) (car (cdr '(a a a))))
#t
```

If we want a universal sameness predicate that can applied to test *numbers*, *symbols*, *booleans*, *procedures*, and *lists* (and strings, characters, and vectors), we use the predicate *equal?*.

```
> (equal? 3 (/ 6 2))
#t
> (equal? 'cat 'cat)
#t
> (equal? '(a b c) (cons 'a '(b c)))
#t
> (equal? (cons 1 '(2 3)) (cons 1 '(2 3)))
#t
> (equal? '(a (b c) d) '(a (b c) d))
#t
> (equal? '(a b c) '(a (c b)))
#f
> (equal? (cdr '(a c d)) (cdr '(b c d)))
#t
```

When comparing pairs, `equal?` test the corresponding entries.

Summary:
```
(eq? a b) => #t
  implies
    (eqv? a b) => #t
      implies
        (equal? a b) => #t

(eq? (cons a b) (cons a b)) => #f
(eqv? (cons a b) (cons a b)) => #f
(equal? (cons a b) (cons a b)) => #t
```

## Procedures
A lambda expression is a special form for defining procedures.
```
> ((lambda (item) (cons item '())) 'bit)
(bit)
> ((lambda (item) (cons item '())) (* 5 6))
(30)
> (define make-list-of-one (lambda (item) (cons item '())))
> (define make-list-of-one
    (lambda (item)
      (cons item '())))
> (make-list-of-one 'bit)
(bit)
> (define make-list-of-two
    (lambda (item1 item2)
      (cons item1 (make-list-of-one item2))))
> (make-list-of-two 'one 'two)
(one two)
```

## Conditional Expressions

```
> (define type-of
    (lambda (item)
      (cond
        ((pair? item) 'pair)
        ((null? item) 'empty-list)
        ((number? item) 'number)
        ((symbol? item) 'symbol)
        (else 'some-other-type))))
> (type-of 1)
number
> (type-of #t)
some-other-type
> (type-of (cons 1 2))
pair
```

A `cond` expression can be replaced by another special form with `if` keyword.
The syntax is as follows: (if *condition* *consequent* *alternative*)

Write a procedure that does the following: If its argument is a pair, return
the `car` of the pair. Otherwise, return the argument.
```
> (define car-if-pair
    (lambda (item)
      (cond
        ((pair? item) (car item))
        (else item))))
> (define car-if-pair
    (lambda (item)
      (if (pair? item)
          (car item)
          item)))
> (car-if-pair '(1 2 3))
1
> (car-if-pair 'abc)
'abc
> (car-if-pair '())
'()
```

The conditional expressions used in `cond` and `if` can be compound conditions connected by logical operators.
```
> (and #t #t)
#t
> (and #t #f)
#f
> (or #t #f)
#t
> (or #f #f)
#f
> (not #t)
#f
> (not #f)
#t
```

Write a procedure to test whether a list contains precisely one item, i.e.
*singleton* list. The empty list is not a pair. Thus the nonempty list whose
`cdr` is empty must contain just one item and is thus a singleton list.
```
> (define singleton-list?
    (lambda (ls)
      (and (pair? ls) (null? (cdr ls)))))
> (singleton-list? '(1))
#t
> (singleton-list? '(1 2))
#f
```

## Recursion

We can define procedures that use themselves as helping procedures.

Write a procedure that, when applied to a nonempty list, produces/returns
the last top-level item in the list.
* `(last-item '(1 2 3 4 5))` => 5
* `(last-item '(a b (c d)))` => (c d)
* `(last-item '(cat))` => cat
* `(last-item '((cat)))` => (cat)

```
> (define last-item
    (lambda (ls)
      (cond
        ((null? (cdr ls)) (car ls))
        (else (last-item (cdr ls))))))
> (last-item '(1 2 3))
3
```

To avoid testing the cases manually, we can use the "racketunit" testing
framework to automatically run all the test cases when the code is run.
```scheme
#lang racket/base

(require rackunit)

(define last-item
  (lambda (ls)
    (cond
      ((null? (cdr ls)) (car ls))
      (else (last-item (cdr ls))))))

(check-equal? (last-item '(1 2 3 4 5)) 5)
(check-equal? (last-item '(a b (c d))) '(c d))
(check-equal? (last-item '(cat)) 'cat)
(check-equal? (last-item '((cat))) '(cat))
```

Define a procedure `member?` that decides for us whether its first argument
is `equal?` to one of the _top-level_ items in the list that is the second
argument.
* `(member? 'cat '(dog hen cat pig))` => `#t`
* `(member? 'fox '(dog hen cat pig))` => `#f`
* `(member? 2 '(1 (2 3) 4))` => `#f`
* `(member? '(2 3) '(1 (2 3) 4))` => `#t`
* `(member? 'cat '())` => `#f`

```scheme
#lang racket/base

(require rackunit)

(define member?
    (lambda (item ls)
      (cond
        ((null? ls) #f)
        ((equal? (car ls) item) #t)
        (else (member? item (cdr ls))))))

(check-equal? (member? 'cat '(dog hen cat pig)) #t)
(check-equal? (member? 'fox '(dog hen cat pig)) #f)
(check-equal? (member? 2 '(1 (2 3) 4)) #f)
(check-equal? (member? '(2 3) '(1 (2 3) 4)) #t)
(check-equal? (member? 'cat '()) #f)
```

Define a procedure `remove-1st` that removes the first top-level occurrence of
a given item from a list of items.
* `(remove-1st 'fox '(hen fox chick cock))` => `(hen chick cock)`
* `(remove-1st 'fox '(hen fox chick fox cock))` => `(hen chick fox cock)`
* `(remove-1st 'fox '(hen (fox chick) cock))` => `(hen (fox chick) cock)`
* `(remove-1st 'fox '())` = > `()`
* `(remove-1st '(1 2) '(1 2 (1 2) ((1 2))))` => `(1 2 ((1 2)))`

```scheme
#lang racket/base

(require rackunit)

(define remove-1st
    (lambda (item ls)
      (cond
        ((null? ls) '())
        ((equal? (car ls) item) (cdr ls))
        (else (cons
               (car ls)
               (remove-1st item (cdr ls)))))))

(check-equal?
 (remove-1st 'fox '(hen fox chick cock))
 '(hen chick cock))
(check-equal?
 (remove-1st 'fox '(hen fox chick fox cock))
 '(hen chick fox cock))
(check-equal?
 (remove-1st 'fox '(hen (fox chick) cock))
 '(hen (fox chick) cock))
(check-equal?
 (remove-1st 'fox '())
 '())
(check-equal?
 (remove-1st '(1 2) '(1 2 (1 2) ((1 2))))
 '(1 2 ((1 2))))
```

Define a procedure `swapper`, which takes three arguments: an item `x`, and
item `y`, and a list `ls`. It builds a new list in which each top-level occurrence
of `x` is replaced by `y` and each top-level occurence of `y` in `ls` is
replaced by `x`.
* (swapper 'cat 'dog '(my cat eats dog food)) => (my dog eats cat food)
* (swapper 'john 'mary '(john loves mary)) => (mary loves john)
* (swapper 'a 'n '(b n a n a n)) => (b a n a n a)
* (swapper 'a 'b '(c (a b) d)) => (c (a b) d)
* (swapper 'a 'b '()) => ()

```scheme
#lang eopl

(require rackunit)

(define swapper
    (lambda (x y ls)
      (cond
        ((null? ls) '())
        ((equal? (car ls) x)
         (cons y (swapper x y (cdr ls))))
        ((equal? (car ls) y)
         (cons x (swapper x y (cdr ls))))
        (else (cons
               (car ls)
               (swapper x y (cdr ls)))))))

;; trace the evaluations of the swapper procedure
(trace swapper)

(check-equal?
 (swapper 'cat 'dog '(my cat eats dog food))
 '(my dog eats cat food))
(check-equal?
 (swapper 'john 'mary '(john loves mary))
 '(mary loves john))
(check-equal?
 (swapper 'a 'n '(b n a n a n))
 '(b a n a n a))
(check-equal?
 (swapper 'a 'b '(c (a b) d))
 '(c (a b) d))
(check-equal?
 (swapper 'a 'b '())
 '())
```

Write a procedure `length` that takes as its argument a list of items `ls`
and then tells how many top-level items are in the list:
* `(length '())` => 0
* `(length '(one))` => 1
* `(length '(a b c d e))` => 5
* `(length '(1 (2 3) (4 5 6)))` => 3

```
#lang eopl

(require rackunit)

(define length
  (lambda (ls)
    (cond
      ((null? ls) 0)
      (else (+ 1 (length (cdr ls)))))))

(check-equal? (length '()) 0)
(check-equal? (length '(one)) 1)
(check-equal? (length '(a b c d e)) 5)
(check-equal? (length '(1 (2 3) (4 5 6))) 3)

(trace length)
```

Define a procedure `sum` that finds the sum of items in a list:
* `(sum '())` => 0
* `(sum '(6))` => 6
* `(sum '(1 2 3 4 5))` => 15

```
#lang eopl

(require rackunit)

(define sum
  (lambda (ls)
    (cond
      ((null? ls) 0)
      (else (+ (car ls) (sum (cdr ls)))))))

(trace sum)

(check-equal? (sum '()) 0)
(check-equal? (sum '(6)) 6)
(check-equal? (sum '(1 2 3 4 5)) 15)
```

Define a procedure `list-ref`, which takes as argument a list of items `ls` and
a non-negative integer `n` and gives us the (n+1)th top-level item in `ls`:
* `(list-ref '(a b c d e f) 3)` => d
* `(list-ref '(a b c d e f) 0)` => a
* `(list-ref '(a b c) 3) ` => list-ref: Index 3 out of range in list (a b c)
* `(list-ref '((1 2) (3 4) (5 6)) 1)` => (3 4)
* `(list-ref '() 0)` => list-ref: Index 0 out of range in list ()

```scheme
#lang eopl

(require rackunit)

(eopl:error 'procedure_name "error message")


```

Define procedure `(duple n x)`, which returns a list containing `n`copies of `x`.
* `(duple 2 3)` => (3 3)
* `(duple 4 '(ho ho))` => ((ho ho) (ho ho) (ho ho) (ho ho))
* `(duple 0 '(blah))` => ()

```scheme
```

Write a function, `calculator`, which takes an infix arithmetic expression and evaluates it. For
example,
```
> (calculator 42)
42
> (calculator ’(1 + 2))
3
> (calculator ’(1 + (2 * 8)))
17
> (calculator ’((((2 + 3) * 2) / 5) + (17 - 1))
18
```
You may assume that all sub-expressions are parenthesized so that you don’t need to worry
about precedence. Also, you need only implement the four basic arithmetic functions,

```scheme
#lang eopl

(define calculator
  (lambda (exp)
    (cond
      ((not (pair? exp)) exp)
      ((eqv? (cadr exp) '+)
       (+ (calculator (car exp)) (calculator (caddr exp)))))))
```
