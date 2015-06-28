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


