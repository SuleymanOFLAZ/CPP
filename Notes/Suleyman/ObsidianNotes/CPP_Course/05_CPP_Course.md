[[00_Course_Files]]
#Cpp_Course 
If which function is called determined on compile-time, If there is no program run-time cost, for example function overloading, this is called <mark style="background: #BBFABBA6;">early binding</mark> or <mark style="background: #BBFABBA6;">static binding</mark>. 

This called <mark style="background: #BBFABBA6;">late binding</mark> or <mark style="background: #BBFABBA6;">dynamic binding</mark>, if which function is called is determined on run-time. In this case another code determines which function is called on run-time. This method is used in C++, Java and C#. Even this is the only method in some languages.

When we look at <mark style="background: #BBFABBA6;">inheritances</mark> in C++, we will see in detail the late binding or dynamic binding.

# Type cast operators
#Cpp_TypeCastOperators
There is only one type conversion operator in C. That is type-cast operator. The expression that generated with type-cast operator is not a L value expression in C.

<mark style="background: #FFF3A3A6;">Keep caution. The meaning of type cast and type conversion is not same</mark>.

Type conversion can be implicit or explicit. Implicit type conversion means the compiler determines the type depending on code.

Type cast is a type conversion with usage of an operator.

Why we have new type-cast operators in C++? Type conversion is done with different purpose and there is different risk levels. This situation is valid both C and C++ and C has only one operator for this purpose.

This purposes are:
1. The compiler would do the same implicit conversion, if we didn't use the type cast. But we use the type cast operator in order to show out intend. Think that case, there is a data loss with implicit type conversion but that is okay for us. How would he reader know that? For that reason we use type conversion. The second benefit of this is the compiler doesn't warn us. 
2. Sometimes we do type conversion between address types. <mark style="background: #BBFABBA6;">Strict aliasing rule</mark>.
3. The type conversion from a const object's address to non-const object's address.

<mark style="background: #FFB8EBA6;">Note</mark>: With brace (uniform) initialization the compiler throw an error, if a data loss occurs.

><mark style="background: #FFF3A3A6;">We have four new type-cast operators in C++. These are keywords also. These are:</mark>
> - <mark style="background: #BBFABBA6;">static_cast</mark>
> - <mark style="background: #BBFABBA6;">const_cast</mark>
> - <mark style="background: #BBFABBA6;">reinterpret_cast</mark>
> - <mark style="background: #BBFABBA6;">dynamic_cast</mark>

This type cast operators are from before modern C++.

<mark style="background: #ADCCFFA6;">static cast</mark>: The int divided int into double example. Integer to real number conversions.

<mark style="background: #ADCCFFA6;">cont cast</mark>: The type conversion from a const object's address to non-const object's address. Keep caution on this, most of the time const conversion lead undefined behavior. For example:
```cpp
int main()
{
	const int x = 10;
	int* p = &x;
}
```
Upper code is syntax error in C++. There is no type conversion from const int* to int*. But the compiler warn us in C, not a syntax error. But we prevent the error with type cast and if we want to change the const object, this is undefined behavior. We need const cast rarely, but we need to be careful about them. 
Another example of the const cast is the strchr() example.
Reference semantics is included to const cast.

<mark style="background: #ADCCFFA6;">reinterpret cast</mark>: Type conversion between different address types.The most dangerous type cast is using one object's address as another object's address. For example a type conversion from a struct's address to char* for byte search. 

<mark style="background: #ADCCFFA6;">dynamic cast</mark>: This conversion cast don't exist in C. It is related to inheritance. This actualizes <mark style="background: #BBFABBA6;">down cast</mark>. The conversion from parent class to child class in inheritance. This is actualized with a code that runs on run-time (The other type casts are done in compile-time). We will look that later.

The syntax of these cast operators is following. The parentheses is not related to priority, the syntax is like that:
```cpp
static_cast<target type>(expression)
```

<mark style="background: #FFF3A3A6;">Why a one type-cast is not good for all those type conversion</mark>:
1. It 2. The compiler throws syntax error if we try to make a wrong type conversion by mistake. For example if we try a static const conversion instead of const cast.

