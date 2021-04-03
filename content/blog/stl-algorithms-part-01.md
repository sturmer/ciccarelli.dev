---
title: STL Algorithms, part 1
kind: article
created_at: 2013-12-23
tags:
 - STL
 - C++
---

- [Part 1](/blog/stl-algorithms-part-01/)
- [Part 2](/blog/stl-algorithms-part-02/)
- [Part 3](/blog/stl-algorithms-part-03/)
- [Part 4](/blog/stl-algorithms-part-04/)
- [Part 5](/blog/stl-algorithms-part-05/)

### Why algorithms
Standard algorithms have a prominent value in the C++ standard template library (STL). They present the great advantage of expressing an operation applied on a range of elements in a container. In this series, I am going to review all the algorithms introduced in the C++11 standard, presenting it in digestible chunks, and will provide at least an example of application for each one.

### `all_of`
We might want to determine whether all elements in a range satisfy some property. For example, suppose we want to find out if all the persons subscribed to this blog come from Europe. We might keep a record of a Person in the form:

~~~cpp
enum Continent {
    EUROPE, ASIA, AMERICA, AFRICA, AUSTRALIA
};

struct Person {
  std::string name;
  Continent continent;
};
~~~

Now say that we have a `std::vector` of customers. To find the answer to our previous question, we might do something like the following:

~~~cpp
// declare p1, p2, p3
std::vector<Person> persons = {p1, p2, p3};
bool all_european = all_of(persons.cbegin(), persons.cend(),
        [](const Person& p) {
            return p.continent == Continent::EUROPE;
        });
if (all_european)
    cout << "All European visitors\n";
else
    cout << "Visitors from outside Europe\n";
~~~

Formally, the signature for `all_of` is:

~~~cpp
template <class InputIterator, class Predicate>
bool all_of(InputIterator first, InputIterator last, Predicate pred);
~~~

One gotcha: the algorithms returns true if the range is empty. It runs in linear time on the number of elements in the range. You can find the complete code is at [this link](https://gist.github.com/sturmer/8082362).

### `any_of`
Now suppose we want to know whether *at least one* of the customers is American. We can accomplish this with the algorithm `std::any_of`, which intuitively enough can be used like the following:

~~~cpp
// ...
bool any_american = any_of(persons.cbegin(),
        persons.cend(),
        [](const Person& p) {
            return p.continent == Continent::AMERICA;
        });
if (any_american)
      cout << "We had an American visitor\n";
  else
      cout << "No visitors from Americas\n";
~~~

As you can see, we recycled the Predicate. The pitfall here is the reverse of the one for all_of: this algorithms returns false if the range is empty. Find the gist [here](https://gist.github.com/sturmer/8082455).

### `none_of`
Let's change example. Say we have a vector of Shape class pointers. Shape can be derived in order to provide more specific properties:

~~~cpp
struct Shape {
    Shape() {}
    virtual float area() = 0;
    virtual ~Shape() {}
};

class Rectangle : public Shape {
private:
    float height;
    float width;

public:
    float area() override {
        return height * width;
    }

    Rectangle(float h, float w) : height(h), width(w) {}
};

class Circle : public Shape {
private:
    float radius;

public:
    float area() override {
        return radius * radius * 3.14; // approx
    }

    Circle(float radius) : radius(radius) {}
};

class Triangle : public Shape {};
~~~

Now we want to see whether in a vector of Shape pointers there is any Triangle.  Since `none_of` is a negative predicate, and reasoning in negative terms is generally harder, we define the predicate outside the invocation of the algorithm, to make things clearer:

~~~cpp

// define rectangles and circles, no triangle
// put them in a vector
vector<Shape*> v{&c1, &c2, &r1, &r2, &r2, &r4};
auto is_triangle = [](Shape* s) {
  return dynamic_cast<Triangle*>(s) != nullptr;
  };
bool no_triangle = none_of(v.cbegin(), v.cend(), is_triangle);
    if (no_triangle)
        cout << "No triangle\n";
~~~

We use the `dynamic_cast` to determine if the pointer points to a Triangle, in which case it would return a non-null pointer. The `std::none_of` algorithm, as the ones mentioned above, has linear complexity depending on the number of elements in the range. Grab the gist [here](https://gist.github.com/sturmer/8083022).

### In the next episode
We have covered three basic algorithms for testing properties of elements in a container: `all_of` checks whether all the elements in a range satisfy a predicate, `any_of` checks whether at least one element satisfies the predicate, while `none_of` checks that none of the elements satisfies the predicate. Lambdas give us a way of expressing very clearly the predicate: if you feel uncomfortable with them, you might want to have a look at the mini-series I have written about them ([part 1][lp1] and [part 2][lp2]).

In the next installment, we are going to get into more algorithms provided by the standard.

[lp1]: /blog/lambdas-in-c11/
[lp2]: /blog/lambdas-in-c11-part-2/
