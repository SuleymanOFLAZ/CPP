[[00_Course_Files]]
#Cpp_Course 
# C and C++ Function Overload Disagreement

There is no function overloading in C, but there is in C++. This brings some problems about the linker. The compiler writes an external reference to the linker for function calls if the function call is not inlined. The C doesn't has function overloading and there cannot be same name function in C. The compiler created the function reference only depending on the function name. But, this cannot be with only function names in C++, because of the function overloading that means same named functions. The C++ compiler writes function references to the linker depending on not only the function names, depending on the function names and the function's parameters.
This mismatch leads to compile and link issues on codes that wrote in C  and compiler with the C compiler, but the function is called in C++ code that compiled with the C++ compiler. The C++ compiler generates links to the linker with C++ style for function call,  if he compiler doesn't know the code is wrote in C. The linker cannot find the function.
We must to tell the C++ compiler that the function compiled in C. The compiler can write the function reference with C style instead of C++ style after that declaration. This declaration is called <mark style="background: #BBFABBA6;">extern C declaration</mark>.

The extern C declaration can made in two different way:
We can make extern C declaration like following:
```cpp
extern "C" int foo(int, int);
```

We can make extern C declaration for more than one function like following:
```cpp
extern "C" {
	int foo1(int, int);
	int foo2(int, int);
	int foo3(int, int);
	int foo4(int, int);
}
```

We generally want that the header file can be included from both C and C++ codes. The C compiler give error if we write the extern C declaration because C syntax does't has that. There is a <mark style="background: #BBFABBA6;">predefined symbolic constant</mark> that shows the compiler is C++ compiler. <mark style="background: #BBFABBA6;">__cplusplus</mark> is considered predefined if the compiler is a C++ compiler. C compilers doesn't have that predefined constant. So we can write the header file that compatible with both C and C++.
```cpp
#ifdef __cplusplus
extern "C" {
#endif
	int foo1(int, int);
	int foo2(int, int);
	int foo3(int, int);
	int foo4(int, int);
#ifdef __cplusplus
}
#endif
```

# Class
The main tool for <mark style="background: #BBFABBA6;">the data abstraction</mark> and <mark style="background: #BBFABBA6;">the object oriented programming</mark> is class in C++.  The main tool is class and it is frequently used, the standard libraries uses classes. There is not corresponding too for class in C, but most similar one is structures. Structures in C++ not same as the C, mostly have commonalities but structures in C++ are in nature of classes and structures has little difference from classes. So the structures and the classes are both classes in C++.

A struct cannot be empty in C, this would be a syntax error. But a struct or class can be empty in C++, and this is called <mark style="background: #BBFABBA6;">empty class</mark>. Empty classes has special purposes and they are used.

The name that we choose as a tag is used directly as type name in C++ (enum, class, struct), but cannot used in C and it must be a type declaration to to directly use the name in C. So there is no need to a typedef declaration in C++. We can do it but nobody do this because it is unnecessary.

The space between a classes's curly braces is open to declaration. The name that declared in that area is called <mark style="background: #BBFABBA6;">class members</mark>. 
A class member can be in three different categories in C++. These categories are:
- <mark style="background: #BBFABBA6;">data members</mark>
- <mark style="background: #BBFABBA6;">member functions</mark>
- <mark style="background: #BBFABBA6;">type members (nested types)</mark>

Data members are the object that holds the values.

There is only one meaning of '"function" in C. But there are two meaning of the "function" in C++ with addition of the classes. The second meaning of the function is the function that defined in a class as a <mark style="background: #BBFABBA6;">member function</mark>. The first meaning of the function (that came from C) is called <mark style="background: #BBFABBA6;">global function</mark> or <mark style="background: #BBFABBA6;">free function</mark> or <mark style="background: #BBFABBA6;">stand-alone function</mark>. 

A type member (member type or nested type) can be a class (both class and struct) or enum.

```cpp
void f(int);

class Nec {
	int mx, my; //data members 
	void foo(int); //member function
	class Nested { // Nested is a type member of Nec class
		//
	};
};
```

## <mark style="background: #FF5582A6;">Scope in C++
</mark>
<mark style="background: #ADCCFFA6;">Scope</mark> is a <mark style="background: #D2B3FFA6;">attribute</mark> of <mark style="background: #FFB86CA6;">names</mark> (identifiers). <mark style="background: #BBFABBA6;">A scope</mark> is a scope that the name can be used legally. The compiler searches and finds a name if the name is used in its scope. The compiler can't find the name if name is used in outside of its scope. 
<mark style="background: #FFF3A3A6;">The important part of C++'s rules is related to scope/<mark style="background: #BBFABBA6;">name-lookup</mark></mark>. Scope and name lookup are different perspectives to same mater. Scope means there is a name and where i can took for this name. Name look up means that we use a name and the compiler must understand who's name is this at compile time. Whose name is this name? is determined by name lookup.
There is complexity difference between C and C++ name lookup rules. C's name lookup rules are so simple in comparison to C++ name lookup rules. So we will see the name lookup in many topics as a subtopic through the course.

