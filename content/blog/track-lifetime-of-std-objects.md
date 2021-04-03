---
title: Track the Lifetime of std Classes' Instances
created_at: 2015-11-03
kind: article
---

I remembered a simple trick I used to know, but that could not remember on the spot, to solve a problem in understanding the exact lifetime of C++ objects.


## Problem statement

Reading about `unique_ptr`'s, I have found some issue trying to understand the exact lifetime of objects of such types. When is the ownership of a `unique_ptr` exactly yielded? What happens when the first one passes its ownership?

## A bad idea

I have gone through a silly phase where I have thought that I could log messages at crucial moments of the objects' lifetime by just writing to standard output in constructors, destructors, copy/move constructors, and copy/move assignment operators. The first idea is to wrap such functions in the object being passed as argument to the `unique_ptr`:

~~~cpp
struct S {
    S() { cout << "S()\n"; }

    /*
     * These will never be called, since what we are
     * copying/moving is the unique_ptr. How do we
     * extend a unique_ptr to print its
     * creation/destruction times?
     */
    S(const S& s) {
      cout << "S(const S& s)" << hex << &s << "\n";
    }

    S(S&& s) {
      cout << "S(S&& s)" << hex << &s << "\n";
    }

    ~S() { cout << "~S()\n"; }
};
~~~

I even have the naive question expressed in a comment. Of course this wouldn't work. While this approach _does_ work when you want to follow the structure `S`'s lifetime, it certainly doesn't when you want to follow the lifetime of the wrapping smart pointer.

I banged my head against Google and ran into some unrelated threads on StackOverflow. No one ever had this problem before? There must be something terribly wrong with what I am looking for...

## How can we do better?

At the end of my study session, I thought about the obvious:

> Just wrap the `unique_ptr` in a structure, and print a message in this structure's ctors, dtors, and copy/move ctors and assignment operators

So I separated my code out into a header, and used it instead:

~~~cpp
#include <memory>
#include <iostream>
#include <iomanip>

// Instrument a unique_ptr with some logging, to follow its
// lifetime.
template <typename T>
struct unique {
    unique() : _p(nullptr) {
        std::cout << "unique() " << std::hex << &_p << "\n";
    }

    unique(T* p) : _p(p) {
        std::cout << "unique(T* p) " << std::hex <<
          &_p << "\n";
    }

    unique(const unique<T>& p) : _p(p._p) {
        std::cout << "unique(const unique& u) " <<
          std::hex << &_p << "\n";
    }

    unique(unique<T>&& u) : _p(move(u._p)) {
        std::cout << "unique(unique<T>&& u) " <<
          std::hex << &_p << "\n";
    }

    // Can't declare this, as unique_ptr's operator= with
    // copy semantics has been explicitly deleted!
    unique<T>& operator=(const unique<T>& p) = delete;

    unique<T>& operator=(unique<T>&& p) {
        std::cout << "operator=(unique<T>&& p) " <<
          std::hex << &_p << "\n";
        _p = move(p._p);
        return *this;
    }

    ~unique() {
      std::cout << "~unique() " << std::hex << &_p << "\n";
    }

    std::unique_ptr<T> _p;
};
~~~

The basic structure of all of these functions is the following:

1.  Tell me your name
1.  Tell me the address of the `unique_ptr`

They really don't do much, but they are terribly useful when things get complicated. Which, given the language, is quite often.

### Example

A very simple one:

~~~cpp
#include <memory>
#include "../util/instr_ptr.hpp"

using namespace std;

unique<int> returnPtr() {
    return unique<int>(new int);
}

int main() {
  unique<int> u = returnPtr();
  cout << hex << &u << endl;
}
~~~

The output would be something as simple as:

<pre>
  unique(T* p) 0x7fff71382e78
  0x7fff71382e78
  ~unique() 0x7fff71382e78
</pre>

As I said, nothing complicated, but it helps.
