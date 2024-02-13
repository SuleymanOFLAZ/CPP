[[00_Course_Files]]
#Cpp_Course 
The existence that is created with using a class definition is called "**object**" or "**instance**". Creating act that object is called "**instantiate**".

A class is a definition that describes how can we use that type. We create objects that suits class definition. Instantiate means creating objects that suits that definition. The object that consisted in end of this process is an instance.

So we can consider following expressions are same: class object, class variable, class instance.
# const member functions
#Cpp_constMemberFunctions
const keyword is one of the most important keyword of the C++ programming language. One of the quality measurement of a program is the usage of the const keyword in places that needed a const keyword. This is described with  "**const correctness**" term.

> [!note] Note:  Const Correctness
>Const correctness means that the object that needs to be const must be const.

The objects are divided in to two categories depending on the changeable. These categories are:
1. **Mutable**: Variables that their values can be changed.
2. **Immutable**: Variables that their values cannot be changed.

> [!note] Note: Advantages of using immutable objects
> There are several advantages of using immutable objects. Some of these:
> - Compiler can do better optimization
> - No need to synchronization in parallel programming

---
Can a non-static member function change the object? How would we know this? Remember the first parameter (an address to object itself) is a hidden parameter. Ho can we know a non-static member function is a accessor or a mutator? We declare a member function that the hidden parameter of the function is a const with a const keyword after the function parentheses. We call that function as **const member function**, and we call the function that declared without const keyword as **non-const member function**.

```cpp
class Myclass{
public:
	void func(int, int); // non-const member function
	void foo(int) const; // const member function
};
``` 

> [!note] Note: The First thing we should pat attention when we are creating a public inter face
We must ask that question first is this member function is a accessor or mutator when we are creation a public interface of a class. If the member function don't created for changing the object than we must declare it with const keyword.

> [!note] Note: 
>The const keyword of a member function is actually qualifying the hidden object address parameter. For example:
```cpp
class Myclass{
	public:
	void foo(int) const;
	// Means
	void foo(const Myclass*, int); // This is a representation, "const Myclass*" is the hidden parameter.
};
```

A non-static member function can be either a const member function or a non-const member function.

Compiler controls for const member functions

> [!warning]  Warning: The declaration and the definition of a const member function must contain the "const" keyword
The compiler throws syntax error, if we declare a const member function but we give a non-const member function for its definition, or vice versa. That means a const member function and a non-const member function are totally different from each other. 

```cpp
// Myclass.h
class Myclass{
	public:
	void foo(int) const;
};

// Myclass.cpp
void Myclass::foo(int) // Syntax ERROR. Because the "foo" declared as const.
{

}
```

> [!warning]  Warning:  A const member function cannot change a non-static data member of the class.
> A const member function cannot change any class non-static data member. If the function tries to change, this would be a syntax error.

```cpp
// Myclass.h
class Myclass{
public:
	void foo(int) const;
private:
	int mx;
};

// Myclass.cpp
void Myclass::foo(int) const // Syntax ERROR. Because the "foo" declared as const.
{
	mx = 45; // Syntax ERROR.
	this->mx = 45; // Same as this syntax. We cannot change the data of low-level const pointer.

}
```

> [!warning]  Warning: const only qualifies the object itself.
>The const keyword in a member function declaration is only qualifies the called object address. So following syntax would be legal:
```cpp
// Myclass.h
class Myclass{
public:
	void foo(int) const;
private:
	int mx;
};

// Myclass.cpp
void Myclass::foo() const
{
	Myclass m;
	m.mx = 20; // OK. "m" is a different object. "const" only qualifies the "this" pointer.
}
```

> [!warning]  Warning:  A const member function shouldn't call a non-const member function
> -> A non-const member function of the class can invoke a const member function of the class.
> -> A const member function of the class cannot invoke a non-const member function of the class.
> This is because there isn't implicit type conversion from "const T\*" to "T\*". Let's explain. First look the syntax of how a member function calls another member function of the same class.
```cpp
// Myclass.h
class Myclass {
public:
	void foo() const;
	void func();
};

// Myclass.cpp
void Myclass::func()
{
	foo(); // This is OK. There is implicit type conversion from "T*" to "const T*".
}

void Myclass::foo()
{
	func(); // Syntax ERROR. There is no implicit type conversion from "const T*" to "T*"
}
```
To understand that we need to look verbose syntax (C style) of the member function call. The compiler searches the function name in case of a function call in according to rules that we mentioned. When it find the function as non-static member function than calls that function with hidden pointer of itself. That a non-static member function calls another non-static member function for the object (hidden object address) that which itself called for.
```cpp
void foo(const Myclass*);

void func(Myclass* p)
{
	foo(p); // OK. But vice versa would be the syntax error. 
}
```