> [!info] Topic: Name Lookup
> Name lookup is a process that the compiler searches the name that we used in an expression to which determine whose name it it. The is a compile-time process. The compiler must find the names declaration, other ways this would be a syntax error.

> [!example]  Exmaple: Name Lookup Example
> Small description of name lookup:
> ```cpp
> int x();
>
>int main()
>{
>	int x = 10;
>	x();
>}
> ```
> With the ==x();== syntax the compiler looks for x (means makes name lookup) in upper example. The compiler finds the ==int x=10;== and at that point the name lookup finishes. Than the compiler checks the syntax if is it legal or not, the ==x();== is a function call so the compiler throws a syntax error. If the ==int x=10;== doesn't exist the compiler would find the ==int x();== at the name lookup and that isn't a syntax error due to ==x();== expression.

>[!tip] Tip: How the name lookup can conclude?
>The name lookup can conclude two different ways in C++. These situations are:
>1. The name that searched for is not found. This is a syntax error. The compiler searched a name and not found the declaration if the name.
>2. More than one declaration is found as the result of the search process (name lookup), but the rule of the language is not determinator to which one is to choose. This situation is not exist in C. We looked it through in function overloading topic. This situation is named as ==ambiguity==.

> [!tip] Tip: There are two important rules of name lookup. (C and C++)
> 1. Name lookup is done in a order that is determined by the language rules. For example the compiler first looks the local scope then looks the global scope if it don't find the name in local scope.
> 2. Name lookup is finishes upon finding the looked name and it don't start again after that. After name lookup the ==context control== is started. <mark style="background: #FFB86CA6;">The compiler doesn't make a second search if the found declaration causes a syntax error</mark>. This is not same in every programming language)

>[!tip] Tip: Scope vs Name Lookup
>Scope: Where the name can be used in aspect of the programmer.
>Name Lookup: Where the name's declaration can be found in aspect of the compiler. A name must be used in an expression for mentioning a name lookup.


Scope rules differ in C++ in comparison to C. First let's remember the scope rules of the C.

>[!info] Info: The scope categories of C:
>1. File scope
>2. Block scope
>3. Function prototype scope
>4. Function scope

Now let's look at scope categories of the C++:

>[!info] Info: The scope categories of C++:
>1. namespace scope (instead file scope of C)
>2. class scope (corresponding of this is not exist in C)
>3. block scope
>4. function prototyp scope
>5. function scope

Scope of the names that declared in a class definition (a member) is class scope. These names are subjected to different rules. <mark style="background: #FFB86CA6;">The following conditions must be provided for searching a name that in class scope</mark>:
1. <mark style="background: #FFB86CA6;">The name must be used as qualified.</mark> Described name is called <mark style="background: #BBFABBA6;">qualified name</mark>.

>[!info] Info: Qualified Name
>A name is a qualified name if the name is used as operand of followed operators:
>1. Dot operator: '.' (A member selection operator)
>2. Arrow operator: '->' (A member selection operator)
>3. Scope resolution operator: '::'


### Scope resolution operator (::)
One of the most used operator in C++. <mark style="background: #FFB86CA6;">There are two different usage of this operator</mark>. These are:
1. <mark style="background: #FFB86CA6;">unary scope resolution operator</mark>
2. <mark style="background: #FFB86CA6;">binary scope resolution operator</mark>

### The unary scope resolution operator
 Unary means the operator takes one operand. <mark style="background: #FFB86CA6;"><mark style="background: #BBFABBA6;">The unary resolution operator</mark> gives a order to the compiler that the qualified name must be searched at namespace scope</mark>. With considering we haven't been looked the namespaces yet, the operator means the name must be searched at global scope. With other way of expression, This operator tells the compiler not look that name in local scope.

>[!example] Example: The unary scope resolution operator
>```cpp
>int x = 34;
>
>int main()
>{
>	int x = 12;
>	x++;
>	::x++;
>}
>```

### The binary scope resolution
<mark style="background: #FFB86CA6;"><mark style="background: #BBFABBA6;">The binary scope resolution</mark> operator is used in two cases</mark>:
1. used with a class name
2. used with a namespace name

