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

## 1.9

```lisp
(define (+ a b)
  (if (= a 0) b (inc (+ (dec a) b))))
```

(I've forgot to depict the `dec` expressions)

```
(+ 4 5)
(inc (+ 3 5))
(inc (inc (+ 2 5)))
(inc (inc (inc (+ 1 5))))
(inc (inc (inc (inc (+ 0 5)))))
(inc (inc (inc (inc (5)))))
(inc (inc (inc (6))))
(inc (inc (7)))
(inc (8))
9
```

```lisp
(define (+ a b)
  (if (= a 0) b (+ (dec a) (inc b))))
```

```
(+ 4 5) 
(+ (dec 4) (inc 5)) 
(+ 3 6) 
(+ (dec 3) (inc 6)) 
(+ 2 7) 
(+ (dec 2) (inc 7)) 
(+ 1 8) 
(+ (dec 1) (inc 8)) 
(+ 0 9) 
9
```

As seen above, the first process is recursive (it defers the operation of inc).
The second process is iterative as the state is known at any point.

## 1.10

I've didn't just plugged into the interpreter, instead tried to get the idea
behind it. Checkout the Wikipedia page for Ackermann function's, it's really
beautiful: https://en.wikipedia.org/wiki/Ackermann_function

Answers are:


```lisp
(A 1 10)
```

This definition expands into `(A (0) (A (0) (A (0) ...)))` up to 10 times. The
latest expression will be `(A 1 1)` which evaluates to 2. So this means we are
multiplying the value `2` up to 10 times. So it's basically `2 ^ 10` -> `1024`


```lisp
(A 2 4)
```

This definition evaluates into `(A 1 (A 1 4))` after a couple of iterations.
Because we know that `(A 1 N)` is equal to `2 ^ N` (from the previous
procedure), this simplifies to `(A 1 16)` (because (A 1 4) is equal to `2^4` ->
16). So basically  we end up with `(A 1 16)`, which is `2 ^ 16` -> `65536`


```lisp
(A 3 3)
```

This definition evaluates into `(A 2 (A 2 (A 3 1)))` after a couple of
iterations. Which is then evaluated as `(A 2 (A 2 2))`. Evaluating `(A 2 2)`
gives us 4, so the final form is `(A 2 4)` which is the same as the previous
example. So the result is `2 ^ 16` -> `65536`


Define the following procedures as mathematical definitions:


```lisp
(define (f n) (A 0 n)) 
```
This is very easy because it always calls the expression `2*y`, so this is mathematically: `2n`


```lisp
(define (g n) (A 1 n)) 
```
This is also known (from the previous definitions), we can define it as: `2^n`


```lisp
(define (h n) (A 2 n)) 
```

We have already two solutions from the above, adding the rest we got

```
(A 2 1) -> 2
(A 2 2) -> 4
(A 2 3) -> 16
(A 2 4) ->  65536
```

Looking carefully we can see this all about the power of two. So it's basically: `2^2^2...` n times

All the definitions above are invalid of `n < 0` and `0` for the case `n = 0`

## 1.11

Recursive process

```lisp
(define (f n)
  (cond ((< n 3) n)
        (else (+ (f (- n 1))
                 (* 2 (f (- n 2)))
                 (* 3 (f (- n 3)))))))
```


For iterative process, we do the same as it's been done previously with the
Fibonnaci example. In this cases the states are:

a <- a + 2b + 3c
b <- a
c <- b

We shortcut for cases `n < 3`, for any other the iterative process (the
`f-iter` procedure) takes effect.

```lisp
(define (f n)
  (if (< n 3)
      n
      (f-iter 2 1 0 n)))

(define (f-iter a b c count)
  (if (= count 2)
      a
      (f-iter (+ a ( * 2 b) (* 3 c)) a b (- count 1))))
```

## 1.12

I understand that by computing the element it means to get an element from a
given row and column. The solution below assumes the row and column index
starts from `0` (convention from: https://en.wikipedia.org/wiki/Pascal's_triangle)

The degenerate cases are:

* We assume every negative column index to be `0` (so each `0` index colum can
  produce `1`)
* We assume every column index greater than row index to be `0` (so each `row
  == col` case produces a `1`)
* We assume that column 0 is always 1. This is our state value which carried
  with each recursive process


```lisp
(define (pascal row col)
  (cond ((< row col) 0)
        ((< col 0) 0)
        ((= col 0) 1)
        (else (+
               (pascal (- row 1) (- col 1))
               (pascal (- row 1) col)))))
```

## 1.13

1. Fib(n) is the closest integer to `q^n /  sqrt 5`. So if we pass `1` and `2` for
`n`, and divide each other we get `q`:

```
Fib(2)/Fib(1) = q
```

So this means basically

```
Fib(n + 1)/Fib(n) = q
```

As seen below, with every high `n` value it satisfies the equation

```
(/ (fib 8) (fib 7))   -> 1.5384
(/ (fib 10) (fib 9))  -> 1.617647
(/ (fib 11) (fib 10)) -> 1.618
```


2. I've skipped the induction proof as it should be straight forward to assume
   for certain cases of n (`0`, `1`, `2`, etc..) and do the proof.


## 1.14

Below is an ASCII drawing of `(count-change 11 5)`. This is taken from another
[place](https://github.com/martinblech/sicp/blob/master/01/1.14.scm) as I did
draw it on a paper. As seen there are four possibilities:

```
10 + 1
5 + 5 + 1
5 + 1 + 1 + 1 + 1 + 1 + 1
1 + 1 + 1 + 1 + 1 + 1 + 1 + 1 + 1 + 1 + 1
```

Diagram:

```
; cc 11 5
;    |    \
; cc 11 4  cc -39 5
;    |    \
; cc 11 3  cc -14 4
;    |    \----------------------------------------\
; cc 11 2                                           cc 1 3
;    |    \------------------------\                   |   \
; cc 11 1                           cc 6 2          cc 0 3  cc -9 3
;    |    \                            |   \---------------\
; cc 11 0  cc 10 1                  cc 6 1                  cc 1 2
;             |    \                   |   \                   |   \
;          cc 10 0  cc 9 1          cc 6 0  cc 5 1          cc 0 2  cc -4 2
;                      |   \                   |   \
;                   cc 9 0  cc 8 1          cc 5 0  cc 4 1
;                              |   \                   |   \
;                           cc 8 0  cc 7 1          cc 4 0  cc 3 1
;                                      |   \                   |   \
;                                   cc 7 0  cc 6 1          cc 3 0  cc 2 1
;                                              |   \                   |   \
;                                           cc 6 0  cc 5 1          cc 2 0  cc 1 1
;                                                      |   \                   |   \
;                                                   cc 5 0  cc 4 1          cc 1 0  cc 0 1
;                                                              |   \
;                                                           cc 4 0  cc 3 1
;                                                                      |   \
;                                                                   cc 3 0  cc 2 1
;                                                                              |   \
;                                                                           cc 2 0  cc 1 1
;                                                                                      |   \
;                                                                                   cc 1 0  cc 0 1
```

The question is asking for:

1. Order of growth of the space (Space complexity): The order of growth is
   increasing linearly because the depth of the tree is increasing linearly.
   Basically we only keep track of the trail nodes. So answer is `Θ(n)`

2. Number of steps used as the amount to be changed increases (Time
   complexity): This requires a more formal mathematic proof. But looking
   carefully and we can see from the diagram that there is a new leaf for each new
   first kind of coin.  So basically because we have `5` kinds of coins, it'll be
   `Θ(n^5)` I've found a better explanation here too:
   http://www.billthelizard.com/2009/12/sicp-exercise-114-counting-change.html

## 1.15

a.) `p` will be applied 5 times

b.) First of all because the procedure is a recursive process, the interpreter
only keeps a fixed result to be applied to `p`. It also requires a fixed number
of steps. The order of growth (space complexity) is increasing logarithmic
(just like a binary-tree search), as we divide it each time until we stop when
the absolute difference of a is smaller than `0.1` So it is `theta(a)`. The
number of steps (time complexity) is also the same (logarithmic increase)

## 1.16

I had the solution like below (without using `a` at all)

```lisp
(define (square n) (* n n))
         
(define (expt b n)
  (if (even? n)
      (fast-expt-iter b n)
      (* b (fast-expt-iter b (- n 1)))))

(define (fast-expt-iter b counter)
  (if (= counter 1)
      b
      (fast-expt-iter (square b) (/ counter 2))))
```

However I believe the solution above is not iterative process, because for a
non even (odd) case it stores the `b` before it continue. I'm not quite sure
here, so I would be happy if anyone reading this can clarify it.

Another solution would be using `cond` and doing the converting of `odd ->
even` via the `a` parameter. This is much more elegant of course and truly
iterative.

```lisp

(define (square n) (* n n))
         
(define (expt b n) (fast-expt-iter 1 b n))

(define (fast-expt-iter a b counter)
  (cond ((= counter 0) a)
        ((even? counter) (fast-expt-iter a (square b) (/ counter 2)))
        (else (fast-expt-iter (* a b) b (- counter 1)))))
```



## 1.17

Below is a recursive process of using `double` and `halve` to implement
multiplication in logarithmic number of steps.

```lisp
(define (double n) ( + n n)) ;; I assume * is not available
(define (halve n) (/ n 2))

(define (* a b)
  (cond ((= b 1) a)
        ((even? b) (double (* a (halve b))))
        (else (+ a (double (* a (halve (- b 1))))))))
```

Just discovered that the else branch can be simplified:

```lisp
(define (* a b)
  (cond ((= b 1) a)
        ((even? b) (double (* a (halve b))))
        (else (+ a (* a (- b 1))))))
```

## 1.18

A iterative process using `double`, `halve` which uses logarithmic number of
steps. So doubling the input size will increase the computing step only by one.
The below procedures are similar to the previous `fast-expt` procedures which
uses the `invariant quantity` described in the book.

```lisp
(define (double n) ( + n n)) ;; I assume * is not available
(define (halve n) (/ n 2))

(define (* a b) (mul-iter 0 a b))

(define (mul-iter a x y)
  (cond (( = y 0) a)
        ((even? y) (mul-iter a (double x) (halve y)))
        (else (mul-iter (+ a x) x (- y 1)))))
```
  

## 1.19

The substitution is below:

![1.19](assets/1_19.png)

So this means we have the following for `p'` and `q'`:

```
p' = p^2 + q^2
q' = 2pq + q^2
```

So the final solution would be:

```lisp
(define (fib n)
  (fib-iter 1 0 0 1 n))

(define (fib-iter a b p q count)
  (cond ((= count 0) b)
        ((even? count)
         (fib-iter a
                   b
                   (+ (* p p) (* q q))
                   (+ ( * 2 p q) (* q q))
                   (/ count 2)))
        (else (fib-iter (+ (* b q) (* a q) (* a p))
                        (+ (* b p) (* a q))
                        p
                        q
                        (- count 1)))))
```


## 1.20

The procedure is like below:

```lisp
(define (gcd a b)
  (if (= b 0)
      a
      (gcd b (remainder a b))))
```

Now using first `normal-order evaluation`

```
(gcd 206 40)
  (if (= 40 0)
      a
	  (gcd 40 (remainder 206 40))

(gcd 40 (remainder 206 40)
  (if (= (remainder 206 40) 0)) ;; 6
      40
	  (gcd (remainder 206 40) (remainder 40 (remainder 206 40))))


(gcd (remainder 206 40) (remainder 40 (remainder 206 40))
  (if (= (remainder 40 (remainder 206 40)) 0) ;; 4
      (remainder 206 40)
      (gcd (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40))))))

      
(gcd (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))
  (if (= (remainder (remainder 206 40) (remainder 40 (remainder 206 40))) 0) ;; 2
      (remainder 40 (remainder 206 40))
      (gcd (remainder (remainder 206 40) (remainder 40 (remainder 206 40))) (remainder (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))))))


(gcd (remainder (remainder 206 40) (remainder 40 (remainder 206 40))) (remainder (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40))))
  (if (= (remainder (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))) 0) ;; 0
      (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))
      (gcd ...)))
```

So now for every `if` branch we evaluate the `predicate` expression first and
then decide if whether to evaluate the `consequent` or the `alternative`
expression. This means every if `predicate` is evaluated until it's equal to
`0` and we exit with evaluating the `consequent`. Now counting all `if`
predicate expressions until it's hit `0`:

```
1 -> (if (= (remainder 206 40) 0))
2 -> (if (= (remainder 40 (remainder 206 40)) 0)
4 -> (if (= (remainder (remainder 206 40) (remainder 40 (remainder 206 40))) 0)
7 -> (if (= (remainder (remainder 40 (remainder 206 40)) (remainder (remainder 206 40) (remainder 40 (remainder 206 40)))) 0)
```

So from the `if` forms we got total: `14`

After that what left is the consequent expression which is:

```lisp
(remainder (remainder 206 40) (remainder 40 (remainder 206 40)))
```

From here we have total: `4` So the final answer for normal-value evaluation
is: **18**


Now let use `applicative-order evaluation`. This is much more easier to
illustrate as we evaluate the expression each time:

```lisp
(define (gcd a b)
  (if (= b 0)
      a
      (gcd b (remainder a b))))
```

```
(gcd 206 40)
(gcd 40 (remainder 206 40))
(gcd 40 6)
(gcd 6 (remainder 40 6))
(gcd 6 4)
(gcd 4 (remainder 6 4))
(gcd 4 2)
(gcd 2 (remainder 4 2))
(gcd 2 0)
2
```

So for `applicative-order evaluation` we evaluate `remainder` in total **4** times.


## 1.21

Find the smallest divisor of the following numbers:


```lisp
(smallest-divisor 199)
(smallest-divisor 1999)
(smallest-divisor 19999)
```

Answer:

```
199
1999
7
```

## 1.22

Below is an implementation of `search-for-primes`, which takes a range and
iterates on only odd integers. We substract `-2` from b so we only iterate
between the rage, example input `100,104` would cause to only process `101`
(`round(a)` is equal to 101) till to `103` (`round(b)` gives 105, so we down
round it to the nearest odd number).

```lisp
(define (round n)
  (if (divides? 2 n)
      (+ n 1)
      n))
   
(define (search-for-primes a b)
  (define (iterate a b)
    (timed-prime-test a)
    (if (< a b) (iterate (+ a 2) b)))
  (iterate (round a) (- (round b) 2)))
```

To the the smallest primer larger than `1000` we define the range between
`1000` and `1100` (it could be larger until we find it, but this is enough and
even large). So for each number we choose only the minimum to capture the
smallest primer after the given number

For `1000` we have:

```
1009 *** 3
1013 *** 3
1019 *** 3
```

For `10,000` we have:

```
10007 *** 9
10009 *** 9
10037 *** 8
```

For `100,000` we have:

```
100003 *** 25
100019 *** 24
100043 *** 24
```

For `1,000,000` we have:

```
1000003 *** 89
1000033 *** 72
1000037 *** 72
```

As seen from the results the time is increasing in `sqrt n`, where n is `10`.
So every time we increase the input by `10` the time increase by `sqrt 10`, so
roughly by `~3.16`

## 1.23

The `next` procedure and the modified `find-divisor` procedure is like below:

```
(define (next n)
  (if (= n 2)
      3
      (+ n 2)))

(define (find-divisor n test-divisor)
  (cond ((> (square test-divisor) n) n)
        ((divides? test-divisor n) test-divisor)
        (else (find-divisor n (next test-divisor)))))

(define (smallest-divisor n) (find-divisor n 2))
```

After implementing it we test it for the following prime numbers:

```lisp
(timed-prime-test 1009)
(timed-prime-test 1013)
(timed-prime-test 1019)
(timed-prime-test 10007)
(timed-prime-test 10009)
(timed-prime-test 10037)
(timed-prime-test 100003)
(timed-prime-test 100019)
(timed-prime-test 100043)
(timed-prime-test 1000003)
(timed-prime-test 1000033)
(timed-prime-test 1000037)
```

The results are:

```
1009 *** 3
1013 *** 3
1019 *** 2
10007 *** 5
10009 *** 6
10037 *** 6
100003 *** 17
100019 *** 17
100043 *** 17
1000003 *** 51
1000033 *** 51
1000037 *** 51
```

It didn't run twice as fast. Comparing with the previous example we can see
that it improved by a factor of **1.5** (not *2*).

This is because previously the `if` form in the `next` procedure still
evaluates the equalness of the input number to `2`, which adds an overhead
generally.


## 1.24

This depends heavily on how many times we call `fast-primes?`. Because it's a
probabilistic algorithm, it depends on how many times we want fetch. For `times
= 5` we get the results:

```
1009 *** 9
1013 *** 9
1019 *** 10
10007 *** 18
10009 *** 11
10037 *** 13
100003 *** 15
100019 *** 14
100043 *** 16
1000003 *** 17
1000033 *** 17
1000037 *** 18
```

For `times = 10` we get:

```
1009 *** 16
1013 *** 18
1019 *** 18
10007 *** 29
10009 *** 21
10037 *** 22
100003 *** 27
100019 *** 36
100043 *** 27
1000003 *** 28
1000033 *** 28
1000037 *** 29
```

And for `times = 20` we get:

```
1009 *** 32
1013 *** 33
1019 *** 34
10007 *** 49
10009 *** 38
10037 *** 62
100003 *** 48
100019 *** 50
100043 *** 49
1000003 *** 54
1000033 *** 55
1000037 *** 57
```

Now the change on the input every `10` factor (1000, 10000, 100000, etc...)
changes logarithmicly.  Every time we increase the input by a factor of `10`,
the increase in time complexity will be very very low. A multiplication in
logarithmic sense is just a addition. So from the data above we can see every
time the input increases by a factor 10, we only increase it by `1` (Because
`log 10` is equal 1). 

However as you see the difference is not `1` because we also execute it many
times to get a better probability and proofnes of proving the given number to
be a prime.  

## 1.25

It is wrong. Testing it with the expmod (let's call this expmod2) based on
fast-expt we can see that after 4-5 couple of iterations it's getting to slow,
so slow that it eats all the memory due the immense computing need. 

This is because of taking the square of different values.

1. The original `expmod` takes the square of ever decreasing values. It
   basically takes the square of **remainders** so the calculations is more
   faster

2. The second `expmod2` takes the square of ever increasing values. Instead of
   remainders, it takes the square of square numbers, which is increasing
   quadratic. 


## 1.26 

Because we are now evaluation the expression: `(expmod base (/ exp 2) m)` twice
instead of once. Each evaluation causes to split another evaluation. Basically
this is a tree recursion and grows exponentially. So the input increased to `2
^ n`. But because we use the modulo operation of multiplication, which is
`O(logn)`. Due the input it's now `O(log 2 ^ n)` which is `n * log 2`. So the
order of growth is: `O(n)`


## 1.27

Instead of random numbers we need to test for integers satisfying `a < n`.
Below is an implementation of it:

```lisp
(define (fermat-test-all n)
  (define (try a n)
    (cond ((= a 0) true)
          ((= (expmod (- a 1) n n) (remainder (- a 1) n)) (try (- a 1) n ))
          (else false)))
  (try (- n 1) n))
```
        
The following Carmichael numbers all returns true for all values `a < n`.

```
(fermat-test-all 561)
(fermat-test-all 1105)
(fermat-test-all 1729)
(fermat-test-all 2465)
(fermat-test-all 2821)
(fermat-test-all 6601)
```

Based on these fact we can conclude that the numbers above fool the Fermat test

## 1.28

A check for `nontrivial square root of 1 modulo n` would be:

```lisp
(define (signal-check x n)
  (if (and (not (or (= x 1) (= x (- n 1))))
           (= (remainder (* x x) n) 1))
      0
      (remainder (* x x) n)))
```

As stated in the question, we signal with `0` if we detect a a nontrivial
square root of 1. If it's not we do the regular modulo of the given number's
square.

Incorparating this into `expmod` gives us:

```lisp
(define (expmod base exp m)
  (cond ((= exp 0) 1)
        ((even? exp)
         (signal-check (expmod base (/ exp 2) m) m))
        (else
         (remainder (* base
                       (expmod base (- exp 1) m))
                    m))))
```

So we can check a single random number with:

```lisp
(define (miller-rabin-test n)
  (define (try-it a)
    (= (expmod a (- n 1) n) 1))
  (try-it (+ 1 (random (- n 1)))))
```

To increase the probability, let us use `fast-prime?` with the new `miller-rabin-test` procedure:

```lisp
(define (fast-prime? n times)
  (cond ((= times 0) true)
        ((miller-rabin-test n) (fast-prime? n (- times 1)))
        (else false)))
```

Testing it with the following expressions:

```
(fast-prime? 561 10)
(fast-prime? 1105 10)
(fast-prime? 1729 10)
(fast-prime? 2465 10)
(fast-prime? 2821 10)
(fast-prime? 6601 10)
```

We can see that it always returns `false`, so Carmichael numbers can't fool it.

## 1.29

Below is the Simpson's rule implementation. A couple of things here:

* Using `define h` simplifies the code tremendous
* We split the original formula into four pieces to be able to use the `sum`
  procedure. The first and last functions calls are added manually, anything
  between is grouped into the multiplies of `2` and `4`. For `2` the function
  begins with `a + 2h` and increases with `2h`. For `4`, it's the same as the
  previous one, just it begins with `a + h`
* The last term can be computed for both `2` and `4` series when we supply
  `n-2` and `n-1` for k. Example computation for series `2` would be: 
  `(a +kh) = (a + (n-2)h) = (b - 2h)`

```lisp
(define (simpson-rule f a b n)
  (define h (/ (- b a) n))
  (define (add-h x) (+ x (* 2 h)))
  (* (/ h 3) (+
              (f a)
              (* 2 (sum f (+ a (* 2 h)) add-h (- b (* 2 h))))
              (* 4 (sum f (+ a h) add-h (- b h)))
              (f (+ a (* n h))))))
```

For the `n = 100` and `n = 1000`:

```lisp
(simpson-rule cube 0 1 100)
(simpson-rule cube 0 1 1000)
```

It results as:

```
#e0.25
#e0.25
```

Compared to the previous `integral` procedure, this is much more better.

## 1.30

The iterative process solution can be seen below:

```lisp
(define (sum-iter term a next b)
  (define (iter a result)
    (if (> a b)
        result
        (iter (next a) (+ (term a) result))))
  (iter a 0))
```


## 1.31

a.)

The procedure `product` is as follow (recursive process implementation):

```lisp
(define (product term a next b)
  (if (> a b)
      1
      (* (term a)
         (product term (next a) next b))))
```

This is the same as the `sum` procedure with two changes:

1. Instead of returning `0` we return `1` for the consequent result of the if
   predicate. This is important because multiplying something with `0` at the
   end would yield a wrong result.
2. Instead of adding the `(term a)`, we multiply it.

An example usage of a procedure calculating the products of a given range would
be:

```lisp
(define (product-integers a b)
  (product identity a inc b))

(product-integers 2 5)
;; gives 2.3.4.5 -> 120
```

Using the a slightly modified version of above, we can simply implement the
factorial procedure as:


```lisp
(define (factorial n)
  (product identity 1 inc n))
```

The approximations of pi according to the formula can be implemented as
followed (where `approx > 0`):

```lisp
(define (pi approx)
  (define (inc-two n) (+ n 2))
  (define (product-integers-two a b)
    (product identity a inc-two b))
  ( * 4
      ( / (* (product-integers-two 4 (+ 4 (* 2 approx)))
             (product-integers-two 2 (+ 2 (* 2 approx))))
          (square (product-integers-two 3 (+ 3 (* 2 approx)))))))
```

The `approx` variable defines approximity of the procedure, the greater the
number is the more precise is the final answer. The trick here is to split the
numerator into two parts, one which begins with 2,4,6,... and the other which
begins with 4,6,8,..
 


b.)
 
