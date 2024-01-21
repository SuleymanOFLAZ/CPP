[[00_Course_Files]]
#Cpp_Course 
A class is a definition that describes how can we use that type. We create objects that suits that definition. Instantiate means creating objects that suits that definition. The object that consisted in end of this process is an instance.
So we can consider following expressions are same: class object, class variable, class instance.

# const member functions
const keyword is one of the most important keyword of the C++ programming language. One of the quality measurement of a programs is the usage of the const keyword in places that needed a const keyword. This is described with a term, that is <mark style="background: #BBFABBA6;">const correctness</mark>.

>[!term] Term: Const Correctness
>Const correctness means that the object that need to be const must be const.

The presences are divided in to two categories depending on the changeable. These categories are:
1. <mark style="background: #BBFABBA6;">Mutable</mark>: Variables that their values can change
2. <mark style="background: #BBFABBA6;">Immutable</mark>: Variables that their values cannot be changed.

There are a lot of benefit of using immutable objects such as the compiler can make better optimization, there isn't a synchronization need in parallel programming.

Do a non-static member function change the object? How would we know this? Because the first parameter (an address to object itself) is a hidden parameter. Ho can i know a non-static member function is a accessor or a mutator? <mark style="background: #FFB86CA6;">We declare a member function that the hidden parameter of the function is a const with a const keyword after the function parentheses</mark>. We call that functions as <mark style="background: #BBFABBA6;">const member functions</mark>, and we call the function that declared without const keyword as <mark style="background: #BBFABBA6;">non-const member function</mark>.

>[!example] Syntax Example: Const member function
>```cpp
>class Myclass{
>public:
>	void func(int, int); // non-const member function
>	void foo(int) const; // const member function
>}
>``` 

<mark style="background: #FFB86CA6;">We must ask that question first is this member function is a accessor or mutator when we are creation a public interface of a class</mark>. If the member function don't created for changing the object than we must declare it with const keyword.

> [!attention] Attention
>The const keyword of a member function is actually qualifying the hidden object address parameter. For example:
>```cpp
>void foo(int) const;
>// Means
>void foo(const Myclass*, int);
>```
><mark style="background: #FFB86CA6;">So, the const qualifier is only for the object that called.</mark>

So a non-static member function can be either a const member function or a non-const member function.

The compiler throws syntax error, if we declare a const member function but define a non-const member function, or vice versa. That means a const member function and a non-const member function are totally different from each other. <mark style="background: #FFB86CA6;">A const member function cannot change any class non-static data member. This would be a syntax error</mark>.

>[!caution] Caution: const only qualifies the object itself.
>The const keyword in a member function declaration is only qualifies the called object address. So following syntax would be legal:
>```cpp
>void Myclass::foo() const
>{
>	Myclass m;
>	m.mx = 20; // OK, another object's mx, not the object's mx that passed as hidden function parameter.
>}
>```

>[!caution] Caution: a cont member function shouldn't call a non-const member function
> -> <mark style="background: #FFB86CA6;">A non-const member function of the class can invoke a const member function of the class</mark>.
> -> <mark style="background: #FFB86CA6;">A const member function of the class cannot invoke a non-const member function of the class</mark>.
> This is because there isn't implicit type conversion from const T\* to T\* in C++. Let's explain. First look the syntax of how a member function calls another member function of the same class.
> ```cpp
> class Myclass {
> public:
> 	void foo() const;
> 	void func();
> };
>
>void Myclass::func()
>{
>	foo(); // This is OK. There is implicit type conversion from T* to const T*
>}
>
>void Myclass::foo()
>{
>	func(); // Syntax ERROR. There is no implicit type conversion from const T* to T*
>}
>```
>To understand that we need to look verbose syntax (C style) of the member function call. The compiler searches the function name in case of a function call in according to rules that we mentioned. When it find the function as non-static member function than calls that function with hidden pointer of itself. That a non-static member function calls another non-static member function for the object (hidden object address) that which itself called for.
>```cpp
>void foo(const Myclass*);
>
>void func(Myclass* p)
>{
>	foo(p);
>}
>```

>[!Caution] Caution: A non-const member function shouldn't called for a const object
>What if we call a non-const member function for a const object. This would be a syntax error.
>```cpp
> class Myclass {
> public:
> 	void accessor() const;
> 	void setter();
> };
>
>void main()
>{
>	const Myclass cm;
>	cm.setter(); // Syntax ERROR. There isn't implicit type conversion from const T* to T*
>}
>```
>But of course if we call a const member function for non-const object that wouldn't be a syntax error. Because there is implicit type conversion from T\* to const T\*.

 We cannot call a non-const member function for a const class object. Of course this is general approach. We explained the base of the idea.

>[!Caution] Caution: Chaining / Fluent API with a const member function
>Keep caution on that 'this' pointer is a low level const pointer if the non-static member function is a const function, and vice versa. In chaining syntax we would return '\*this' as reference syntax or 'this' as pointer syntax. So if the function is a const function, the return value of the function should be type of const of he object's address or reference. This is because, there is no implicit type conversion from const T\* yo T\*.