The name is searched in the class scope if the binary resolution operator used with the class name and the searched name. The name is searched in the namespace scope if the binary resolution operator used with the namespace name and the searched name. 

>[!attention] Attention, example: A static class date member exmaple
>```cpp
>class Myclass {
>public:
>	int x; // Case 1
>	static int x; // Case 2
>};
>
>int main()
>{
>	Myclass::x = 10; // Case 1: // Error the object is not defined
>	Myclass::x = 10; // Case 2: // OK. x is static
>}
>```

>[!important] Important topic: The control phases in C++.
>The code control is done in three phases at most of the situation in C++. This topic is frequently asked in interviews. These controls are done in following order:
>1. Name lookup
>2. context control
>3. access control
>
> **Name lookup**: Searching for the name.
> **Context control**: The checks for code legality according to the language rules after name lookup.
><mark style="background: #BBFABBA6;">**Access control**</mark>: Access control is not exist in C (every code can access data members in C). The languages like C++, C# and Java check that which codes are have rights to use the name in case of a name using. 

### Access Control
<mark style="background: #FFB86CA6;">Class members can be in three different status in order to the access control that the compiler is done</mark>. These statutes are:
1. <mark style="background: #BBFABBA6;">public members</mark>
2. <mark style="background: #BBFABBA6;">protected membres</mark>
3. <mark style="background: #BBFABBA6;">private members</mark>

The whole that the all public members are existed are called <mark style="background: #BBFABBA6;">public interface</mark>. The whole that the all protected members are existed are called <mark style="background: #BBFABBA6;">protected interface</mark>. The whole that the all private members are existed are called <mark style="background: #BBFABBA6;">private interface</mark>.

This means a class's member must be one of these status unlike C. 

>[!info] **Public members**: 
>All code can use the public members without a access restriction.

>[!info] **Private members**: 
>Private members are only open to implementation of the class and closed to the clients. That means only the class codes itself can use the private members. The principle that called <mark style="background: #BBFABBA6;">data hiding</mark> or <mark style="background: #BBFABBA6;">information hiding</mark> is related to private. We acquire a lot of advantage with making the members private. The most important advantage is clients can't use that members, If they try to access to private members that would be syntax error. The clients doesn't effect from the changes because they are not using that members when we do changes on private members.

>[!info] **Protected members**: 
>Protected members are related to <mark style="background: #BBFABBA6;">inheritance</mark> tool set. Inheritance is a tool that frequently used in object oriented programming. We can automatically create a new class that takes the public interface of another class with inheritance mechanism. The inherit class can't use the private members of its parent class, but can use protected members of its parent class. That means a protected member can be used by a inherited class but can't be used by outsider code.

<mark style="background: #FFB86CA6;">There are three keyword for access control</mark> that called <mark style="background: #BBFABBA6;">access specifiers</mark>: public, private and protected. These keyword is represent a area in the class, not a member in C++. This keywords are most common between programming languages and they are represent a member in some languages. We can use these keywords multiple time in a class definition.

>[!example] Example: Access specifiers syntax
>```cpp
>class Myclass {
>public:
>	// public interface area
>private:
>	// private interface area
>protected:
>	// protected interface area
>};
>```

<mark style="background: #FFB86CA6;">The default area is private in case no access specifier is specified with class keyword, but the default area is public in case no access specifier is specified with struct keyword.</mark>

>[!example] Example: Access control
>```cpp
>class Myclass {
>	int x;
>};
>
>int main()
>{
>	Myclass m;
>	m.x = 12; // Access control: Syntax error, x is not public member
>}
>```

>[!important] Important Note:
>A class's public, protected and private areas are not a scope. So we cannot declare same name members between these areas. For example following is a syntax error.
>```cpp
>class Data{
>public:
>	int x;
>private:
>	double x; // Syntax error, x is already declared. The scope is same with public area.
>};
>```

>[!note] Additional note:
>There is not global function in programming languages like C# and Java. That means there are only the member functions in these languages. The functions are called methods in general term in these languages. But, global function are exist and frequently used in C++. There are a lot of global function usage in standard library. Global functions and class's member function serve in cooperation most of the time. So, when we say public interface, we mean the member functions and global functions that came with class's header file.
>
><mark style="background: #FFB86CA6;">Why we use global functions instead of member functions some times? </mark>
>Short answer is: Being member function of some functions has disadvantages or not compatible with language's other tools in some causes that related to language's syntax. 

## Member Functions
<mark style="background: #FFB86CA6;">A member function is a function that takes a hidden parameter that the address of a class's object and access the class's members via that address. </mark>A member function can access all the class's members.

