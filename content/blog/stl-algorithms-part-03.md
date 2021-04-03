---
title: STL Algorithms, part 3
created_at: 2014-01-06
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

In the previous posts of this series, we have had a glance at *non-modifying sequence algorithms*, i.e., the algorithms that are supposed not to modify the elements of a sequence (or at least, the structure of the sequence; see the observations about `for_each`.

In this installment, as anticipated, we will start looking at the algorithms for searching.

### `find`
The STL `find` algorithms, specified in section [`alg.find`] of the C++11 standard, are useful when searching elements in the sequence that satisfy some requirements. The basic form has the prototype that follows:

~~~cpp
template<class InputIterator, class T>
InputIterator find(InputIterator first, InputIterator last,
    const T& value);
~~~

This algorithm looks for the element whose value is, well, *value*. This implies that the class `T` has some comparison operator that allows the algorithm to see whether the current element is the right one. For example, if we have a class Card, for which we define an `operator==()`, then we can use the algorithm as follows:

~~~cpp
class Card {
    std::string val;

public:
    explicit Card(const std::string& value) : val(value) {}
    std::string get_val() const { return val; }
};

bool operator==(const Card& c1, const Card& c2) {
    return c1.get_val() ==  c2.get_val();
}

ostream& operator<<(ostream& os, const Card& c) {
    os << c.get_val();
    return os;
}

int main() {
    vector<Card> v{Card("Ace"), Card("Deuce"), Card("Four"),
        Card("Five"), Card("Eight"), Card("Nine"),
        Card("Jack"), Card("Queen"), Card("King")};
    Card needle1("Eight");
    Card needle2("Seven");
    auto it = find(v.cbegin(), v.cend(), needle1);
    std::string verb;
    if (it != v.cend())
        verb = " is";
    else
        verb = " is not";
    cout << "Card " << needle1 << verb << " in the deck\n";
    it = find(v.cbegin(), v.cend(), needle2);
    if (it != v.cend())
        verb = " is";
    else
        verb = " is not";
    cout << "Card " << needle2 << verb << " in the deck\n";
    return 0;
}
~~~

The output of this program (compiled with clang 3.2.7) is:

<pre>
  Card is in the deck
  Card Seven was not in the deck
</pre>


Notice that the return type of the algorithm is an iterator in the sequence, so to get the element we could dereference the iterator *it*.

### `find_if`, `find_if_not`
One might want to find an element based on some predicate.  This is what the other two forms of `find` are for. These are their prototypes:

~~~cpp
template<class InputIterator, class Predicate>
InputIterator find_if(InputIterator first, InputIterator last,
    Predicate pred);

template<class InputIterator, class Predicate>
InputIterator find_if_not(InputIterator first, InputIterator last,
    Predicate pred);
~~~

As seen for the algorithms presented in previous posts, also in this case we can use both a functor or a closure. Let's see an example with closures:

~~~cpp
int main() {
    vector<int> v{1, 2, 3, 4, 5 ,6};
    auto it = find_if(v.cbegin(), v.cend(),
            [](const int& i){ return i%2 == 0; });
    if (it != v.cend())
        cout << "Found even number: " << *it << '\n';

    // Find next even numbers:
    it = find_if(it + 1, v.cend(),
            [](const int& i){ return i%2 == 0; });
    if (it != v.cend())
        cout << "Found another even number: " << *it << '\n';

    return 0;
}
~~~

In this example, we look for even numbers in a vector. Since the algorithm is defined for each sequence, we can use the iterator returned by the first invocation of `find_if` to start another sequence; only we have to be careful to increment it, otherwise, we are going to get the value 2 again. The result, as expected, is:

<pre>
  Found even number: 2
  Found another even number: 4
</pre>

### Conclusions
As you might have noticed so far, the usage of algorithms is fairly intuitive, and they can be very powerful to express ideas.

In the next installments, we'll see some other interesting variants of the `find` algorithm. Stay tuned!
