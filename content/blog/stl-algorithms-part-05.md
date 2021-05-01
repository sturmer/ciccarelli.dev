---
title: STL Algorithms, part 5
created_at: 2014-02-04
kind: article
tags:
 - STL
 - C++
---

- [Part 1](/blog/stl-algorithms-part-01/)
- [Part 2](/blog/stl-algorithms-part-02/)
- [Part 3](/blog/stl-algorithms-part-03/)
- [Part 4](/blog/stl-algorithms-part-04/)
- [Part 5](/blog/stl-algorithms-part-05/)

In the [previous post][p4] in the series, we have analyzed more algorithms from the STL considered to be part of the non-modifying sequence operations. In this post, we are going to show some use case for algorithms which fall into the modifying sequence operations.

As brilliantly pointed out in [Josuttis][josuttis], however, note that an unordered container and an associative container can never be used as the destination of a modifying algorithm. The rationale is that elements in that kind of containers are constant after they've been inserted, and changing their content might compromise their order established by the sorting and hashing functions.

## `copy` and `move`

We have seen an example in the [last post][iter], where we have shown how to combine the `copy` algorithm with a `back_inserter` to provide, among other goodies, an elegant way of printing containers. The `copy` algorithm performs a sequence of assignments from elements of a container to elements of another one.  Its declaration, as defined in the standard (\[alg.copy\]), is:

~~~cpp
template<class InputIterator, class OutputIterator>
OutputIterator copy(InputIterator first,
                    InputIterator last,
                    OutputIterator result);
~~~

It's really easy to understand with an example:

~~~cpp
vector<int> v({1,2,3,4,5});
vector<int> primes({1, 2, 3, 5, 7, 11, 13});

copy(primes.begin() + 4, primes.end(), v.begin() + 2);

// Using Louis Delacroix's cxx-prettyprint
cout << "v: " << v << '\n';
~~~

This copies the elements from the fifth to the last of the vector `primes` into the vector v, starting from the third position. Be very careful here: we are not inserting those elements between the elements 2 and the 3 of the vector `v`, we are overwriting positions 2 to 4 (starting from 0) with elements from `primes`.  The output is:

<pre>
V: [1, 2, 7, 11, 13]
</pre>

To print the vector, we have used Louis Delacroix' [cxx-prettyprint][cxxpp], a great tool to easily print STL containers. As you can see, the last three elements have been overwritten. If you had tried to do the same with a destination vector of size 4, this is the surprising end you'd have run into:

<pre>
V: [1, 2, 7, 11]
</pre>

This is what happens on my architecture (x86_64, with clang 3.2 and gcc 4.8.1 both showing the same result). The standard does not mention whether this is undefined behavior or not, but I find it risky anyway, so I'd rather check the relative sizes before attempting any copy. Again in Josuttis, however, we are said that *for `copy()`, `result` should not be part of the range \[`first`, `last`)*.

### Using `move`

You might have noticed that what we are doing is actually moving elements from one container to another. While the `copy` algorithm uses the copy assignment operator to perform the assignments, we might want to use the `move` algorithm in order to take advantage of a move assignment operator that the objects in the container might provide. Consider the following trivial code for a `struct` providing move contructors (I think I got the idea from [Professional C++][procpp]

~~~cpp
struct Moveable {
  int i;
  Moveable(const int i) : i(i) { cout << "ctor\n"; }

  Moveable(const Moveable& m) : i(m.i) {
    cout << "copy ctor\n";
  }

  Moveable& operator=(const Moveable& m) {
    i = m.i;
    cout << "copy assignment\n";
    return *this;
  }

  Moveable(Moveable&& m) : i(m.i) {
    m.i = 0;
    cout << "move ctor\n";
  }

  Moveable& operator=(Moveable&& m) {
    i = m.i;
    m.i = 0;
    cout << "move assignment\n";
    return *this;
  }
};
~~~

If we use this class in a `copy`, we obtain that, during the execution of the algorithm, the copy assignment operator is chosen to perform the assignment.  Using the `move` algorithm, instead, we use some client code like:

~~~cpp
vector<Moveable> v1({
  Moveable(1),
  Moveable(3),
  Moveable(5),
  Moveable(7)
});

vector<Moveable> v2({
  Moveable(2),
  Moveable(4),
  Moveable(8),
  Moveable(16)
});

move(v1.begin(), v1.end(), v2.begin());
cout << "v1: " << v1 << '\n';
cout << "v2: " << v2 << '\n';
~~~

Our result is the following:

~~~cpp
ctor
ctor
ctor
ctor
copy ctor
copy ctor
copy ctor
copy ctor
ctor
ctor
ctor
ctor
copy ctor
copy ctor
copy ctor
copy ctor
move assignment
move assignment
move assignment
move assignment
v1: [0, 0, 0, 0]
v2: [1, 3, 5, 7]
~~~

We have: 4 objects constructed in the initializer list, then copied in the vector; this happens for the 2 vectors; then the `move` algorithm is called, and 4 move assignments are performed. As you see, we have pilfered the values from the vector `v1`. Not that this was necessary; I think it only makes clear one criterion that should be noted: we use `copy` to copy elements between containers, but we use `move` when the elements in the source container are *disposable*, in the sense that we don't access them after the move.

## Variations
The `copy` and `move` operations come with variants. The variants of copy are:

* `copy_if`
* `copy_backward`
* `copy_n`

To copy when a condition is verified (determined by a function object), the algorithm with the `_if` suffix should be used.

If we want, instead, to copy the input range last to first element, we can use the backward version (that has a suffix `_backward`). One subtlety to which we have to pay attention is the matter of overlapping ranges.  The forward version can be used when the first element in the output sequence comes after the first element in the input sequence. The backward version is to be used whenever this does not hold true.

Finally, the suffix `_n` version can be used to directly specify the number of elements to be copied, rather than giving the end of the range. For example, we can say:

~~~cpp
copy_n(src.begin(), 10, dest.begin())
~~~


instead of specifying the more cumbersome `src.begin() + 10`.

The `move` algorithm, instead, only offers the variation to move backward, called, indeed, `move_backward`.

## Summary
We have started to have a brief look at the the sequence modifying algorithms.  The following is a resume table with the name of the algorithm and a one-line example usage:

|Algorithm|Example|
|---- |---- |
|`copy`|`copy(src.begin(), src.end(), dest.begin())`|
|`copy_backward`|`copy_backward(src.begin(), src.begin() + 4, src.begin() + 2)`|
|`move`|`move(src.begin(), src.end(), dest.begin())`|
|`move_backward`|`move(src.begin(), src.begin() + 10, src.begin() + 3)`|
|`copy_if`|`copy_if(src.begin(), src.end(), dest.begin(), [](int i){ return i > 10; })`|
|`copy_n`|`copy_n(src.begin(), 10, dest.begin())`|

[p4]: /blog/2014/01/14/stl-algorithms-part-4
[iter]: /blog/2014/02/17/iterators-and-algorithms
[cxxpp]: http://louisdx.github.io/cxx-prettyprint/
[josuttis]: http://www.cppstdlib.com/
[procpp]: http://www.wrox.com/WileyCDA/WroxTitle/Professional-C-2nd-Edition.productCd-0470932449.html