<mark style="background: #FFB86CA6;">There are two kind of member functions</mark>:
1. <mark style="background: #BBFABBA6;">non-static member function</mark>
2. <mark style="background: #BBFABBA6;">static member function</mark>

>[!example] Syntax Exmaple: static and non-static member functions
>```cpp
>class Myclass{
>public:
>	void func(); // a non-static member function
>	static void foo(); // a static member function
>};
>```

<mark style="background: #FFB86CA6;">Non-static member function has a hidden pointer parameter to the class itself</mark>. That means if a non-static function has one parameter in visual, actually has two parameter with that hidden parameter. When we call the member function with dot '.' operator we pass the objects address to the member function as the hidden parameter. This is similar to following syntax in C:

```cpp
class Myclass{
public:
	void func(); // a non-static member function
	void foo(); // a static member function
};

int main()
{
	Myclass m;

	m.func(10); // func is a qualified name
	// is similar to following syntax in C:
	func(&m, 10);
}
```

<mark style="background: #FFB86CA6;">But this is not the only difference. There are scope difference too</mark>. The member function is searched in class scope.

<mark style="background: #FFB86CA6;">There is third difference, that is access control</mark>. A member function can access the private members of the class, but global functions can't. This doesn't mean the member function is only access the called object's private members. <mark style="background: #FFB86CA6;">A member function can access all object's private members</mark>. For example it can access a global class object's private members. For example:

```cpp
class Myclass{
public:
	void func(int); // a non-static member function
	void foo(int, int); // a static member function
private:
	int mx, my;
};

Myclass gn;

int Myclass::func(int x)
{
	gm.gx = 120; // This is not a syntax error.
}
```


### Definition of member function
The definition of the class is wrote in header file as general approach. The member functions and global functions are wrote in code file as general approach. Are all class definitions wrote in header file? No, if the class is not a public interface, it is wrote in code file.

>[!important] 
>The name is searched in following order if we use a unqualified name in a member function:
>1. block that the name is used (actually the from the start of block and the point that name is used)
>2. class scope
>3. namespace scope

```cpp
#include "myclass.h"

int mx = 45;

void Myclass::func(int ×)
{
	int mx = 5;
	mx = x; // First look at local scope, than look at class scope, than look at namespace scope
	::mx = x; // Direcly look at namepace scope
	Myclass::mx = x; // Direcly look at class scope
}
```
The name is first searched in local scope, than searched in class scope, than searched in namespace scope with unqualified name.
The name is searched in namespace scope with unary scope resolution operator.
The name is searched in class scope with binary scope resolution operator.

>[!tip] Tip: "this" usage
>We must understand the name lookup and we shouldn't use "this" keyword to specify the accessed member is the member that the call is performed. Because this makes the syntax harder to read. 
>If we use an unqualified name in a member function (we mean non-static member function when we say "member function"), the compiler searches in local scope first, that searches class scope if it not found the name in local scope. After the compiler founds the name in the class scope and the object is non-static object, the compiler uses that. So, there is no need to use "this" pointer to mention called objects members.

>[!tip] Tip: Function overloading with member functions
>Member functions can be overloaded. And don't forget all member functions is in class scope.

>[!warning] Warning: There is no function redeclaration for the member functions

>[!warning] Warning: Access controls is done last
>Let's look a member function overload:
>```cpp
>class Myclass{
>private:
>	void func(int);
>public:
>	void func(double);
>};
>
>int main()
>{
>	Myclass m;
>	
>	m.func(12); // syntax ERROR.
>}
>```
>This example has a syntax error. The access control is done lastly so, the compiler first chose the private function as the result of the function overload resolution then makes the access control and throws the error.

>[!example]
>```cpp
>class Myclass{ 
>private:
>public: 
>	void func(double); 
>	void foo(double); 
>}; 
>
>void foo(int)
>{
>
>}
>
>int Myclass::func(double d)
>{ 
>	foo(12);
>}
>```
>the compiler first searches the name in the class scope and find the non-static foo() function declaration in the class scope. 
>
  Keep caution in followings on this example:
>  1. There isn't a function overloading in this example. On of the rule for function overloading was that the function must be in same scope. The class scope and namespace (global in this case) scope are different.
> 2. The name lookup. The function foo is searched in class scope first. We can call the global function like this: <mark style="background: #D2B3FFA6;">::foo();</mark>.
>  
>  <mark style="background: #FFB86CA6;">Do not forget</mark>:
>  <mark style="background: #FFB86CA6;">And in upper example foo is called for the object that which object func is called.</mark>

>[!example] Syntax Example:
>Below syntax is legal though it is unnecessary until we see inheritances.
>```cpp
>m.Myclass::func(1.2); // This is valid
>```