>[!caution] Caution
>Following syntax is a const overloading.
>```cpp
>class Myclass {
>public:
>	void func()const;
>	void func();
>};
>```
>This syntax is widely used and used in standard library too. If the object is const than the only viable function is cost function. If the object not const than two functions is viable but non-const is called. If the non-const function not exist than the const function is called with a non-const object.

>[!example] Example: Const overloading
> A const overloading example: Vector's front and back functions are overloaded. So when we call for a const object that the syntax error is occur. Because if we call the function for a const object than the function with const return reference is called and assigning a const reference is syntax error. If we call the function for a non-const object than the non-const function is called and that is OK.

```cpp
#include <vecto>
int main
{
	const std::vector<int> vec{ 1, 2, 3, 5, 7 };
	
	vec.front () = 99;
	vec.back() = 333;
	
	for (auto val : vec)
		std::cout <<val<<" ";
}

// The corresponding syntax is 
class vector{
public:
	int &front();
	const int &front() const;
}
```

## The mutable keyword (data member usage)
There are member functions that they don't change the class object's value or state at he problem domain. For this reason, there must be const member functions in semantic way.
What is the problem domain? If we add a variable that counts member functions call count, would it be in the problem domain of the object? Of course no, because the value is only for debug purposes and doesn't effect the problem domain of the class object. So, how we change the value of that variable in a const member function? We were have been did it with type cast previously until a new keyword added to the language. This keyword added because there is a inconsistency between the syntax if the language and the semantic of the language. This keyword is '<mark style="background: #BBFABBA6;">mutable</mark>'.We use mutable keyword on a data member to say the compiler that changing the data member inside a const member function doesn't effect the class object's problem domain.

Mutable keyword has different meaning depending in the situation. Se mentioned usage on a data member. 

>[!question] Interview Question
>**Question**: What is the meaning of the usage of mutable keyword on a class data member?
>
>**Answer**: We say to the compiler that changing that object value doesn't effect the class's value that in the problem domain.

>[!Example] Syntax Example
>Mutable keyword example (usage on a data member).
>```cpp
>class Myclass {
>public:
>	void func()const;
>private:
>	mutable int debug_call_count = 0;
>};
>
>void Myclass::func()const
>{
>	++debug_call_count; // It is OK since the data member is mutable
>}
>```
# ODR

>[!note] Acronym
>C++ has wide acronym library. <mark style="background: #BBFABBA6;">Acronyms</mark> are abbreviations that created with first letter of the expressions.

>[!Example] Some acronym example for C++ 
AAA: Almost Always Auto. Describes a coding style
ADL: Argument Depended Lookup
EBO: Empty Base Optimization
SFINAE: Substitution Failure Is Not An Error

ODR means "<mark style="background: #BBFABBA6;">One Definition Rule</mark>". <mark style="background: #FFB86CA6;">Defining an presence more than one causes a syntax error or a undefined behavior depending in the situation. So ODR means the rule that one definition for each presence</mark>.

So, in order to ensure ODR:
We must define an presence more than one.
If two same definition is exist in a file the compiler gives syntax error
If two same definition is exist in two different file the linker gives error. But it is possible to pass that state without an error and that would be undefined behavior.
We cannot give definition on a header file because every include of it means a definition.

But there is some exceptions for the ODR rule according to the language rules. And this exceptions are used frequently.
There is some presences that if the definitions of it are same <mark style="background: #BBFABBA6;">token-by-token</mark> there isn't a undefined behavior even they are defined in multiple modules in a project.

So there are situations that defining more than one is not breaks the ODR rule. But there are some conditions for that:
1. Defining more than one cannot be in the same source file
2. The presences that defined in different source files must be same token-by-token

So that presences can defined in header files. Because when we include the header, the definition of that object is same token-by-token at every place.

>[!info]  Info: The presences that can be defined in header files
>So these presences are:
>1. class definitions
>2. inline functions
>3. inline variables (C++17)
>4. constexpr functions (implicitly inline)
>5. template codes (all type of templates. class templates, function templates, variable templates, type templates etc.)

# inline expansion && inline functions

C and C++ compilers are optimizing compilers. These compilers has a optimizer module. 
Inline expansion is a optimizing method.
The compiler writes a reference for linker and that called external reference. For example a function reference.
The compiler can directly use the function instead of giving reference to the linker if the compiler knows the function's definition if case of a function call. These techniques generally are called inline expansion.

So the questions for inline expansion are:
1. can it done?
2. is there a benefit?

The compiler must see the function's definition. The compiler must has capability to do that. Some compilers can do that and some can't. The compilers has switches, so the compiler has different switches for inline expansion. For example always inline expansion switch that means use always inline expansion if it possible. Or a never inline switch.

The benefit of a inline expansion can differ depending on situation.
Getting benefit of inlining can be possible with the functions that have few statements and called frequently.