<mark style="background: #FFF3A3A6;">We cannot use that type cast operators instead each other. But there is one exception: the type</mark> <mark style="background: #D2B3FFA6;">void*</mark>. The type conversion from void* to other pointer types are legal in C, but not legal in C++. static_cast and reinterpret_cast can be used from void* to another pointer type.

<mark style="background: #FFF3A3A6;">We use static_cast from enum types to integer types or enum types.</mark>

<mark style="background: #FFF3A3A6;">There is no two conversion at same time. The following code is syntax error. There are two conversion here: int is a const object. For example</mark>:
```cpp
int main()
{
	const int x = 10;
	char* p = reinterpret cast<char*> (&x); // Syntax error, there is two type converion here
	const_cast<char *>(reinterpret_cast<const char*>(&×))); // OK
}
```

# Implicit type conversion on C++

1. Integer -> Real
2. Real -> Integer
3. Bidirectional conversion between int, long int, long long int, short, char, float, double, long double 
4. There is implicit type conversion between addresses.
5. T -> const T
6. T* -> void*
7. There is no implicit type conversion void* to T*
8. enum (enum class) -> arithmetic
9. There is no implicit type conversion from arithmetic to enum

# Scoped enum
Scoped enum (enum class) added to the language with modern C++. This is not related to classes, this is only keyword. 

<mark style="background: #FFF3A3A6;">Problem with enums:</mark>
1. Good things are: There are no implicit type conversion from arithmetic types to enum and here is not implicit type conversion between enums. But the bad things is: There is implicit type conversion from enum types to arithmetic types.
2. Each enum type has a <mark style="background: #BBFABBA6;">underlying type</mark>. Enum types are a integer type. The underlying type of enum type must be int in C. But in C++, the compiler determines enum's underlying type. This comes from before modern C++. Underlying type can be an integer and can be int as smaller. So int, unsigned int, long int, long long int etc. The bad thing is we must use 4 bytes at least for an enum and this is a memory problem. The second problem is the portability of program is effected by this design because the underlying type can be differ between compilers.
3. The enum size not know is a enum declared like that: <mark style="background: #D2B3FFA6;">enum Color;</mark>. Why we do this, because we don't want to include the header file that Color is declared. Because including more header file means dependency is increased. So, the compiler doesn't know the size of Color and that situation would syntax error. Enum is a incomplete type in C++. This is not an issue in C, because the underlying type of enum is int, that means enum is a complete type in C.
4. Enumerators of all enums considered in same scope. So we cannot define same enumerator name in different enums.

<mark style="background: #ADCCFFA6;">Complete Type and Incomplete type</mark>: A C topic. If size of a type is not known the type is a incomplete type.

<mark style="background: #FFF3A3A6;">Modern C++ added enum class and added some syntax features to enums.</mark>

# enum class
Differences of enum class:
1. <mark style="background: #FFF3A3A6;">Underlying type can be stated directly with syntax</mark>. If we didn't state the underlying type, it is be int by default. That solves the problems that we mentioned. <mark style="background: #FFF3A3A6;">This feature added to classical enums too</mark>, not only to enum classes.
```cpp
enum class Color {red, blue, green}; // underlying type: default int
----
enum class Color : char{red, blue, green}; // We can state the underlying type
----
enum class Color : unsigned{red, blue, green}; // We can state the underlying type
----
enum class Color: int; // Forward decleration example. The rules is existed on classical enums too
----
enum Color : unsigned{red, blue, green}; // We can state the underlying type
```
2. enum classes has their own scopes. This solves the scope problem of classical enums.
```cpp
enum class Color {red, blue, green};

Color mycolor = red; // Syntax error.
Color mycolor = Color::red; // We used binary resolution operator
```
Writing that resolution operator makes the code hard to read. <mark style="background: #FFF3A3A6;">In C++20, a new feature is added to the language</mark>: <mark style="background: #FFF3A3A6;">We can use the enum class without resolution operator</mark>, if we declare a special using declaration. The enum can be used without resolution operator in the scope of the using declaration: <mark style="background: #D2B3FFA6;">using enum Enum_Name;</mark>. Or we can use only one enumerator without specifying the resolution operator in the scope of the using declaration: <mark style="background: #D2B3FFA6;">using Color::blue;</mark> This syntax is following:
```cpp
enum class Color {red, blue, green};

int main()
{
	using enum Color;
	Color c = red; // this is valid in the scope of upper decleration
---
	// Or we can write like following to use blue without resolution operator
	using Color::blue; // the using decleration for only one u enumeration. Note that there is no enum keyword in decleration
	Color x = bleu;
}
```

