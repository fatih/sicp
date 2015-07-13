Chapter notes. Partially from the book, but mostly my own words.
--

# Chapter 1

* The convention of placing the operator to the left of the operands is known
  as `prefix notation` or `prefix form`.
* Looking at evaluation strategies: https://en.wikipedia.org/wiki/Evaluation_strategy
* LISP uses applicative-order evaluation (also called call-by-value). There
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
  definitions in the block structure requiring it, we just remove them. `x` will
  be then a `free variable` for the internal definitions, but a `bound variable`
  for the procedure itself. This is called `lexical scoping`. Basically in `Go`
  terms, we create a function and several anonymous functions, which all capture
  the argument for the initial function. An example for `block structure` and
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

* We call processes `recursive process` if the process builds up a chain of
  deferred operations. In this case the interpreter needs to track all
  operations that needs to be track later. The longer the chain the more
  information needs to be maintained.
* We cal processes `iterative process` if the process can be described at any
  point. The state at any point can be summarized by a fixed number of state
  variables 
* IMPORTANT: Both two processes are `recursive procedures`. `Recursive
  procedure` is about the syntax, means the procedure is referring to itself,
  however `recursive process` is about how the process evolves. That's why
  `iterative` process might be seen confusing, but it's not.
* Another pattern is called `tree recursion` (example would be implementing a
  straight forward Fibonacci implementation). The number of steps required by a
  tree-recursive process will be proportional to the number of nodes in the
  three, while the space required will be proportional to the maximum depth for
  the tree.
* There are two growths, one is the order of growth in space, the other one is
  the required number of steps. They are called `space complexity` and `time
  complexity`. So the better the time complexity of an algorithm is, the faster
  the algorithm is. In `space complexity`, the number of space required is low
  the better the algorithm is.
* We can distribute a modulos operation like: `ab mod m` == `(((a mod m) (b mod m)) mod m)`
  This is uses in the `expmod` procedure. Appereantly this is also true: 
  `b ^ n mod m` == `((b * (b ^ (n - 1) mod m)) mod m)` not sure why...
* I really love the examples, each one is iterating over the previous one,
  making it enjoyable.
* I've read that one can convert every recursive function into a iterative
  function (in imperative languages) by using a `stack`. So I wonder if this so
  called `stack` is the same as `state` variable we create while converting a
  recursive process into an iterative one. Probably the so called stack is
  something where the state is stored. Perhaps I'm going to learn more in the
  latter chapters of SICP.