We can use <mark style="background: #BBFABBA6;">inline</mark> keyword to defined inline functions. We tell the compiler that do inline expansion with that function it it is possible. The compile can inline expand on function that is not stated with inline keyword, or cannot inline expand on function that is stated with inline keyword. So what is the meaning of using inline keyword if the compiler doesn't care about it.

<mark style="background: #FFB86CA6;">But the important point of using the inline keyword is we create a function that don't break the ODR</mark>. So a inline function definition is count as one regardless of how many times is it defined in source files. <mark style="background: #FFB86CA6;">That means we can write inline function definitions in the header files.</mark> 

<mark style="background: #FFB86CA6;">One of the most applicant for inline functions is that member functions of the classes that has few process like a simple get function</mark>. So it is important to make that member functions inline. But the compiler must see the definition of that function. To ensure that 

We can use inline keyword in definition or declaration or both of them to make the function inline. But if there is not inline qualification on any of them, this is function is not inline and cannot be used in header files.

<mark style="background: #FFB86CA6;">We can apply same rules to the global functions too</mark>. That mean we can define a global function with inline keyword inside a header file.

<mark style="background: #FFB86CA6;">constexpr functions and function templates are implicitly inline</mark>. So we can use them in a header file without inline keyword. We can also use inline keyword with constexpr functions. That don't make change in ODR rule of constexpr function because implicitly inline. But using inline keyword with constexpr function makes sense in that perspective we say the compile to inline that function. Since there is no guarantee for that, this is not clearly meaningful but has a little sense inside them, since a constexpr function can be called with non-constexpr parameters a like a regular function.

<mark style="background: #FFB86CA6;">There are two way to define a member function inline</mark>:
1. Definition of function can be given in header file at some place outside of the class definition with inline keyword
2. writing the function definition inside the class definition. Even we don't write the inline keyword the function is inline in with this syntax. We can use the inline keyword but this is optional since the function is inline already. This is more clear syntax.

<mark style="background: #FFB86CA6;">Some important metters</mark>:
1. If we don't provide inline keyword, we close the inline expansion possibility of that function to other modules(files) because they are not know the functions definition. Other way, the compiler has possibility to do the inlining if it considers it meaningful.
2. Inline functions leaks the implementation details. Because they are in header files.
3. we include another header file if the inlined function uses presences from an external files. And that increases the dependency,

There are term called <mark style="background: #BBFABBA6;">header only library</mark>. That means there is not source file that compiled. If all functions are inlined, that is possible and these are design decisions.

<mark style="background: #FFB86CA6;">Which functions can be defined as inline</mark>:
1. non-static member functions
2. static member functions
3. global functions
4. friend functions

# constructor and destructor 

There are two important member functions inside the member functions of class. These are constructor and destructor.

Creating a class object is done with a function call to a function of the class, and this is one of the most important rule of the C++. This function is a member function and called <mark style="background: #BBFABBA6;">constructor.</mark> The constructor makes the class object usable. <mark style="background: #FFB86CA6;">Constructor is a must, that means we cannot create a class object without the constructor</mark>. Constructor is a non-static member function of the class but it has a special status.

Destructor is called when the object is destroyed. This is a special non-static member function too.

There are some special rules for constructor:
1. We cannot chose the name of the constructor and name of the constructor must be same as the class name
2. There is not return concept of the constructor. That doesn't mean there is not return of the constructor. non-existence of return value and non-existence of return concept are two different things
3. constructor must be a non-static member function of the class. (There are static constructor concept in some other programming languages, there is not in C++)
4.  Constructor cannot be a const member function. 
5. Constructor cannot be called like other member functions (cannot be called with dot operator or arrow operator)

Access control is legal for constructor. That means a constructor can be private, public or protected. But making constructor private causes the syntax error in case of creating an object. Is there situations that we prefer a private constructor? There is, we can prefer private constructor in case of some design patterns. 
```cpp
class Myclass{
	Myclass(int); // Private constructor

	void foo()
	{
		Myclass m(12); // OK, a member function can access private members
	}

};

int main()
{
	Myclass m(12); // Syntax ERROR, the constructor is private
}
```

A class has a storage in memory. We must set that memory before clients use the class object. So constructor make a class object usable. constructor initializes the non-static member functions or does other initializations(can be creating a log file, connecting to a database, set operation to a serial port). 

A constructor can be more that one and can be overloaded. Function overload rules are legal in here.


> Summary
> 1. Constructor initializes the class object and makes it usable for clients
> 2. Constructor must be non-static and non-const
> 3. Constructor has't a return concept
> 4. Constructor must has the name name with class
> 5. Constructor can be a member or public, protected, or private
> 6. Constructor can be overloaded

## Default Constructor
Constructors has a special overload that called default constructor. A default constructor is a constructor that there ins't a parameter or all parameter's have default argument. That means a default constructor is called without arguments.

```cpp
class  Myclass{
	Myclass(); // Default constructor, no parameter
	// OR
	Myclass(int=10); // Default constructor, all parameter have default
}
```

>[!tip] Tip: Abbreviation
>ctor: means constructor
>dtor: means destructor

# destructor


# Special member functions of classes

---
Return: [[00_Course_Files]]