# Debugger

The missing tool in the ecosystem

## Usage

[![Clojars Project](http://clojars.org/debugger/latest-version.svg)](http://clojars.org/debugger)

```
user=> (dotimes [n 5] (debugger.core-test/foo n))

Break from: /Users/razum2um/Code/debugger/src/debugger/core_test.clj:12 (type "(help)" for help)

   17:         e (fn [] nil)
   18:         x "world"
   19:         y '(8 9)
   20:         z (Object.)
=> 21:         ret (break (inc 42))]
   22:     (println "Exit foo with" ret)))

debugger.core-test/qux:31=> (h)

 (h)    (help)          prints this help
        (wtf)           prints short code of breakpointed function
        (wtf??)         prints full code of breakpointed function
 (l)    (locals)        prints locals
 (c)    (continue)      continues execution, preserves the result and will break here again
        (skip 3)        skips next 3 breakpoints in this place
 (q)    (quit)          or type Ctrl-D to exit break-repl, pass last result further, will never break here anymore

                        use (debugger.core/reset-skips!) if breaks are skipped
                        you can also access locals directly and build sexp with them
                        any last execution result (but `nil`) before exit will be passed further
                        if last result is `nil` execution will continue normally

nil
```

## Locals access

```
debugger.core-test/foo:21=> (l)
{x "world",
 a [1 2],
 y (8 9),
 args (0),
 e #<core_test$foo$e__3434 debugger.core_test$foo$e__3434@4adf5792>,
 ...}
nil
debugger.core-test/foo:21=> z
#<Object java.lang.Object@3dc76ae9>
```

## Code expection

```
debugger.core-test/foo:21=> (whereami)

   12: (defn foo [& args]
   13:   (let [a [1 2]
   14:         b #{3 4}
   15:         h {:k "v"}
   16:         d nil
   17:         e (fn [] nil)
   18:         x "world"
   19:         y '(8 9)
   20:         z (Object.)
=> 21:         ret (break (inc 42))]
   22:     (println "Exit foo with" ret)))

nil
```

## Stack expection

```
debugger.core-test/foo:21=> (wtf??)

  [0]                      core_test.clj:21     debugger.core-test/foo
  [1]                        RestFn.java:408    clojure.lang.RestFn
  [2]   form-init1204084863726891616.clj:1      user/eval3682
  [3]                      Compiler.java:6703   clojure.lang.Compiler
  [4]                      Compiler.java:6666   clojure.lang.Compiler
  [5]                           core.clj:2927   clojure.core/eval
  [6]                           main.clj:239    clojure.main/repl
  [7]                           main.clj:239    clojure.main/repl
  [8]                           main.clj:257    clojure.main/repl
  [9]                           main.clj:257    clojure.main/repl
 [10]                        RestFn.java:1523   clojure.lang.RestFn
 [11]             interruptible_eval.clj:67     clojure.tools.nrepl.middleware.interruptible-eval/evaluate
 [12]                           AFn.java:152    clojure.lang.AFn
 [13]                           AFn.java:144    clojure.lang.AFn
 [14]                           core.clj:624    clojure.core/apply
 [15]                           core.clj:1862   clojure.core/with-bindings*
 [16]                        RestFn.java:425    clojure.lang.RestFn
 [17]             interruptible_eval.clj:51     clojure.tools.nrepl.middleware.interruptible-eval/evaluate
 [18]             interruptible_eval.clj:183    clojure.tools.nrepl.middleware.interruptible-eval/interruptible-eval
 [19]             interruptible_eval.clj:152    clojure.tools.nrepl.middleware.interruptible-eval/run-next
 [20]                           AFn.java:22     clojure.lang.AFn
 [21]            ThreadPoolExecutor.java:1142   java.util.concurrent.ThreadPoolExecutor
 [22]            ThreadPoolExecutor.java:617    java.util.concurrent.ThreadPoolExecutor/Worker
 [23]                        Thread.java:745    java.lang.Thread

nil

```

## Break on next `(break)` or skip it

```
debugger.core-test/foo:21=> (c)
43
Exit foo with #<Object java.lang.Object@3dc76ae9>

Break from: /Users/razum2um/Code/debugger/src/debugger/core_test.clj:12 (type "(help)" for help)

   17:         e (fn [] nil)
   18:         x "world"
   19:         y '(8 9)
   20:         z (Object.)
=> 21:         ret (break (inc 42))]
   22:     (println "Exit foo with" ret)))

debugger.core-test/foo:21=> (skip 3)
nil
Exit foo with 43
Exit foo with 43
Exit foo with 43

Break from: /Users/razum2um/Code/debugger/src/debugger/core_test.clj:12 (type "(help)" for help)

   17:         e (fn [] nil)
   18:         x "world"
   19:         y '(8 9)
   20:         z (Object.)
=> 21:         ret (break (inc 42))]
   22:     (println "Exit foo with" ret)))

debugger.core-test/foo:21=> (c)
43
Exit foo with 43
nil
```

## Acknoledgements

- @richhickey for `clojure.main/repl`
- @GeorgeJahad for the lesson [how to preserve locals](https://github.com/GeorgeJahad/debug-repl/blob/master/src/alex_and_georges/debug_repl.clj#L68)

## License

Copyright © 2014 Vlad Bokov

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.
