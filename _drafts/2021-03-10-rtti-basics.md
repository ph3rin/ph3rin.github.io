---
title: "RTTI Basics"
date: 2021-03-10
categories: c++
permalink: "cxx/rtti-basics"
---

## What is RTTI?

RTTI stands for "[Run-time Type Information][1]". It allows a C++ program to inspect the
type of an object at run-time. Let us look at some examples:

```c++
struct Base {
    virtual void foo() { std::cout << "Base\n"; }
    virtual ~Base() = default;
};

struct Derived : Base {
    void foo() override { std::cout << "Derived\n"; };
};
```

Here we have two classes, `Base` and `Derived`. Notice that they are polymorphic, i.e they have virtual member functions. This is significant to RTTI, as we shall see later.

Now, suppose we have the following:

```c++
void do_something_with(Base* ptr) {
  // do something
}

int main() {
  int choice = 0;
  std::cin >> x;

  Base b;
  Derived d;
  
  if (x == 0) {
    do_something(&b);
  } else {
    do_something(&d);
  }
}
```

Note that when `do_something_with` is called, it gets pointer to `Base`. The actual type (i.e the dynamic type) of the object `ptr` is pointing to is opaque to this function.

Well, not completely opaque.

If we call `ptr->foo()` inside `do_something_with`, the actual type will be revealed by the behavior of the program: if it is merely a `Base` object, then _Base_ will be printed, or else, if it is a `Derived` object, then _Derived_ would be printed.

Well, you might say, that's the working of the v-table. Each polymorphic object contains a pointer to its virtual table, where the information about which virtual functions should be called is stored. That is exactly what we want.

Since each object of a polymorphic type must have its own v-table (even if they share a common base class), we can use that v-table to identity the type of the object!

C++ does expose such mechanism to the programmer, in the form of `dynamic_cast` and `typeid`.

## dynamic_cast


 
[1]: https://en.wikipedia.org/wiki/Run-time_type_information