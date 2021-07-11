---
layout: post
title: "Macros in Racket: an Outline"
tags:
  - "tech"
  - "racket"
---

<div class="toc" markdown="1">

- [Introduction](#introduction)  
- [I. What to Transform: (define-syntax ...)](#what-to-transform)  
- [II. How to Transform It](#how-to-transform-it)  
	- [A. General Racket Tooling](#general-racket-tooling)  
		- [1. (require (for-syntax ...))](#require-for-syntax)  
		- [2. (begin-for-syntax ...)](#begin-for-syntax)  
		- [3. (define-for-syntax ...)](#define-for-syntax)  
	- [B. Pattern-Matching Transformers](#pattern-matching-transformers)  
		- [1. (syntax-rules ...)](#syntax-rules)  
			- [a. (define-syntax-rule ...)](#define-syntax-rule)  
		- [2. (syntax-case ...)](#syntax-case)
- [III. Other Resources](#other-resources)  

</div>

<a name="introduction"></a>

Racket has a very powerful macro system, and a flexible one, too. Learning about macros can be a little intimidating, especially if you just dive into the [Racket Reference chapter](http://docs.racket-lang.org/reference/Macros.html) on the subject! One of the best-regarded tutorials on Racket's macro system is Greg Hendershott's, which nods to this with the title [Fear of Macros](http://www.greghendershott.com/fear-of-macros/index.html). You should read it either before or after this outline.

Macros are procedures that transform a program's code. One thing to keep in mind is that macros in Racket are basically normal Racket code, but instead of operating on typical data structures or values they operate on *syntax objects*. You can think of a syntax object as an s-expression or identifier, bundled up with some information about where it's found in source.[^1] 

[^1]: The identifier or s-expression itself can be unwrapped from the source information quite easily, using the ```syntax->datum``` function.

The other key difference between macros and normal Racket code is that macros perform their operations in a separate phase of program execution. One part of working with macros is indicating to Racket what to do at *syntax phase* (or "compile time") and what to leave for run time.

Headings may be accompanied by links to the Racket \[R\]eference or \[G\]uide.

## I. What to Transform: (define-syntax ...) [R](https://docs.racket-lang.org/reference/define.html?q=define-syntax#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define-syntax%29%29) [G](http://docs.racket-lang.org/guide/pattern-macros.html#%28part._define-syntax_and_syntax-rules%29) {#what-to-transform}
Racket's primary method for introducing a macro to a program is the ```define-syntax``` function. ```define-syntax``` is a compile-time version of ```define```, and there are some notable similarities between the two. When we ```define``` something, we're telling Racket to substitute a value (or computation) wherever it sees a keyword – for example, ```(define PI 3.14)```. Similarly, ```define-syntax``` tells Racket how to substitute syntax for other syntax wherever it sees a keyword.

Here's how we write a function in runtime-world:

```racket
(define id-to-recognize
  (lambda (args)
    "return this string"))
```

And here's how we might write a function to transform syntax:

```racket
(define-syntax id-to-recognize
  (lambda (label-for-the-syntax-object) 
    (syntax "the-syntax-object was replaced with this")))
```

They look awfully similar, because a macro is really just a function that accepts and returns syntax objects. In both cases, Racket will look for the specified identifier, and will replace it with the result of the procedure we specify.

One difference is in what the function expects to receive: the normal run-time method call passes its arguments – the remainder of the contents of the s-expression – to the `lambda`. The macro passes the entire syntax object – the whole s-expression, bundled up with information about its source and scoping. We can then access the s-expression, and pass it around to be manipulated, using the label we defined. Here, though, we're just replacing the s-expression with a pre-defined string.[^2]

[^2]: I've been writing as though an s-expression must be involved. This isn't true – both run-time functions and macros can identify and replace "naked" identifiers.

Just like the regular ```define```, ```define-syntax``` comes with a shorthand form so that we don't have to write ```lambda``` when specifying the transformation function:

```racket
(define-syntax (id-to-recognize label-for-the-syntax-object)
  (syntax "the-syntax-object was replaced with this"))
```

As mentioned above, we aren't actually doing anything with the syntax object that `define-syntax` grabs – we're just sending back a string.

## II. How to Transform It {#how-to-transform-it}

Next up, let's look at the tools available to carry out transfomations on syntax.

### A. General Racket Tooling [G](http://docs.racket-lang.org/guide/macro-transformers.html) {#general-racket-tooling}

We can work with syntax objects using the same functions as we're used to anywhere else. We could use `syntax->list` to convert our s-expression syntax object to a list of syntax objects within the expression, for example, and then use `car` and `cdr` to slice and dice it.

Because syntax transformation takes place in a separate phase than normal code, though, we may need to tell Racket explicitly that we want normal code tools around for that phase – all Racket automatically makes available during syntax phase is `racket/base`.

Each of these are ways to include or create functionality we need within our transformation function. They are defined outside of a macro itself, though – don't include them within `define-syntax`.

#### 1. (require (for-syntax ...)) {#require-for-syntax}

Just like you'd expect, this makes the specified modules available during the syntax phase, so we can their functions within the transformation function in `define-syntax`.

We can write our own modules and `require` them `for-syntax`, as well.

#### 2. (begin-for-syntax ...) [R](https://docs.racket-lang.org/reference/begin.html?q=for-syntax#%28form._%28%28quote._~23~25kernel%29._begin-for-syntax%29%29) {#begin-for-syntax}

Anything included within this s-expression is evaluated at syntax phase, and in sequence. This allows us to define helper functions *without* having to put them in a separate module that we then `(require (for-syntax ...))`:

```racket
(begin-for-syntax
  (define (a-helper args) ...)
  (define (another-helper) ...))
```

#### 3. (define-for-syntax ...) [R](http://docs.racket-lang.org/reference/define.html?q=define-for-syntax#%28form._%28%28lib._racket%2Fprivate%2Fbase..rkt%29._define-for-syntax%29%29) {#define-for-syntax}

This allows us to jump right into defining a helper function for syntax phase. It's the same thing as `(begin-for-syntax (define ...))`.

### B. Pattern-Matching Transformers {#pattern-matching-transformers}

Manually breaking down syntax objects and operating on them could be a real pain. Instead, it's handy to name each part of our syntax object and then to compose our new syntax using a template that refers to those names. Racket has robust pattern-matching tools for the syntax phase. Some of these are used as the transformer function within `define-syntax`; others are used in conjunction with it or in place of it entirely.

#### 1. (syntax-rules ...) [R](http://docs.racket-lang.org/reference/stx-patterns.html?q=syntax-rules#%28form._%28%28lib._racket%2Fprivate%2Fstxcase-scheme..rkt%29._syntax-rules%29%29) [G](http://docs.racket-lang.org/guide/pattern-macros.html#%28part._define-syntax_and_syntax-rules%29) {#syntax-rules}

`syntax-rules` gives us a straightforward way to match up different forms of a syntax object with the various transformations we'd like to execute. We use `syntax-rules` in place of the transformer function within `define-syntax`.

`syntax-rules` takes any number of pairs, where the first member is a *pattern*, and the second is a *template*. If those sound pretty similar, keep in mind that a template is something that still needs to be filled in with content. The pattern should look like an s-expression you want to match, but with labels in place of each member of the expression. The template should look like the code that you want to result from the transformation.

```racket
(define-syntax diff-num-args
  (syntax-rules ()
    [(diff-num-args arg1) (print (string-append 
                                   "There was one argument: "
                                   arg1))]
    [(diff-num-args arg1 arg2) (print (string-append
                                        "There were two arguments: "
                                        arg1
                                        " and "
                                        arg2))]))

$ (diff-num-args "A")
"There was one argument: A"
$ (diff-num-args "A" "B")
"There were two arguments: A and B"
```

You might notice the empty set of parentheses right after `syntax-rules` – that's for any terms that we want to treat literally in the patterns, so that they won't be assigned as labels for the different s-experession pieces.

##### a. (define-syntax-rule ...) [R](http://docs.racket-lang.org/reference/stx-patterns.html#%28form._%28%28lib._racket%2Fprivate%2Fmisc..rkt%29._define-syntax-rule%29%29) [G](http://docs.racket-lang.org/guide/pattern-macros.html#%28part._define-syntax-rule%29) {#define-syntax-rule}

If we only have on pattern to match, we can use `define-syntax-rule`. It acts just the same as `syntax-rules`,[^3] except that we can't reserve literal terms. Note that `define-syntax-rule` doesn't take place in the context of `define-syntax` – instead, it replaces both `define-syntax` and `syntax-rules`. Of course, you can only use it for a single transformation rule, so it's really the quick-and-dirty option.

[^3]: In fact, it's a Racket macro that expands to `syntax-rules`!

#### 2. (syntax-case ...) [R](http://docs.racket-lang.org/reference/stx-patterns.html#%28form._%28%28lib._racket%2Fprivate%2Fstxcase-scheme..rkt%29._syntax-case%29%29) [G](http://docs.racket-lang.org/guide/syntax-case.html) {#syntax-case}

Pattern matching is nice, but there's a limitation to `syntax-rules`: we can't do anything *except* replace syntax with other syntax. It would be nice to react to some patterns in more comprehensive ways, such as by handling errors.

`syntax-case` gives us that flexibility: instead of pairs of patterns and templates, `syntax-case` involves pairs of patterns and *expressions*. Expressions are normal Racket code, but we can dive into template mode as needed – to check different bits of the input syntax, for example, or ultimately to return the final result of the macro, which will be a syntax object.

The [Racket Guide section](http://docs.racket-lang.org/guide/syntax-case.html) on `syntax-case` has a clear example of this.

## III. Other Resources {#other-resources}

- [Fear of Macros](http://www.greghendershott.com/fear-of-macros/index.html)
- [Macros and Languages in Racket](http://rmculpepper.github.io/malr/index.html)
- [A Scheme syntax-rules Primer](http://www.willdonnelly.net/blog/scheme-syntax-rules/)
- [DSL Embedding in Racket](https://www.youtube.com/watch?v=WQGh_NemRy4) [YouTube]