> [!warning]  Warning: A non-const member function shouldn't called for a const object
>What if we call a non-const member function for a const object. This would be a syntax error.

```cpp
 class Myclass {
 public:
 	void accessor() const;
 	void setter();
 };

void main()
{
	const Myclass cm;
	cm.setter(); // Syntax ERROR. There isn't implicit type conversion from "const T*" to "T*".
}
```
But of course if we call a const member function for non-const object that wouldn't be a syntax error. Because there is implicit type conversion from "T\*" to "const T\*".

> [!warning]  Warning: Chaining / Fluent API with a const member function
>'this' pointer is a low level const pointer if the non-static member function is a const function, and vice versa. In chaining syntax, we would return '\*this' as reference syntax or 'this' as pointer syntax. So, if the function is a const function, the return value of the function should be type of const object address or reference. This is because, there is no implicit type conversion from 'const T\*' to 'T\*'.

```cpp
// Myclass.h
class Myclass {
public:
	Myclass& foo() const;
};

// Myclass.cpp
Myclass& Myclass::func()
{
	return *this; // Syntax ERROR. 'this' is a low-level const poiner. So, there isn't implicit type conversion from "const T*" to "T*".
}
```
This syntax error situation can be turned into a valid situation with followings:
- Both the function and return type can be const 
- Both the function and return type can be non-const.
The situation is same in pointer syntax.

> [!warning]  Warning: A const member function can be a const overload
>Following syntax is a const overloading.  This syntax is widely used and used in standard library, too. If the object is const than the only viable function is cost function. If the object not const than two functions is viable but non-const is called. If the non-const function not exist than the const function is called with a non-const object.
>Functions are chosen depending on the constness of the object that overloaded function is called for.
```cpp
class Myclass {
public:
	void func()const;
	void func();
};

int main()
{
	Myclass m;
	m.func(); // non-const member function is called.

	Myclass cm;
	cm.func(); // const member function is called.
}
```

>[!example] Example: Const member function overloading in standard library
>'front' and 'back' functions of the vector class are overloaded. If we call these functions with a const vector object, this wold be an error. But if we call these functions with a non-const vector object, this would be ok. But how the compiler knows we called these functions with a const object and throw an error? Because these functions have const overloading. If we call the function 'front' with a const object, the overload with const return type is chosen. So, the function returned a const object and we use it in left side of the assignment operator. That causes the error.

```cpp
#include <vecto>
int main
{
	std::vector<int> vec{ 1, 2, 3, 5, 7 };
	const std::vector<int> vec2{ 1, 2, 3, 5, 7 };

	vec.front() = 99; // OK. The object is a non-const
	vec.back() = 333; // OK. The object is a non-const

	vec2.front() = 99; // Syntax ERROR. The object is a const
	vec2.back() = 333; // Syntax ERROR. The object is a const
	
	for (auto val : vec)
		std::cout <<val<<" ";
}

// The corresponding syntax is 
class vector{
public:
	int &front();
	const int &front() const;
};
```

# The mutable keyword (data member usage)
#Cpp_mutable
There are such member functions that these functions do not change the perceived value/status of the class object in the problem domain. Therefore, from a semantic perspective, it must be a const member function.

Let's assume, we have a data member that represents the call count of the member functions for debug purposes. Is the change in the value of the debug object related to the meaning of the class object in the problem domain? The answer is no. But if we try to change the value of debug object in the const member function that would be syntax error. Here is a conflict between the syntax rules of the language and the semantic side. The debug variable can be changed according to semantic side of the language because it is not related to value of object that in problem domain. But the syntax of the language prohibits this.

```cpp
class Myclass { 
public: 
	void func()const; 
private:
	int debug_call_count = 0; 
};

void Myclass::func()const
{
	++debug_call_count; // Syntax error. A const member function can't change it.
}
```

We had to do some tricks to reconcile syntax and semantics in early times of C++ (before the standards). For example, we were doing type conversion. We need to change the type of 'this' pointer to non-const in this example.

```cpp
void Myclass::func()const
{
	++const_cast<Myclass *>(this)->debug_call_count; // In early C++
}
```

But we never do it like that. There is a keyword used for this purpose. This keyword is '**mutable**'.  This keyword indicates the value of the variable is not related to value of class object. This indication is for compiler and the code reader. A mutable variable can be changed by non-const member functions.

