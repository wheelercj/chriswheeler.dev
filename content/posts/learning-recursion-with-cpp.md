+++
title = 'Learning recursion with C++'
date = 2022-05-02T23:23:49-08:00
lastmod = 2024-01-14T12:41:00-08:00
+++

Recursion is a type of loop created by making a function call itself. Here's a recursive function that loops infinitely (until the computer runs out of memory) when called:

```cpp
void print_stars() {
    cout << "*";
    print_stars();
}
```

Just like for loops and while loops, recursion can be used to do something repeatedly. Some problems are easier to solve with one kind of loop than another.

* **for loops** are usually best when something needs to be done a known number of times.
* **while loops** are usually best when something needs to be done repeatedly while certain condition(s) are true.
* **recursion** is usually best when you can break the problem down into smaller, but otherwise identical, problems until you get to a very simple problem.

For example, below is a recursive way to print a range of numbers. Since we start off knowing how many numbers to print, this is better as a for loop but still possible with recursion.

```cpp
#include <iostream>
using namespace std;
void print_numbers(int i, int max);

int main() {
    print_numbers(0, 10);
    return 0;
}

void print_numbers(int i, int max) {
    if (i > max)
        return;
    cout << i << " ";
    print_numbers(i + 1, max);
}
```

Output: `0 1 2 3 4 5 6 7 8 9 10 `

In the code above, `print_numbers` loops because it calls itself. The `if` statement makes sure the function stops calling itself at the right time. The function prints all numbers between the two numbers passed to it as arguments. Here is the non-recursive (the iterative) equivalent with the same output:

```cpp
#include <iostream>
using namespace std;

int main() {
    int max = 10;
    for (int i = 0; i <= max; i++)
        cout << i << " ";
    return 0;
}
```

As you can see, in this case using a for loop is more simple and makes the code easier to read. Simple examples of recursion are often slightly more simple as iterative loops, but can still be helpful for learning. Notice how many parts of the iterative version of the code are also present in the recursive version. The for loop's `i <= max` choosing whether the for loop should continue is equivalent to the recursive function's `i > max` choosing whether the recursive loop should end. Similarly, the for loop's `i++` is equivalent to the recursive function's `i + 1`.

If we want to print out even numbers within a range instead of all the numbers in that range, only a small change is required:

```cpp
#include <iostream>
using namespace std;
void print_even_numbers(int i, int max);

int main() {
    print_even_numbers(0, 10);
    return 0;
}

void print_even_numbers(int i, int max) {
    if (i > max)
        return;
    if (i % 2 == 0)  // Here's the only change (except the function name).
        cout << i << " ";
    print_even_numbers(i + 1, max);
}
```

Output: `0 2 4 6 8 10 `

## how to write recursive functions

One reason why learning to write recursive functions is often confusing at first is because with for loops and while loops, it's easy to think of the big picture of what you want to do first and gradually add detail. With recursion, it's usually easier to start with details ("base cases") and gradually build up from there.

For example, let's say we want to write a recursive function that reverses a string. Start by thinking, "What is an example of a simple input that is easy to deal with?"

What if the function is called with a string of size 1? Then we can just return that string.

```cpp
string reverse(string str) {
    if (str.size() == 1)
        return str;
    // TODO: handle cases where the string's size is not 1.
}
```

Also, what if the given string is empty? We can change the `==` to `<=` so that an empty string can be returned.

```cpp
string reverse(string str) {
    if (str.size() <= 1)
        return str;
    // TODO: handle cases where the string's size is > 1.
}
```

Now we have solved this for all the simple inputs that don't require any parts of the string to be swapped, and the function isn't recursive yet. Any string length greater than 1 will require some amount of swapping, and this is where we can start using recursion.

Let's think about how exactly we could swap four characters without code.

```
abcd
```

There are many ways to do this, but here's a way that is recursive:

_Swap the first letter with the rest of the string, but before finishing this swap, pause and do the same thing on the rest of the string._

