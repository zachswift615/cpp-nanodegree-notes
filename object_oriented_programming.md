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

So to have a member variable have a default value, you'd do `int year{0}` in the class definition. But if you want to set a constructor parameter to that same year variable upon creation, you'd use an initializer list. So in this expression, `Date::Date(int day, int month, int y) : year(y) {` the `year` is the member variable and `y` is the input param to the constructor

## Constants and constructor syntax
- if you declare a const, it has to be set via an initializer list. 
- Also initializer lists are preferred because they're faster for the compiler
- attributes defined as references must use initialization lists

```cpp
#include <asset.h>
#include <string>

struct Person {
    public: 
        Person(std::string name): name(name) {}
    
    std::string const name;
};

int main() {
    Person alice("Alice");
    Person bob("Bob");
    assert(alice.name != bob.name);
}
```

## Encapsulation

```cpp
#include <cassert>

class Date {
public:
  Date(int day, int month, int year);
  int Day() const { return day_; }
  void Day(int day);
  int Month() const { return month_; }
  void Month(int month);
  int Year() const { return year_; }
  void Year(int year);

private:
  bool LeapYear(int year) const;
  int DaysInMonth(int month, int year) const;
  int day_{1};
  int month_{1};
  int year_{0};
};

Date::Date(int day, int month, int year) {
  Year(year);
  Month(month);
  Day(day);
}

bool Date::LeapYear(int year) const {
    if(year % 4 != 0)
        return false;
    else if(year % 100 != 0)
        return true;
    else if(year % 400 != 0)
        return false;
    else
        return true;
}

int Date::DaysInMonth(int month, int year) const {
    if(month == 2)
        return LeapYear(year) ? 29 : 28;
    else if(month == 4 || month == 6 || month == 9 || month == 11)
        return 30;
    else
        return 31;
}

void Date::Day(int day) {
  if (day >= 1 && day <= DaysInMonth(Month(), Year()))
    day_ = day;
}

void Date::Month(int month) {
  if (month >= 1 && month <= 12)
    month_ = month;
}

void Date::Year(int year) { year_ = year; }

// Test
int main() {
  Date date(29, 2, 2016);
  assert(date.Day() == 29);
  assert(date.Month() == 2);
  assert(date.Year() == 2016);
    
  Date date2(29, 2, 2019);
  assert(date2.Day() != 29);
  assert(date2.Month() == 2);
  assert(date2.Year() == 2019);
}
```

## Accessor Funcitons
you should mark them as const because they should never change stuff obv.

you have to have them if there are setter function to private members. 

```cpp
// Example solution for creating a BankAccount class
#include <iostream>
#include <string>

class BankAccount
{
  private:
      // Class attributes:
      
      long int number;
      std::string owner;
      float amount;

  public:
      // Set  methods:
      void setNumber(long int number);
      void setOwner(std::string owner);
      void setAmount(float amount);
      // Get methods:
      long int getNumber() const;
      std::string getOwner() const;
      float getAmount() const;
};

// Implementation of Set methods:
void BankAccount::setNumber(long int number) {
    // Changing attribute to incoming value
    BankAccount::number = number;
}

void BankAccount::setOwner(std::string owner) {
    BankAccount::owner = owner;
}

void BankAccount::setAmount(float amount) {
    BankAccount::amount = amount;
}

// Implementation of Get methods:
long int BankAccount::getNumber() const {
    // Getting specified attribute
    return BankAccount::number;
}

std::string BankAccount::getOwner() const {
    return BankAccount::owner;
}

float BankAccount::getAmount() const {
    return BankAccount::amount;
}

// main function
int main(){
  BankAccount ba;
  ba.setAmount(100);

  std::cout << ba.getAmount() << std::endl;
}
```

## Mutator function (Setter functions)

### Exercise: Car Class

c style strings: https://www.learncpp.com/cpp-tutorial/c-style-strings/

In this lab you will create a setter method that receives data as an argument an converts it to a different type. Specifically, you will receive a string as input and convert it to a character array.

Create a class called Car.
- Create 3 member variables: horsepower, weight and brand. The brand attribute must be a character array.
- Create accessor and mutator functions for all member data. The mutator function for brand must accept a C++ string as a parameter and convert that string into a C-style string (a character array ending in null character) to set the value of brand.
- The accessor function for the brand must return a string, so in this function you first will need to convert brand to std::string, and then return it.

```cpp
// Partial example solution for Car class 
// getters and setters for brand only
#include <string>
#include <cstring>
#include <iostream>
// Define Car class
class Car {
    // Define private attributes
    private:
        int horse_power;
        int weight;
        char *brand;
    // Declare public getter and setter
    public:
        void SetBrand(std::string brand_name);
        std::string GetBrand() const;
};

// Define setter
void Car::SetBrand(std::string brand_name) {
    // Initialization of char array
    Car::brand = new char[brand_name.length() + 1];
    // copying every character from string to char array;
    strcpy(Car::brand, brand_name.c_str());
}

// Define getter
std::string Car::GetBrand() const {
    std::string result = "Brand name: ";
    // Specifying string for output of brand name
    result += Car::brand;
    return result;
};

// Test in main()
int main() {
    Car car;
    car.SetBrand("peugeot");
    std::cout << car.GetBrand() << "\n";
}
```

## Pyramid Class Exercise

```cpp
#include <cassert>
#include <stdexcept>

// TODO: Define class Pyramid
class Pyramid {
    public:
        Pyramid(int length, int width, int height) {
            Validate(length, width, height);
            Width(width);
            Length(length);
            Height(height);
        }

        void Width(int w) {
            width = w;
        };

        void Length(int l) {
            length = l;
        };

        void Height(int h) {
            height = h;
        };

        int Width() {
            return width;
        };

        int Length() {
            return length;
        };

        int Height() {
            return height;
        };

        double Volume() {
            return (Length() * Width() * Height()) / 3;
        };

    private: 
        int width{0};
        int length{0};
        int height{0};
        void Validate(int l, int w, int h) {
            if (l < 0 || w < 0 || h < 0)
                throw std::invalid_argument("Negative dimention");
        }
};

// Test
int main() {
  Pyramid pyramid(4, 5, 6);
  assert(pyramid.Length() == 4);
  assert(pyramid.Width() == 5);
  assert(pyramid.Height() == 6);
  assert(pyramid.Volume() == 40);

  bool caught{false};
  try {
    Pyramid invalid(-1, 2, 3);
  } catch (...) {
    caught = true;
  }
  assert(caught);
}
```

## Static members
usually declared in the class, but must be defined outside the class in the global scope. The only exception is if the static member is marked as constexpr then it can be both declared and defined inside the class.

```cpp
struct Kilometer {
  static constexpr int meters{1000};
};
```
