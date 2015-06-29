Chapter notes. Partially from the book, but mostly my own words.
--

# Chapter 1

* The convention of placing the operator to the left of the operands is known
  as `prefix notation` or `prefix form`.
* Looking at evaluation strategies: https://en.wikipedia.org/wiki/Evaluation_strategy
* LISP useses applicative-order evaluation (also called call-by-value). There
  is also normal-order evaluation(called call-by-name). Applicative-order
  evaluation is more efficient, because it avoids multiple evaluations of
  expressions. It evaluates the arguments first and applies the resulting
  procedure to the resulting arguments.
* `predicate`: an expression whose value is interpreted as either true or
  false. In Scheme these are the constants #t and #f. If the interpreter checks
  a predicate's value, it interprets #f as false.
* conditional expression: The name `cond` is used, consisting of predicates. It
  continues until a predicate's value is true. If  none of the predicate's is
  true, the value of `cond` is undefined
* There are some primitive predicates, such as `>`, `<`, `=`, etc..
* It's awesome we can create predicates our self. An example from the book:

		(define (>= x y) (or (> x y) (= x y)))

* Most of the black-box examples are already well known for me
* We can encapsulate definitions under a definition. This is called `block
  structure`. We can also omit variable passing to the internal definitions, by
  just not to pass and change the formal parameters of the internal definitions.
  So if the procedure has a variable `x` and there are internal (privates)
  definitions in the block structure requring it, we just remove them. `x` will
  be then a `free variable` for the internal definitions, but a `bound variable`
  for the procedure itself. This is called `lexical scoping`. Basically in `Go`
  terms, we create a function and several anonymous functions, which all capture
  the argument for the inital function. An example for `block structure` and
  `lexical scoping` from the book is:

		(define (sqrt x)
		  (define (good-enough? guess)
			(< (abs (- (square guess) x)) 0.001)) 
		  (define (improve guess)
			(average guess (/ x guess))) 
		  (define (sqrt-iter guess)
			(if (good-enough? guess) 
			  guess
			  (sqrt-iter (improve guess))))
		  (sqrt-iter 1.0))

