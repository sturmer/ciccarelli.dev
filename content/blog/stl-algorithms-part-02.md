---
title: STL Algorithms, part 2
created_at: 2013-12-30
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

### Previously on this blog
In the previous installment, we have introduced three algorithms, `all_of`, `any_of`, and `none_of`, very useful to determine whether a range of elements satisfy some predicate, expressed as a lambda.

In this second episode, we will explore more algorithms offered by the STL. My objective, in the long run, is to cover all of them with examples, and maybe grasp the ideas that might come to my mind during the process. As usual, if you have any doubt or just spot a plain error, I'd greatly appreciate if you let me know.

### for_each (Act I)

~~~cpp
template<class InputIterator, class Function>
Function for_each(InputIterator first, InputIterator last, Function f);
~~~

The algorithms that we have seen so far fall into the category of *non-modifying sequence operations*, because they can be applied without having to worry about changing the underlying range (or to invalidate the container).  The next few ones fall in the same category.

A very interesting one is the `for_each` algorithm. It can be used to apply some operation to a range of elements. Things become tricky in this case, because the algorithm requires the function, that we want to apply to the elements in the range, to be *MoveConstructible*.  I will cover rvalue references and move semantics in one of the next posts (or you can have a look at this excellent [post][moves-demystified], the best I have read so far about the subject), but for now it is enough to say that this means what follows.


#### Intermezzo: being MoveConstructible
Being *MoveConstructible*, for an object (in this case a function object or a lambda), means that it must obey a post-condition. In this case, if we have either of the following expressions:

~~~cpp
T t = rv
T(rv)
~~~

then the post-condition (in the first case) is that `t` has the same value that `rv` used to have, before `t`'s construction is terminated (that is to say, before this statement), but after `t` is constructed (and this statement has been executed), no guarantee is given about the state of `rv` (in particular, its state might be different from the state it had before the assignment). The same is true for the second type of expression.

This requirement is stronger than being required to be *CopyConstructible*, where the state of `rv` after the assignment has to stay unchanged.  For now, we can think of move semantics as a way of reducing the number of object creations. To do this, the language offers the chance to change the state of an object being moved from.

### `for_each` (Act II)
What does this mean with respect to our usage of `for_each`? This has to do with the return type of the algorithm: it is `std::move(f)`. Since `f` is required to be *MoveConstructible*, this guarantees that, if `f` is an object with a state, then the returned value has a valid state, even if optimizations are applied when returning it.

It can be exemplified like follows:

<pre>
for each element in the range
    dereference the element
    apply the function f
return a (possibly destructive) copy of f
</pre>

When returning, the result of the copy is perfectly valid, but the object it is copied from might get altered by the copy operation. The fact that this copy is actually a *move* is not important to understand the effects.  The net result is: we can neglect this, and leave the responsibility to the implementer of the STL.

An example will make everything clear:

~~~cpp
int main(int argc, const char *argv[]) {
    vector<int> v{1, 2, 3};
    for_each(v.begin(), v.end(),
            [](int& i) { i *= 2; });
    for (auto& vv: v)
        cout << vv << '\n';
    return 0;
}
~~~

This will print the numbers 2, 4, 6. Note that the standard specifies that, if `f` returns a result (which is not the case for our example), that result is ignored. As you can see, even though `for_each` is supposed to belong to the non-modifying sequence operations, it can alter the values of the elements in the container. This has been object of a discussion; one conclusion is that the `for_each` itself does not modify the container, while the function `f` could; another, is that modifying the elements is not equivalent to modifying the sequence's structure (i.e., the `for_each` does not invalidate the underlying pointers). If you are interested in the fine points of the discussion, you can have a look [here][fn-modify] and [here][SO-foreach].

### Conclusions
We have seen another fine algorithm offered by the standard template library.  Even though of simple application, this one has been subject to some debate about its classification as non-modifying sequence operation; it has also given me the chance to provide a first (very incomplete!) explanation of what is the move semantics in the C++11 standard.

In the next installment of this series, I am going to talk about algorithms for finding elements in a container (searching algorithms).

[fn-modify]: http://www.open-std.org/jtc1/sc22/wg21/docs/lwg-defects.html#475
[SO-foreach]: https://stackoverflow.com/questions/662845/why-is-stdfor-each-a-non-modifying-sequence-operation
[moves-demystified]: http://holdstare.github.io/technical/2013/11/23/moves-demystified.html