## "this" pointer
'this' is a keyword in C++. Some other programming languages uses 'self' keyword instead if 'this' keyword. <mark style="background: #FFB86CA6;">We can use 'this' keyword only in a non-static member function. Usage of 'this' keyword in a global function or a static member function is a syntax error</mark>. 'this' keyword allow us to use the hidden pointer variable of the non-static member function. <mark style="background: #FFB86CA6;">'this' is address of the object that the function is called for. 'this' represent the address of the object</mark>. In some other programming languages, 'self' don't represent the address, directly represents the object. If we need such kind of approach we can use \*this. 
<mark style="background: #FFB86CA6;">'this' is a PR value expression, not a L value expression</mark>. 

>[!example] Syntax Example: 'this'
>```cpp
>class Myclass{ 
>private:
>	int mx;
>	void foo();
>public: 
>	void func(int); 
>}; 
>
>int Myclass::func(int x)
>{ 
>	mx = x;
>	this->mx = x;
>	(*this).mx = x;
>	Myclass::mx = x;
>}
>```
>All four example in upper code is same. But if there is a local variable called mx inside func() only the unqualified mx means the local mx.

> [!tip] Tip: Naming convention to members
> Indication the being member with 'this' keyword is a wrong approach.
> There are two too widespread naming convention for member naming:
> 1. usage of 'm_' prefix
> 2. usage of '_' postfix

<mark style="background: #FFB86CA6;">Why is there 'this' pointer? </mark>
1. <mark style="background: #FFB86CA6;">Let's think about that a member function calls a global function and passes own object's address as a parameter</mark>.
```cpp
class Myclass{ 
private:
public: 
	void func(int); 
}; 

void f(Myclass*);
void g(Myclass&);

int Myclass::func(int x)
{ 
	f(this);
	g(*this); // 'this' reference usage
}

int main()
{
	Myclass m;
	m.func(12);
}
```
2. <mark style="background: #FFB86CA6;">The return of a member function is the object itself</mark>. This used in C# and Java too. And it provides some advantages. This is called <mark style="background: #BBFABBA6;">fluent API</mark> in general, <mark style="background: #BBFABBA6;">chaining</mark> in C++.
> [!info] Additional Info: Fluent API - Chaining
> To make possible the syntax <mark style="background: #D2B3FFA6;">x.foo().func().f();</mark> same as:
> x.foo();
> x.func();
> x.f();
> The question is that how the x.func(); corresponds to he object itself? The answer the return of the function must be a L value reference to the object.
>
>Example Syntax:
>```cpp
>class Myclass{ 
>private:
>public: 
>	Myclass& foo();
>	Myclass& func();
>	Myclass& bar();
>}; 
>
>Myclass& Myclass::foo()
>{
>
>	return (*this);
>}
>
>int main()
>{
>	Myclass m;
>	m.bar().foo().func();
>}
>```
>First function bar is called, then function foo is called, then function func is called.
>If we use pointer instead of reference we would make the chain call with -> operator.
3. There is a same name object in local scope and we want to access object in the class scope. We can use 'this' or resolution operator for do this.

>[!note] Important Note:
><mark style="background: #FFB86CA6;">'this' is a PR value expression. So we cannot assign it a another object's address</mark>. The following is a syntax error:
>```cpp
>int main() 
>{ 
>	Myclass m;
>	this = &m; // Syntax ERROR, this is a PR value expression
>}
>```
>
><mark style="background: #FFB86CA6;">But the following syntax is legal</mark>:
>```cpp
>{ 
>	Myclass m;
>	*this = m; // OK
>}
>```
><mark style="background: #FFB86CA6;">If *this is be a const object, then that would be illegal. We will look through that.</mark>


---

# Summary
>[!summary] Section Summary:
>1. Member functions can be overloaded. (In class scope)
>2. A member function cannot be redeclared.
>3.  Overload between a member function and a global function is not possible
>4. Access control is always done at last
>5. This pointer
>6. *this
>8. Fluent API - Chaining

>[!info] Ending Info: Chaining and operator overloading
>We use fluent API/chaining before with syntax std::cout << x << d;.
>**Operator overloading**: the compiler converts the object expression that is operand of an operator to a function call if a convenient function is declared when class object be a operators operand. The functions that called in this situation (The situation that an object is an operand of an operator) are called <mark style="background: #BBFABBA6;">operator functions</mark>. This functions can be a global function or a member function.
>The syntax is like following:
>```cpp
>m1 + m2
>m1.operator+(m2)
>
>std::cout << x << d;
>std::cout.operator<<(x).operator<<(d);
>```

---
Return: [[00_Course_Files]]