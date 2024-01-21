[[00_Course_Files]]
#Cpp_Course 
# Special Member Functions
## Constructor

Look to [[RAII (Resource Acquisition Is Initialization)]]

Constructor and destructor are a non-static member functions. These functions can't be const.

>[!info] Info
>ctor is used as abbreviation to constructor
>dtor is used as abbreviation to destructor
>

Some specialities of the constructor:
- Ctor's name cannot be chosen and must be same as the class's name.
- Ctor have no return concept. That doesn't mean it have no a return value.
- Ctor can be overloaded.
- Ctor can take a default argument.
- Ctor doesn't have to be public, and can be private. But that means it get struct in access control when we try to create an object. There are some special use cases of it, we will mention it later

### Default Constructor
The constructor that has no parameter or all of it's parameters take a default value is called as default constructor.

#### How we should initialize the object to make a call to the default ctor?
There are different kind of initialization in C++, differently from C.

1. With default Initialization

> [!info] Info: Default Initialization
> Syntax: int x; Myclass m:
> Variables that are primitive types and has auto lifetime are initialized with undetermined value, if they are initialized with the default initialization.
> Variables that has static lifetime are initialized with zero initialization before their initialization.
> References cannot be default initialized.
> Top level const variables cannot be default initialized.

Default initialize of an class object means initializing it with default constructor.

2. With value initialization

> [!info] Info: Value Initialize
> Syntax: Myclass {};
> It is the value initialize if the inside of the braces are empty. It is different scenario the inside the braces is not empty. But if it is empty the default constructor is called.

> [!warning] Warning
> There is t


## Destructor
Specialities of the destructor:
- Cannot be overloaded. A class can have only one destructor.
- Cannot have parameter.
- Has no return concept. That doesn't means it is not has a return value.
- Can be public, private or protected.
- Can be revoked by it's name. This is helpful in very specific cases. (Ctor cannot be revoked by it's name.)
- Must be a non-static and non-const member function.

>[!important] Important: When a global class object created and destroyed?
>A global variable (that has static lifetime) is created before the main() function and destroyed after the main function. Therefore, ctor of an global object is called before the main() function and dtor of it is called after main() function.

> [!important] Important: Order of revoking the ctor and dtor of multiple global class objects
> Global variables that are in the same source file is created in order of the their declaration, and destroyed the reverse order of their creation order. So the order of the call to the ctor and the dtor is same as that.

> [!info] Info: <mark style="background: #FFB86CA6;">Static Initialization Problem</mark>
> But if the objects are in at different source files, the creation order of they is not guaranteed. So if these variables connected to each other, fault occurs. (C and C++)

> [!important] Important: When a static local object is created?
> Static local variables are created one time at the first function call and destroyed at the end of the program. So  ctor of a static local class object is called at the first call to the function and dtor is called at the end of the program. This rule is guarantee with the language rules.

> [!important] Important: Lifetime of the local variables
> The ctor of the local object is called with the enter the function scope and dtor is called when the exit the function. This is also valid in loop scope (ex. for scope).

> [!question] Question (Interview)
> Print from 1 to 100 without using a loop.
> One of the answer: declare a 100-size array of an class object and count and print in the ctor.

---
Return: [[00_Course_Files]]