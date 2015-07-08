Random thoughts while reading SICP.
--

* Installed DrRacket with SICP support from http://www.neilvandyke.org/racket-sicp/
	* There is also MIT Scheme, but people recommend nowadays DrRacket with the
	  Neil Van Dyke's SICP module.
* To use SICP support just add `#lang planet neil/sicp` to the beginning of the file
* Learned that I can open the REPL from terminal with `neil/sicp` support(for
  more info http://crash.net.nz/) :

		racket  -i -p neil/sicp -l xrepl

  I liked this better because it let me use `readline`, so all bash (emacs)
  shortcuts (like jumping to the beginning) works. Don't know if DrRacket
  support it though.
* Solutions for each chapter can be found at:
  http://community.schemewiki.org/?SICP-Solutions  Nice to cross check after
  implementing the exercises.
* Comments are written with `;;`
* Love the exercises 1.4 and 1.5. I liked 1.4 because of the ability choosing
  the operator ourselfs. And 1.5 because it shows elegantly how a language's
  evaluation strategy is defined in terms of applicate-order and name-order
  evaluation.
* Happy to finish the first sub section (1.1). 40 pages read and I really like
  it so far.
* DrRacket is so far so good. I like how it adjust the memory so it can
  overcome infinite recursive calls (of course increasing is not the solution
  :)). Because the examples or exercises grow in size I'm using more and more
  DrRacket GUI instead of opening the REPL from the terminal.
* There are video lectures from MIT (The author of SICP itself) which is
  teaching SICP to a handful of HP enginners in the 1980's. It's called: `MIT
  6.001 Structure and Interpretation, 1986` and can be find on Youtube and OCW
  (example: https://www.youtube.com/watch?v=2Op3QLzMgSY)
* Learned [Ackermann's Function](https://en.wikipedia.org/wiki/Ackermann_function). 
  It's being used to benchmark a compilers ability of optimizing recursion. The
  reason for it is the extreme deep recursion (for even very low values)
* The book get's more and more mathematical (I think this is due Harold
  Abelson's background). I like it very much as it remembers the old days in
  University while studying EE.
* The counting change example did take a lot of time to understand. It's
  actually very simple if you depict it. It's using tree recursion and shows
  how elegant (but very inefficient) it can be solved. It seems visualizations
  helps me to understand things more faster.
* Exercise 1.11 was hard to solve. I've never thought writing it similar to the
  iterative Fibonacci solution. Eventually I solved it after many hours (didn't
  gave up :). So looking afterwards I've found a great answer to it here:
  http://stackoverflow.com/a/2366209/462233
* Didn't know that the exercises would take so much time.
* I've read the paper
  http://cs.brown.edu/~sk/Publications/Papers/Published/fffk-htdp-vs-sicp-journal/paper.pdf
  which explains the tradeoff's of reading SICP and recommends instead of HTDP
  (How to design programs). For example it says that SICP is not for beginners
  as it asks questions from other domains and needs additional time to learn
  that domains knowledge. I partially agree with it as it's get more and more
  mathematical. By mathematical, I don't mean the usage of mathematical and
  well understood tools, instead I'm meaning examples about mathematical proofs
  and functions.
* Sometimes you need to reach another book so you can understand what SICP
  really means. For example "order of growth of the space" means `space
  complexity` and "the number of steps required for the process" means `time
  complexity`. I know that they are fairly the same, however in `1.2.3`, time
  complexity was not even a headline. It was just buried in a line which you
  barely think it's something important. But each additional question was about
  it. So what this means for me is, SICP wants to me research very extensively
  and do my own homework on some topics. It's not clear somehow, but I'm ok
  with it.
* Damn, now I'm thinking in terms of iterative process when trying to implement
  a recursive procedure. I've solved 1.17 with an iterative based approach, and
  1.18 was asking explicit for it, now I have to implement 1.17 again but this
  time with a recursive process.
* I've finally get the simple find-divisor procedure. I didn't sure why we
  would compare it with the square of the divisor. Until I read the note 44.
  Suddenly all was clear for me and it was very enlightening. 
* I think I'm now proficient on understanding the procedures of checking a
  numbers primality. SICP at its finest :) I mean It's really heavly based on
  math. For example I've just learned a lot about modulo operations, Fermat's
  Little Theorem, what `congruent` means, etc.. On ther other hand I really
  like it, but it's just taking a lot of time.


