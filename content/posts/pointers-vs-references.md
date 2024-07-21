+++
title = 'Pointers vs. references'
date = 2024-07-03T14:17:41-07:00
lastmod = 2024-07-21T00:37:18-07:00
tags = []
+++

Some differences between pointers and references can be confusing, especially since there are several different definitions for those words. This can make it difficult to answer simple questions like "Does Golang have reference types?" (TLDR: most people and [the official Go FAQ](https://go.dev/doc/faq#references) say that Go does have reference types.)

## Pointers

In some languages like C++, pointers can point anywhere in memory. They can point to null, or to memory with data of the pointer's type, or any other part of memory. In other languages including Go, the same is true except that they cannot point just anywhere; they can point to null/nil or to memory with data of the pointer's type, but not anywhere else.

## References

C++'s references are similar to Go's pointers in that they can refer to memory with data of the reference's type, but not other parts of memory. However, C++'s references cannot refer to null unlike most languages with references. As far as I know, no programming language allows references to refer to parts of memory that do not have data of the reference's type (besides null/nil).

Some definitions of "references" disagree on whether references need to be able to be "returned by reference" (see description below) even when reassigned. References in most languages do allow returning reassignment by reference, but Go's references do not. One of the Go language's project members, Dave Cheney, uses that fact to claim [There is no pass-by-reference in Go](https://dave.cheney.net/2017/04/29/there-is-no-pass-by-reference-in-go), but the official Go FAQ says [maps, slices, and channels are references](https://go.dev/doc/faq#references). (I ran Cheney's sample code with Go 1.22.5 and it gave the same output as in his post.) Whether Go has references depends on the debatable definition of "references", but in my experience, most people say Go has references.

Related: [Value, reference, and move types and semantics](/posts/value-reference-and-move-types-and-semantics)

## Returned by reference

Here's what "returned by reference" means:

1. a function calls another, giving it a reference variable
2. the called function changes the variable but does not return it
3. the called function ends
4. the calling function resumes, but the variable still has the changes made by the called function as if it was returned

Both functions accessed the same copy of the reference variable, the same part of the computer's memory.

All of this is true in Go when only *mutating* a map, slice, or channel, but not when *reassigning* it. Returning nothing after reassigning a map, slice, or channel does nothing to the variable. My guess is that this behavior makes Go's garbage collection more efficient since the scope of dynamically allocated memory is more predictable. Fortunately, the static analysis tools most people install by default seem to catch all coding mistakes related to this.
