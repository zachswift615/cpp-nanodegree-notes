# Object Oriented Programming in C++

```cpp
struct Date {
    int day{1};
    int month{1};
    int year{2000}
}
```

initialize member variables with default value by using 
curly syntax in struct definition

## access specifiers
 member variables are public by default

you can make all variables until the next access modifier 
private for example like this

```cpp
struct Date {
    private:
        int day{1};
        int month{1};
        int year{2000}
    }
```

protected means private but also accessible from derived classes ok so in a struct, you can't use `this` you just have to have input variables be a different name than the private variables you're setting 

## classes
invariants are logical conditions that member variables must adhere to

by convention, use structs when variables are independent of each other, but use classes when they related and depend on each other. 

one subtle difference between classes and structs: by default struct members are public and class members are private

## scope resolution
`::` is the scope resolution operator. particularly useful for separating class declaration from class definition.

```cpp
class Date {
    public:
        int Day() const { return day; }
        void Day(int day): // Declare but not define
        int Month() const { return month; }
        void Month(int month) {
            // use same variable names but use :: to specify scope
            if (month >= 1 && month <= 12) Date::month = month;
        }
        int Year() const { return year; }
        void Year(int year) { Date::year = year}
    private:
        int day{1};
        int month{1};
        int year{0};
}

// Define member function Date::Day().
void Date::Day(int day) { 
    if (day >= 1 && day <= 31) Date::day = day; 
}
```

### Namespaces
allows you to group logically related variables and functions together. helps avoid same-named functions in different parts of program

```cpp
namespace English {
    void Greet() {std::cout << "Hello, World!\n"; }
} // namespace English

namespace Spanish {
    void Hello() {std::cout << "Holda, Mundo!\n"; }
} // namespace Spanish

int main() {
    English::Hello();
    Spanish::Hello();
}
```

### `std` namespace
std is just the namespace used by the [C++ standard library](https://en.wikipedia.org/wiki/C%2B%2B_Standard_Library)

## Initializer lists
in general prefer initialization to assignment to avoid cases where member variable gets used before it is assigned.

initialization lists ensure that member variables are initialized _before_ the object is created.

This is why class member variable can be declared `const`, but only if the member variable is initialized through an initialization list. Trying to initialize a const class member within the body of the constructor will not work.