Our first implementation was based on `recursive process`, so we implement the
`iterative process` as seen below:

```lisp
(define (product-iter term a next b)
  (define (iter a result)
    (if (> a b)
        result
        (iter (next a) (* (term a) result))))
  (iter a 1))
```

Note that this nearly identical to the iterative implementation of `sum`
procedure (as expected).

## 1.32

a.)

The implementation of accumulate is below (which is recursive process):

```lisp
(define (accumulate combiner null-value term a next b)
  (if (> a b)
      null-value
      (combiner (term a) (accumulate combiner null-value term (next a) next b))))
```


Based on the `accumulate` procedure we can implement both `product` and `sum` as below:

```lisp
(define (product term a next b)
  (accumulate * 1 term a next b))

(define (sum term a next b)
  (accumulate + 0 term a next b))
```


b. )

The iterative process implementation is as followed:

```lisp
(define (accumulate-iter combiner null-value term a next b)
  (define (iter a result)
    (if (> a b)
        result
        (iter (next a) (combiner (term a) result))))
  (iter a null-value))

```

## 1.33

The `filtered-accumulate` procedure can be written as:

```lisp
(define (filtered-accumulate filter combiner null-value term a next b)
  (if (> a b)
      null-value
      (if (filter a)
          (combiner (term a) (filtered-accumulate filter combiner null-value term (next a) next b))
          (filtered-accumulate filter combiner null-value term (next a) next b))))
```