This starts with moving the `a` towards the back . . .

```
 bcd
⤷a
```

. . . but pauses partway through to start swapping the `b`.

```
 cd
⤷b
⤷a
```

Eventually, every character will be in its own string, in the middle of being swapped.

```
 d
⤷c
⤷b
⤷a
```

Then we're at our base case of a string of length 1, and we can start recombining the characters.

```
dc
⤷b
⤷a
```

```
dcb
⤷a
```

```
dcba
```

This idea of swapping the first character with the rest of the string recursively is easy to put into code. With C++, we can get part of a string using the [substr](https://cplusplus.com/reference/string/string/substr/) function.

```cpp
string word = "hello";
cout << word.substr(1);  // ello
```

Here's what we left off with:

```cpp
string reverse(string str) {
    if (str.size() <= 1)
        return str;
    // TODO: handle cases where the string's size is > 1.
}
```

We're going to need the first character (`str[0]`) and the rest of the string (`str.substr(1)`). Since we want to swap them, they will be swapped in the code:

```cpp
string reverse(string str) {
    if (str.size() <= 1)
        return str;
    return str.substr(1)  ?  str[0]  ?  // TODO
}
```

We're going to combine these parts at some point, so we can add a plus sign to combine the strings.

```cpp
string reverse(string str) {
    if (str.size() <= 1)
        return str;
    return str.substr(1) + str[0];
}
```

Now if the function is given the input `"ab"`, it returns `"ba"`! But if given `"abc"`, it returns `"bca"` but should return `"cba"`. We need to also swap the substring's first character with the rest of the substring. That's the same as what we're already doing, just at a different scale. Time to use recursion.

```cpp
string reverse(string str) {
    if (str.size() <= 1)
        return str;
    return reverse(str.substr(1)) + str[0];
}
```

That's it! This function can reverse any string.

Writing recursive functions is usually easier when starting with figuring out what the base case(s) are and handling those first, and then figuring out how the problem can be broken down into smaller problem(s) that are otherwise the same.

Side note: the function above doesn't work "correctly" with most emoji and some other Unicode symbols because each emoji or symbol can be multiple characters long, but that's beside the point. The string's _characters_ are reversed correctly.

You might be wondering how an individual function can be recursive when its parameter(s) are reused and don't hold all the info. In the example above, the variable `str` will start with a value like "hello", but next have the value "ello", then "llo", etc. Where are the other letters going? The answer is that they still exist in the computer's memory in _a different instance of the function_. Each time any function is called, its information (name, variables, etc.) is saved until the function call ends. A recursive function's info will also be saved each time it is called, and each of these saves are kept separate in memory. When a recursive function that has called itself many times finally starts returning, the saves are then returned to in the reverse order of their creation, bringing back the info from earlier calls. These function instances are saved into part of the computer's memory called the stack (same as the stack data structure, but with a specific purpose). Since computer memory is limited, a recursive function that calls itself way too many times will crash its program with a stack overflow error (usually caused by an infinite loop.)

**Practice question 1:** try writing a recursive function that prints a range of numbers like in one of the first few examples above, but descending instead of ascending, such as `5 4 3 2 1 `. One possible solution is at the end of this page.

**Practice question 2:** write a function that uses recursion to find the sum of a descending range of numbers. For example, the sum of numbers 5 through 1 is 5 + 4 + 3 + 2 + 1 = 15. Then have your program print the result. One possible solution is at the end of this page.

**Practice question 3:** do the same as question 2 but recursively find and print the product (multiplication) of a descending range instead of the sum. For example, the product of the range 5 through 1 is 5 * 4 * 3 * 2 * 1 = 120. One possible solution is at the end of this page.

**Practice question 4:** create a recursive function that can calculate a [factorial](https://en.wikipedia.org/wiki/Factorial). For example, the factorial of 5 (also written as 5!) can be found like this: 5! = 5 * 4 * 3 * 2 * 1 = 120. Factorials always start at some non-negative number and end at 1 (except 0! = 1). Then print the result. One possible solution is at the end of this page.

## recursion with data structures

Recursion can be helpful in many situations. Whether it's better to use recursion or iteration for a problem depends on what you're trying to solve; if you're not sure, just guess and maybe lean towards iteration. A sign that recursion might be better is if you not only need to perform an action repeatedly, but each repetition must also be at **a different scale**. For example, programs that help you search through many files at a time usually (always?) use recursion because if the search doesn't find a match in the files in the starting folder, the search can be repeated in any folders within that folder. In other words, an action can be performed repeatedly and at different scales (folders within folders).

Some cases where recursion is often better than iteration include file operations, sorting (e.g. [quicksort](https://github.com/wheelercj/Algorithms/blob/b7d1a757283a1d5ce6a7c2a1942f61e7f6a04b48/Algorithms/Searching%20and%20Sorting.cpp#L201) or [merge sort](https://github.com/wheelercj/Algorithms/blob/b7d1a757283a1d5ce6a7c2a1942f61e7f6a04b48/Algorithms/Searching%20and%20Sorting.cpp#L235)), and working with some data structures like linked lists, trees, and graphs. Note that a computer's files and folders are a tree data structure. Recursion is more commonly used with the functional [programming paradigm](https://en.wikipedia.org/wiki/Programming_paradigm) than the perhaps more widely used procedural and object-oriented paradigms. In fact, some languages that support functional programming (such as Clojure) don't even have iterative loops.

Below is a more involved example of recursion, this time using a [linked list](https://en.wikipedia.org/wiki/Linked_list) with pointers. The code defines a Node structure that holds an integer and a pointer to the next node in the list. It then creates a linked list of four nodes, prints their contents, and then deletes the list.

```cpp
#include <iostream>
using namespace std;

struct Node {
    int value = 0;
    Node* next = NULL;
    Node(int new_value) {
        value = new_value;
    }
};

void append(int new_value, Node*& ptr);
void print_list(Node* ptr);
void delete_list(Node* ptr);

int main() {
    Node* list_head = NULL;
    append(11, list_head);
    append(12, list_head);
    append(13, list_head);
    append(14, list_head);

    print_list(list_head);

    delete_list(list_head);
    return 0;
}

void append(int new_value, Node*& ptr) {
    if (ptr == NULL)
        ptr = new Node(new_value);
    else
        append(new_value, ptr->next);
}

void print_list(Node* ptr) {
    if (ptr == NULL)
        return;
    cout << ptr->value << " ";
    print_list(ptr->next);
}

void delete_list(Node* ptr) {
    if (ptr == NULL)
        return;
    delete_list(ptr->next);
    delete ptr;
    ptr = NULL;
}
```

Output: `11 12 13 14 `

Notice how each of the three functions that do something with the linked list are recursive. Take a moment and imagine trying to write these three functions (`append`, `print_list`, and `delete_list`) without using recursion.

Below is an example of how to do that.

```cpp
#include <iostream>
using namespace std;

struct Node {
	int value = 0;
	Node* next = NULL;
	Node(int new_value) {
		value = new_value;
	}
};

void append(int new_value, Node*& list_head);
void print_list(Node* ptr);
void delete_list(Node*& ptr);

int main() {
	Node* list_head = NULL;
	append(11, list_head);
	append(12, list_head);
	append(13, list_head);
	append(14, list_head);

	print_list(list_head);

	delete_list(list_head);
	return 0;
}

void append(int new_value, Node*& list_head) {
	if (list_head == NULL) {
		list_head = new Node(new_value);
		return;
	}
	Node* ptr = list_head;
	while (ptr->next != NULL)
		ptr = ptr->next;
	ptr->next = new Node(new_value);
}

void print_list(Node* ptr) {
	for (; ptr != NULL; ptr = ptr->next)
		cout << ptr->value << " ";
}

void delete_list(Node*& ptr) {
	while (ptr != NULL) {
		Node* temp = ptr->next;
		delete ptr;
		ptr = temp;
	}
}
```

Output: `11 12 13 14 `

As you can see by comparing the samples above, using iterative loops is only slightly more difficult than using recursion for linked lists. However, for other data structures like trees and graphs, using iterative loops is usually significantly more difficult than using recursion. Sometimes iterative loops are easier to use, and sometimes recursive loops are easier to use. In any case, recursion is another tool in your belt that will be very helpful in the future.

**practice question 5:** add to the recursive version of the linked list code above a recursive function that finds the sum of all the integers in the list. One possible solution is at the end of this page.

**practice question 6:** add to the recursive version of the linked list code above a recursive function that finds the length of the list. One possible solution is at the end of this page.

**practice question 7:** add to the recursive version of the linked list code above a recursive function that finds the location of a value in the list. For example, in the list containing the numbers 11, 12, 13, and 14, the location of 11 is 0, the location of 12 is 1, etc. One possible solution is at the end of this page.

**practice question 8:** add to the recursive version of the linked list code above a recursive function that inserts a new value into the list at a given index. If index 0 is chosen, the new node should become the first node in the list. One possible solution is at the end of this page.

## intermediate topics

It's never impossible to implement something with iteration that is significantly easier to implement with recursion. After all, programming languages that allow recursion must themselves implement recursion somehow. They do that using iteration. (The exception is that, as mentioned above, some languages don't have iterative loops.)

As an example of how much easier recursion can be, below is a function written in Java that creates a random binary tree (you can [see the definition of `Node` here](https://mystb.in/BoringRomanCharts)).

```java
public Node randomTree() {
    if (Math.random() < 0.4)
        return null;
    return new Node(Math.random() * 100, randomTree(), randomTree());
}
```

[The iterative equivalent](https://mystb.in/CompanyAnalogChile) of this function is about 40 lines long. However, a large part of why it's so much longer iteratively in this particular case is because there happened to be a requirement that the `Node` variables in the `Node` class be `final`.

Some programming languages have more sophisticated recursion implementations than others, especially languages that depend on recursion more. For example, some programming languages have [tail-call optimization](https://en.wikipedia.org/wiki/Tail_call), with which recursive functions that have their recursive call as the last action of the function (tail-call) will, when compiled, be automatically changed to use iteration instead of recursion. This optimization can reduce memory use and prevent stack overflow errors. The link above includes a list of languages and whether they have tail-call optimization.

**practice question 9:** create a `Node` class (or struct) like in the linked list examples above, but this time for a binary tree. Using `Node`, make a sample binary tree for testing, and create a recursive function that performs a [depth-first search](https://en.wikipedia.org/wiki/Depth-first_search) on the given tree. Sample pseudocode is on the linked Wikipedia page.

**practice question 10:** take a few minutes to think about how you could rewrite the depth-first search function from practice question 9 using iteration instead of recursion. How could it work for trees with different depths and different numbers of nodes? Some thoughts on this are at the end of this page.

**practice question 11:** create a recursive function that solves [Tower of Hanoi](https://en.wikipedia.org/wiki/Tower_of_Hanoi). The function should take inputs of: the number of disks (an integer), the name of the rod to move disk(s) from (a string), the name of the rod to move disk(s) to (a string), and the name of the other rod (a string). The function should not return anything but should print the human-readable steps to take that solve the puzzle. Each step moves one disk. For example, if you name the rods "A", "B", and "C", one step might look like "move a disk from A to C". One possible solution is on the linked Wikipedia page.

**practice question 12:** create a recursive function that returns the Nth value in the [Fibonacci sequence](https://en.wikipedia.org/wiki/Fibonacci_sequence). The function's only input should be an integer that is a sequence index. For example, an input of 0 returns 0, 1 returns 1, 2 returns 1, 3 returns 2, etc.

### dynamic programming

[Dynamic programming](https://en.wikipedia.org/wiki/Dynamic_programming) is a challenging and important category of algorithms for solving a wide range of problems. It always uses recursion in some sense, although it can use recursively defined variables rather than recursive functions. Dynamic programming algorithms must also save and reuse intermediate results to be considered dynamic programming algorithms.

The two most common ways of storing intermediate results are **tabularization** and [**memoization**](https://en.wikipedia.org/wiki/Memoization) (not memorization). Tabularization uses a table (an array) that is recursively defined. Memoization is the saving of a function's return values for reuse any time the function is called again with the same inputs; this avoids having to calculate those return values again. (Code that uses memoization is not necessarily dynamic programming because it does not necessarily use recursion.) Both tabularization and memoization are good to understand, but this guide will only cover memoization.

Let's add memoization to a recursive function and make it a dynamic programming algorithm. The [Fibonacci sequence](https://en.wikipedia.org/wiki/Fibonacci_sequence) function from sample answer 12 (copied below) is recursive, but currently requires recalculating the previous two values in the sequence to find each next value. Since it is not saving and reusing results as it goes, it is currently NOT dynamic programming.

```cpp
#include <iostream>
using namespace std;

// fib returns the nth value of the Fibonacci sequence.
int fib(int n) {
    if (n <= 1) {
        return n;
    }
    return fib(n - 1) + fib(n - 2);
}

int main() {
    cout << fib(8);
    return 0;
}
```

Since the `fib` function's original input is `8`, the program's output is `21` (the 8th value in the Fibonacci sequence), which adding memoization should not change.

Some programming languages have an easy way to add memoization to a function; unfortunately, C++ memoization takes a few steps. We can use a [`map`](https://en.cppreference.com/w/cpp/container/map) data structure to store a function's inputs and corresponding outputs.

```cpp
#include <iostream>
#include <map>  // A map can be used to store a function's inputs and outputs.
using namespace std;

map<int, int> m{{0, 0}, {1, 1}};
// The `m` variable will hold the `fib` function's inputs and outputs. The map's keys
// are the sequence indexes, and the map's values are the sequence values. We start with
// two values: the 0th value in the Fibonacci sequence is 0, and the 1st value is 1.

// fib returns the nth value of the Fibonacci sequence.
int fib(int n) {
    // First, we check if we've seen this input before.
    if (m.count(n) == 0) {
        // If we haven't seen this input yet, we calculate & save the output.
        m[n] = fib(n - 1) + fib(n - 2);
    }
    return m[n];
}

// The main function is unchanged.
int main() {
    cout << fib(8);
    return 0;
}
```

Now this is dynamic programming, and it can run significantly faster because each intermediate result is only calculated once, unlike before.

## further reading

* [Dear Functional Bros](https://www.youtube.com/watch?v=nuML9SmdbJ4), a youtube video by CodeAesthetic, covers recursion and why it's important
* [Recursion — Wikipedia](https://en.wikipedia.org/wiki/Recursion_(computer_science))
* [Dynamic Programming is not Black Magic](https://qsantos.fr/2024/01/04/dynamic-programming-is-not-black-magic/) by Quentin Santos
* Leetcode has many questions for practice with [recursion](https://leetcode.com/problemset/algorithms/?page=1&topicSlugs=recursion) and [dynamic programming](https://leetcode.com/problemset/algorithms/?topicSlugs=dynamic-programming&page=1)
* and of course, [Learning recursion with C++](/learning-recursion-with-cpp)

## practice question sample solutions

Here are some of the possible solutions to the practice questions above. If you want to run the code, a super quick way is with [tio.run](https://tio.run/#cpp-gcc).

**sample answer 1:** A recursive function that prints a descending range of numbers:

```cpp
#include <iostream>
using namespace std;
void print_descending_numbers(int i, int min);

int main() {
    print_descending_numbers(5, 1);
    return 0;
}

void print_descending_numbers(int i, int min) {
    if (i < min)
        return;
    cout << i << " ";
    print_descending_numbers(i - 1, min);
}
```

Output: `5 4 3 2 1 `

**sample answer 2:** A program that uses recursion to find the sum of a descending range, and then prints the sum:

```cpp
#include <iostream>
using namespace std;
int find_sum(int i, int min);

int main() {
    int sum = find_sum(5, 1);
    cout << sum;
    return 0;
}

int find_sum(int i, int min) {
    if (i < min)
        return 0;
    return i + find_sum(i - 1, min);
}
```

Output: `15`

**sample answer 3:** A program that uses recursion to find the product of a descending range, and then prints the result:

```cpp
#include <iostream>
using namespace std;
int find_product(int i, int min);

int main() {
    int product = find_product(5, 1);
    cout << product;
    return 0;
}

int find_product(int i, int min) {
    if (i < min)
        return 1;
    return i * find_product(i - 1, min);
}
```

Output: `120`

**sample answer 4:** A program that uses recursion to calculate a factorial, and then prints the result:

```cpp
#include <iostream>
using namespace std;
int factorial(int n);

int main() {
    int result = factorial(5);
    cout << result;
    return 0;
}

int factorial(int n) {
    if (n == 0)
        return 1;
    return n * factorial(n - 1);
}
```

Output: `120`

**sample answer 5:** A function that recursively finds the sum of all integers in the linked list:

```cpp
int find_sum(Node* node_ptr) {
	if (node_ptr == NULL)
		return 0;
	return node_ptr->value + find_sum(node_ptr->next);
}
```

**sample answer 6:** A function that recursively finds the length of the linked list:

```cpp
int find_length(Node* node_ptr) {
	if (node_ptr == NULL)
		return 0;
	return 1 + find_length(node_ptr->next);
}
```

**sample answer 7:** A function that recursively finds the location of a value in the linked list:

```cpp
int find_value(int target, Node* node_ptr, int i) {
	if (node_ptr == NULL)
		return -1;
	if (target == node_ptr->value)
		return i;
	return find_value(target, node_ptr->next, i + 1);
}
```

**sample answer 8:** A function that recursively inserts a new value into the linked list at a given index:

```cpp
void insert(int new_value, int index, Node** next_ptr) {
	if (index == 0)
    {
		Node* new_node = new Node(new_value);
		new_node->next = *next_ptr;
		*next_ptr = new_node;
	}
	else if (*next_ptr == NULL)
		cout << "Error! Invalid index." << endl;
	else
		insert(new_value, index - 1, &(*next_ptr)->next);
}
```

**sample answers 5-8:** You can see all of the above linked list code combined [here](https://gist.github.com/wheelercj/299d03080d69b59547e7678faa7a1009). Its output is:

```
11 12 13 14
sum = 50
length = 4
index of 12 = 1
11 12 13 8 14
```

**sample answer/pseudocode 9:** [Depth-first search — Wikipedia](https://en.wikipedia.org/wiki/Depth-first_search#Pseudocode)

**sample answer/thoughts 10:** to write an iterative depth-first search, your first thought might be to use nested while loops and to have as many loops as there are "branches" in the tree. But then a tree with more or fewer branches would need a different number of loops and the function wouldn't work. Instead, each time we encounter a branch, we can temporarily save data about where to come back to later if needed. The perfect way to do this is with a [stack data structure](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)). Since the recursive solution also uses a stack (the call stack) the iterative and recursive approaches end up being quite similar except that the iterative solution takes more work.

**sample answer 11:** [Tower of Hanoi — Wikipedia](https://en.wikipedia.org/wiki/Tower_of_Hanoi#Recursive_implementation)

**sample answer 12:** A function that recursively finds the Nth value in the Fibonacci sequence:

```cpp
int fib(int n) {
    if (n <= 1) {
        return n;
    }
    return fib(n - 1) + fib(n - 2);
}
```
