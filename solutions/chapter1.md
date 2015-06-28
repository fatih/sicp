### 1.1

```
-> 10
10
```

```
-> (+ 5 3 4)
12
```

```
> (- 9 1)
8
```

```
> (/ 6 2)
3
```

```
-> (+ (* 2 4) (- 4 6))
6
```

```
-> (define a 3)
-> (define b (+ a 1))
a is 3
b is 4
```

```
-> (+ a b (* a b))
19
```

```
-> (= a b)
#f
```


```
-> (if (and (> b a) (< b (* a b)))
	b 
	a)
4
```

```
-> (cond ((= a 4) 6)
	  	 ((= b 4) (+ 6 7 a))
	  	 (else 25))
16
```

```
-> (+ 2 (if (> b a) b a))
6
```

```
-> (* (cond ((> a b) a) 
       	    ((< a b) b)
            (else -1)) 
   (+ a 1))
16
```


### 1.2


```lisp
-> (/ (+ 5 4 (- 2 (- 3 (+ 6 (/ 4 5)))))
   (* 3 (- 6 2) (- 2 7)))
-37/150
```

### 1.3

```lisp
(define (square a) (* a a))
(define (square-sum a b) (+ (square a) (square b)))
(define (largest-square-sum a b c)
  (cond ((< a b) (if (< a c) (square-sum b c) (square-sum a b)))
        (else (if (< b c) (square-sum a c) (square-sum a b)))))
```


### 1.4

Explain:

```lisp
(define (a-plus-abs-b a b) 
  ((if (> b 0) + -) a b))
```

Answer:

We choose the operator based on the predicate value of the value `b` being
greater than 0 or not. If `b` is positive the operator result is `+` otherwise
it's `-`.  So our operator is the result of the compound expression, which is
then used for adding or subtracting to the operands `a` and `b`.


### 1.5

We have a test procedure below. 

```lisp
(define (p) (p)) 
(define (test x y)
  (if (= x 0) 0 y))
```

Evaluate the expression:

```lisp
(test 0 (p))
```

What behaviour will we observe with an interpreter that uses applicative-order evaluation?
What behaviour will we observe with an interpreter that uses normal-order evaluation?

Answer:

1. If it's applicate-order evaluation it will run infinitely. Because the
procedure with name (p) will recursively apply to itself (trying to evaluate
itself). So the expression `(test 0 (p))` never exists, because the sub
expression `(p)` never returns.

2. If it's normal-order evaluation, it will return the `consequent` value of
the if special form, because the argument `x` will be substituted with `0`
and the sub expression `(= x 0)` yields true, which  results in returning the
consequent value `0`

### 1.6

It's because of the evaluation strategy. `if` is a special form, so the
consequent and alternative are not evaluated until the predicate is known.
However `new-if` is a new procedure, and because Scheme uses applicate-order
evaluation, all arguments are evaluated. What this means it, it infinitely
evaluates `sqrt-iter`, which evaluates itself recursively until its out of
memory.

### 1.7

Two example of small and large numbers that makes it fails are:

```
(sqrt 0.000016)
(sqrt 81234567890123)
```

The small number is failing because the square root of `0.000016` is 0.004` and
the tolerance of `0.001` itself is enough to skew the guess, because it has
little difference with the original value.

The large number is failing because it takes a very long time to get the right
gueest to the high number of retrievals. Why? Because substraction `0.001` from
a very large number doesn't affect the result anymore due to floating-point
numbers. (think of substracting `0.0001` from infinity, nothing changes right). 

A better `good-enough?` precade would be (procedures that are changed from the
previous implementation):

```lisp
(define (sqrt-iter guess prev-guess x)
  (if (good-enough? guess prev-guess)
      guess
      (sqrt-iter (improve guess x) guess x)))

;; we can also define 0.001 as (/ guess 1000) to represent it as a fraction of
;; the guess instead of an hardcoded value
(define (good-enough? guess prev-guess)
  (< (abs (- guess prev-guess)) 0.001))

(define (sqrt x) (sqrt-iter 1.0 0.0 x))
```

This yields better results for both small and large numbers. For small numbers
it yield better results because we are not bound to the actuall result and can
iterate more precisely. For large numbers it yield better because the number of
iterations are lower than before and we are again bound to the tolerance of
`0.001`


## 1.8

All definitions with examples can be seen follow. This is exactly the same as
with `sqrt` procedure, we only changed the `improve` procedure with the
procedure given in the book. The follow definitions also includes the guess
improvement from the following example (1.7)

```lisp
(define (cbrt-iter guess prev-guess x)
  (if (good-enough? guess prev-guess)
      guess
      (cbrt-iter (improve x guess) guess x)))

(define (improve x guess)
  (/ (+ (/ x (square guess)) (* 2 guess)) 3))

(define (good-enough? guess prev-guess)
  (< (abs (- guess prev-guess)) (/ guess 1000)))

(define (square x) (* x x))
(define (abs x)
  (cond ((< x 0) (- x))
        ((> x 0) x)
        ((= x 0) 0)))

(define (cbrt x) (cbrt-iter 1.0 0.0 x))

;; examples
(cbrt 0.001)
(cbrt 27)
(cbrt 64)
(cbrt 125)
```