<mark style="background: #FFB8EBA6;">Note</mark>: There are using keywords that corresponds to different meanings. We look through them later. 

3. <mark style="background: #FFF3A3A6;">There are not implicit type conversion from enum classes to arithmetic types.</mark>


The added features to the classical enums with modern C++:
1. specifying the underlying type
2. We can use classical enums with the resolution operator just like enum classes. This is not a effect, it is added for just to make the syntax same.

<mark style="background: #FFF3A3A6;">Use the enum classes if there is no special situation to use the classical enums.</mark>

<mark style="background: #FFF3A3A6;">Enum classes are not a class type.</mark> The can test that with following syntax, and note that is_class_v<> is a <mark style="background: #BBFABBA6;">compile-time literal</mark>, included with <mark style="background: #BBFABBA6;">type_traits</mark>, and we will look them in detail later:

```cpp
#include < type_traits>
#include ‹stdexcept>

using namespace std;

enum class Color {Blue};

int main()
{
	constexpr boo1 b = is_class_v<Color>; // This is false
}
```

# decltype specifier/operator
Type deduction was exist before modern C++. But, type deduction is only exist on usage of templates. Modern C++ spread the type deduction on most of the language and add new type deduction tools. One of them is auto keyword, and we look it a little bit. The another one is the <mark style="background: #BBFABBA6;">decltype specifier</mark>. Auto and decltype are  completely different tools.
We use auto on a variable and initialize a variable to deduct the type. But usage of decltype is different and not related to initialization. <mark style="background: #FFF3A3A6;">decltype is a keyword and we write a expression between parentheses after the keyword and that syntax corresponds to a type and we can use that syntax everywhere that we use a type.</mark> <mark style="background: #FFF3A3A6;">The type deduction is done at compile time with decltype</mark>. There is no obligation to initialize or declare a variable with decltype.

```cpp
decltype(expr) foo(); // decltype(expr) corrensponds to the function return type

int x = 5;
decltype(x) y; // decltype(x) means int in this situation

std::vector<decltype(x)> z;
```

<mark style="background: #FFF3A3A6;">How the type deduction is done with decltype? The type deduction that is done with decltype is done with two format</mark>:
1. <mark style="background: #FFF3A3A6;">The operand of the decltype is in a name form. The dot operand of a struct or class <mark style="background: #D2B3FFA6;">decltype(a.b)</mark> and -> operand of a pointer <mark style="background: #D2B3FFA6;">decltype(ptr->y)</mark> is included in that name form</mark>.
```cpp
int x = 10;
const int cx = 20;
int* ptr = &x;
int& r = x;

int main()
{
	decltype(c) a = 5; // decltype is int
	decltype(cx) a = 5; // decltype is const int 
	decltype(cx) a; // Syntax error. const int must be initialized
	decltype(ptr) a = &x; // decltype is int*
	decltype(r) a = x; // decltype is int&
	decltype(r) a; // syntax error. int& must be initialized

	int a[] = {1, 2, 3};
	decltype(a) b; // decltype is int[3]. There is not array decay with decltype
	
}
```
<mark style="background: #FFF3A3A6;">There is no array decay with decltype.</mark> How was the declaration is done is the decltype.