An example of summing even numbers for a given range could be:

```lisp
(define (sum-odd-integers a b)
  (define (even? n) (= (remainder n 2) 0))
  (filtered-accumulate even? + 0 identity a inc b))

(sum-odd-integers 1 4) ;; 2 + 4 -> 6
```

a.)

The implementation would be (we assume we have a `prime?` procedure, you can
easily implement it with our previous implementations, such as using the
fast-divisor procedure or fermat's test):


```lisp
(define (sum-square-prime-numbers a b)
  (filtered-accumulate prime? + 0 square a inc b))
```

Using this as:

```
(sum-square-prime-numbers 1 10)
```

would yield: `87`

b.)

The implementation can be written as:

```lisp
(define (product-relative-primes n)
  (define (relative-gcd? x) (= (gcd x n) 1))
  (filtered-accumulate relative-gcd? * 1 identity 1 inc n))
```

where the gcd procedure is already known as:

```lisp
(define (gcd a b)
  (if (= b 0)
      a
      (gcd b (remainder a b))))
```

For example:

```
(product-relative-primes 10)
```

would procedure `189`, which is correct because there are four relatives to
`10`, which are:

`gcd(1,10)`, `gcd(3,10)`, `gcd(7,10)` and `gcd(9,10)`, so `1*3*7*9` -> `189`

## 1.34

Asking the interpreter to evaluate:

```
(f f)
```

We'll get an error. The reason is `f` is excepting a procedure. However in

```lisp
(define (f g)
  (g 2))
```

we pass the number `2` to `f`. So `f` is expecting a procedure, however it
doesn't get it.  When run it, the error message is also obvious about it:

```
. . application: not a procedure;
 expected a procedure that can be applied to arguments
  given: 2
  arguments.:
```

It can be seen clearer with the order of evaluation:

```
(f f)
(f 2)
(2 2) ;; fails here!
```
