---
title: STL Algorithms, part 4
created_at: 2014-01-14
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

We have seen some examples of usage of the `find` algorithm, with its variants `find_if` and  `find_if_not`. In this installment, we are going to see other variants of finding algorithms, plus another algorithm useful when counting items in a sequence, `count`. Remember that we are still in the realm of non-modifying sequence algorithms (but see the discussion about `for_each` [here](/blog/2013/12/30/stl-algorithms-part-2/ "STL Algorithms, part 2")).

### `find_first`: Find first occurrence of any element taken from a set
This algorithm is useful when we are looking for one element of a set. Consider a `std::string`, which is also a container, and the problem of finding any vowel in such a string. We could go like the following:

~~~cpp
int main() {
  // is y a vowel?
  set<char> vowels{'a', 'e', 'i', 'o', 'u', 'y'};
  string haystack{"haystack"};
  auto it = find_first_of(haystack.begin(), haystack.end(),
    vowels.begin(), vowels.end());

  while (it != haystack.end()) {
    cout << *it << '\n';
    it = find_first_of(it + 1, haystack.end(),
      vowels.begin(), vowels.end());
  }
}
~~~

This small program will print the sequence of vowels in the word *haystack*. I am not sure that *y* is a vowel (actually I doubt), but I can invoke the exemplification\'s excuse. The output is:

<pre>
    a
    y
    a
</pre>

This algorithm can have a significant overhead because it has quadratic complexity (its execution time grows as the multiplication of the length of the two sequences). It prototype is:

~~~cpp
template<class InputIterator, class ForwardIterator>
InputIterator
find_first_of(InputIterator first1, InputIterator last1,
  ForwardIterator first2, ForwardIterator last2);
~~~

There is also a similar version where we can add a predicate, this way stating that we are looking for any of the elements in the second sequence for which the predicate is satisfied.

### `find_end`: Find subsequences
This algorithm has a deceptive name. It finds the subsequence in a sequence, returning the iterator to the first character of such sequence, or returning the last element of the first sequence if no such subsequence is found. The name comes from the fact that such subsequence is the rightmost, in case it occurred more than once. Consider the following example:

~~~cpp
string subseq{"lazy"};
string seq{"the quick brown fox jumped over the lazy dog, "
  "but the lazy dog woke up"};
auto it = find_end(seq.begin(), seq.end(),
        subseq.begin(), subseq.end());

std::string res(it, seq.end());
cout << "Result: " << res << '\n';
~~~

Its output is simply:

<pre>
Result: lazy dog woke up
</pre>


Also this algorithm has a variant where we can specify a predicate to be satisfied for each element matched.

### `adjacent_find`: Find consecutive elements
Suppose now that you want to find whether there are repeated elements in a string. The `adjacent_find` algorithm does just that, returning an iterator to the first element that is followed by itself at least once. Also this algorithms comes with a variation allowing to specify a predicate:

~~~cpp
string seq{"arbababbbbbbbbbccc"};
auto it = adjacent_find(
  seq.begin(), seq.end(),
  [](char c1, char c2) {
    return c1 != 'r' && c2 == 'b';
  }
);

std::string res(it, seq.end());
cout << "Sequence: " << seq << '\n';
cout << "Result:   " << res << '\n';
~~~

The outcome of this algorithm is the following: for each letter and its immediate successor, the predicate should be satisfied. This holds true for the first time when we arrive at the fourth letter, so the outcome is:

<pre>
  Sequence: arbababbbbbbbbbccc
  Result:      ababbbbbbbbbccc
</pre>

### `count` and `count_if`: Count elements
The last algorithm considered in this post is `count`, with its variation `count_if`. As expected, it counts the number of elements (possibly, matching a predicate):

~~~cpp
string seq{"arbababbbbbbbbbccc"};
auto it = count_if(seq.begin(), seq.end(),
  [](char c) { return c == 'b'; });
cout << "Result:  " << it << '\n';
~~~

and it counts 11 occurrences.

### Conclusions
In this installment, we have seen examples of some variations of `find`, plus a way to count occurrences of an element in a sequence. At this point, the algorithms in the *non-modifying sequence operations* category are mostly over; in the next episode, we are going to wrap them up.