2. <mark style="background: #FFF3A3A6;">The operand of the decltype is a expression.</mark> The rule set for this notation is different form previous (the operand was a name). Let the expression that is decltype's operand be of T type. (a expression has a type and that type cant be a reference, but can be a int, double or int* etc). The type that achieved with decltype is depends on the decltype's operand's value category. So, the expression can be in PR value category or L value category or X value category.
Let the expression that is decltype's operand be of T type. <mark style="background: #FFF3A3A6;">Depending on the value category:</mark>
- <mark style="background: #FFF3A3A6;">PR value: decltype is T</mark>.
- <mark style="background: #FFF3A3A6;">L value: decltype is a L value reference, that mean the decltype is T&</mark>.
- <mark style="background: #FFF3A3A6;">X value: decltype is a R value reference, that means the decltype is T&&</mark>.

We not see he X value yet. But a function call to a function that returns a R value reference is a X value expression.
```cpp
decltype(× + 5)
decltype(*ptr)
decltype((x)) // x is a name but (x) is a expression
---
int&& foo();

int main()
{
	int x = 10;
	double dval = .5;
	decltype(× + 5) ival; // decltype is int. Because the x+5 is PR value end type of it is int.

	decltype(× + dval) ival; // decltype is double. Because the x+5 is PR value and type of it is dobule.

	int* ptr = &x;
	decltype(*ptr) y = x; // decltype is int&. Because the *ptr is a L value and type of it is int. 
	decltype(*ptr) y; // syntax error. Because decltype is int& and not initialized.

	int a[5]{};
	int y{};
	decltype(a[2]) x = y; // decltype is int&. Because the a[2] is a L value and type of it is int.

	int a = 5;
	int b = 6;

	decltype(a) x = b; // decltype is int. Because a is a name
	decltype((a)) y = b: // decltype is a int& because (x) is a L value expression. Parentheses are priority operator here.

	decltype(foo()) x = 10; // decltype is a int&&. Because the foo() is a X value expression (returns a R value reference). This is same as writing int&& x = 10;
}
```

<mark style="background: #FFF3A3A6;">Example</mark>: How would the following program print ? (450 and 10, because ):
```cpp
 #include <iostream>

using namespace std;

int main()
{
	int a = 10;
	int y = 45;
	
	decltype(++a) x = y; // ++a is a L value expression. decltype is int&
	× *= 10;
	
	sta::cout << "у = " << y < "\n";
	std::cout << "a = " << a << "\n";
}
```
<mark style="background: #FFF3A3A6;">Why a prints 10 instead of 11. The expression of decltype's operand is a</mark> <mark style="background: #BBFABBA6;">unevaluated context</mark>. <mark style="background: #FFF3A3A6;">The compiler doesn't produce a code if the expression is a unevaluated context. This is guarantee with the language rule. C language has one example of unevaluated context, but C++ has many. The only unevaluated context in C is the sizeof operator, and the sizeof operator's operand doesn't evaluated in C++ too.
Examples</mark>:
```cpp
// Unevaluated Context exmaple.
using namespace std;

int main()
{
	int* p = nullptr;
	int a[3]{};
	auto n1 = sizeof(*p); // OK. The compiler does't dereferance the null pointer
	auto n2 = sizeof(a[5]); // OK. The cmopiler doesn't use the a[5]
}
```

<mark style="background: #FFB8EBA6;">Additional example</mark>:
```cpp
int main()
{
	int x = 10;
	int&& r = 5 + x;
}
```
<mark style="background: #FFF3A3A6;">Question 1</mark>: What is the type of variable r? = int&&
<mark style="background: #FFF3A3A6;">Question 2</mark>: What is the value category of expression r? = L value expression, because r is a name and all names is a L value expression.

<mark style="background: #FFB8EBA6;">The upper example is asked in exams in function overload form</mark>:
```cpp
void func(int&&)
{
	std::cout << "1";
}

void func(int&)
{
	std::cout << "2";
}

int main()
{
	int&& r = 10;
	func(r);
}
```
<mark style="background: #FFF3A3A6;">Question</mark>: Which function is called in upper code? The answer is 2nd function because r is a name and a L value expression.

---
Return: [[00_Course_Files]]