```cpp
class Myclass { 
public: 
	void func()const; 
private:
	mutable int debug_call_count = 0; 
};

void Myclass::func()const
{
	++debug_call_count; // OK. The variable is mutable.
}
```

> [!question] Question: What is meaning of 'mutable' keyword when we use it for a class data member?
> A: We declare to the compiler and the reader that, this variable member of the class is not related to value of the class (value in the problem domain). So, the compiler don't give syntax error in case of value change of this variable in const member function.
> #Cpp_Interview_Question 

> [!warning]  Warning: Mutable is a overloaded keyword
> Mutable is overloaded keyword. There is another use case of 'mutable' keyword, too. But we will mention it later.


# ODR - One Definition Rule
#Cpp_ODR

ODR is an acronym. ODR means "**One Definition Rule**". Defining an presence more than one causes a syntax error or a undefined behavior depending in the situation.  ODR means one definition for each presence.

"Declaration" and "definition" are different concepts in C and C++. Declaration can be made more than once, but the definition has to be unique (For each presence). But if we define a presence more than once, it will be a syntax error or undefined behavior depending on the usage. So breaking the one definition rule is causes a syntax error or a undefined behavior.

```cpp
class Myclass {
	// ...
};

class Myclass { // Syntax ERROR. Same definiton of one object
	// ...
};
```

If we define a presence more than one time in a source file (means same scope - global scope), it will syntax error. The compiler has to see the breaking the one definition rule to give a syntax error.

But if we define a presence two times in different source files, it will not a syntax error. Because the compiler compiles each source files separately. But we break the one definition rule. This is undefined behavior.

**Consequences of breaking one definition rule:**
1. Syntax error (If definitions are in same source files)
2. Undefined behavior (If definitions are in separate source files)

> [!note] Note: ODR and static
> If we define the object as static, it won't lead undefined behavior

> [!warning]  Warning: Don't define a function in a header file
> Don't define a function in header file, it leads to function definition in each source file that included the header. It will leads to undefined behavior. The linker can give error or not depending on working environment, it is undefined behavior in every case.

**EXCEPTIONS TO ONE DEFINITION RULE**
The rules of the language says there are some exceptions of the one definition rule. And this exceptions are used frequently.

There are entities that, although defined in more than one module within the project, do not cause undefined behavior if their definitions are the same **token-by-token**. The linker is guaranteed to see at worst one of these definitions from the linking phase.

The rules for ODR exception:
1. The definitions have to be in different source files.
2. The definitions have to be same token-by token.

```cpp
// Myclass.cpp
class Myclass{
public:
	void func(int);
	void foo(int);
};

// main.cpp
class Myclass{ // There is no undefined behavior. The definitions are same token-by-token and in different files.
public:
	void func(int);
	void foo(int);
};
```

```cpp
// Myclass.cpp
class Myclass{
public:
	void func(int);
	void foo(int);
};

// main.cpp
class Myclass{ // UNDEFINED BEHAVIOR. Because the definitions are not same token-by-token
public:
	void func(int);
	void foo(int);
	void g(int);
};
```

 **CONCLUSION**
 If an entity has this status, defining these entities in the header file does not create undefined behavior. Because every source file include same header and same definition. That means every instance of the definition is same token-by-token in source files.
 
Due to this rule we get the right to define some entities in the header file.

**WHICH ENTITIES DO NOT CAUSE UNDEFINED BEHAVIOR IF WE DEFINE THEM INSIDE A HEADER FILE**
1. class definitions
2. inline function definitions
3. inline variables (since C++17)
4. constexpr functions (Because, implicitly inline)
5. template codes (All type of templates. class templates, function templates, variable templates, type templates etc.)

> [!warning]  Warning: What we cannot do due to ODR?
> - We cannot define a non-inline function in header file.
> - We cannot define classes that has same name but has different token syntax in different source files.

>[!note] Acronym
>C++ has wide acronym library. Acronyms are abbreviations that created with first letter of the expressions.
>Some C++ acronyms
> - AAA: Almost Always Auto. Describes a coding style
> - ADL: Argument Depended Lookup
> - EBO: Empty Base Optimization
> - SFINAE: Substitution Failure Is Not An Error

# Inline Expansion & Inline Functions

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
# Terms
- object
- instance
- instantiate
- const correctness
- mutable
- immutable
- const member function
- non-const member function
- mutable keyword
- ODR "One Definition Rule"
- token-by-token
- acronym
---
Return: [[00_Course_Files]]