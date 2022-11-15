# 1. Struct vs. Class

1.1. Struct: Public by default; Class: Private by default

1.2 Both struct and class can be used to define a type. Usually a class is used for defining a type, unless we want all variables to be accessed directly from outside. 

1.3 In C++, both struct and class can be used to define member variables and functions in them. Note that we can only define member variables but not member functions in C structs.

# 2. Declaration vs. Definition, of Member Functions vs. Non-member Functions

2.1 Declaration of a member function must be inside the class body. Definition of a member function may be either inside or outside the class body.

2.2 Definition <b>and</b> Declaration of a non-member function should be outside the class body.

# 3. this

3.1 Member function access the object on which it is called by an implicit "this" parameter. Calling "this" returns the <b>address</b> of the object itself.

3.2 "this" is implicitly defined. We cannot define "this" as a parameter or variable.

3.3 "this" is a const pointer, meaning we cannot change the address it points to.

# 4. Const Member Functions

4.1 Example: `std::string isbn() <b>const</b> {return this->bookNo;}`. The const here after parameter list is to <b>change the type of "this"</b>, so that "this" can be binded to a const object.

4.2 By default, the type of "this" is `ClassType *const`, so "this" cannot be binded to a <b>const object</b>. But if we make isbn a const member function, then:
`std::string isbn() const {return this->bookNo;}` is equivalent to `std::string isbn(const ClassType *const this) {return this->bookNo;}`. The const member function cannot change the object ("this") it is called on.

# 5. Class Scope and Member Functions

5.1 A class itself is a scope. A member function is defined in the scope of class it belongs to. 

5.2 Even if something A is defined in the class scope but <b>after</b> a member function B, the member function B can still access A because the member declarations are compiled at first.

5.3 Outside the class scope, members are accessed only through the forms of objects, references, and pointers.

5.4 We access type members using the scope operator. e.g. `Screen::pos ht = 24, wd = 80 // use the pos type defined by the Screen class`

5.5 WHen outside the class, return type must specify the class scope it belongs to.
    class Window_mgr {
        public:
            ScreenIndex addScreen(const Screen&);
    }

    Window_mgr::ScreenIndex; // return type must be seen before we are in the scope of Window_mgr
    Window_mgr::addScreen(const Screen &s)
    {
        screens.push_back(s);
        return screens.size() - 1;
    }


# 6. Defining a Member Function outside the Class

6.1 Example:
    double Sales_data::avg_price() const {
        if (units_sold)
            return revenue/units_sold;
        else
            return 0;
    }

After seeing <b>Sales_data::</b>, compiler will interpret the function in the scope of Sales_data class. The variables revenue and units_sold will implicitly refer to the member variables in Sales_data class.

# 7. Defining a Function to Return "this" Object

7.1 We don't need "this" to access a member variable, but we need it to access the object as a whole, like `return *this`.

# 8. Constructors

8.1 Constructors do not have a return type.

8.2 Constructors <b>cannot be defined as const</b>. This is because when we construct a const object, we still need to store values into it. A const object becomes "const" <b>after its initialization is finished.</b>

8.3 If we do not explicitly define constructor for our class, the <b>compiler</b> will implicitly define the default constructor, called synthesized default constructor. The synthesized default constructor will either use the in-class initializers or default initialize the member variables in the class.

8.4 Cases where synthesized default constructor cannot be used:
- If we define any constructor by ourselves.
- If class have built-in or compound type members (e.g. arrays or pointers), because their blocks might contain undefined values.
- If class does not have a default constructor

8.5 Define constructors:
- default constructor: do `className() = default;` in class declaration/definition
- constructor initializer list: 
    - e.g. `Sales_data(const std::string &s, unsigned n, double p): bookNo(s), units_sold(n), revenue(p*n) { }`
    - e.g. `Sales_data(const std::string &s): bookNo(s) { }`. In this case, members not specified are implicitly initialized by synthesized default constructor, with in-class initializers or are default initialized, which = `Sales_data(const std::string &s): bookNo(s), units_sold(0), revenue(0.0) { }`
- outside the class body: 
    Sales_data::Sales_data(std::istream &is)
    {
        read(is, *this);
    }

8.6 Constructor Initializer List
- members that are <b>const</b> or <b>references</b> or <b>does not define a default constructor</b> must be initialized.
- Initializer is complete when the body of constructor starts executing. The correct way to write the constructor with const or reference members is:

    class ConstRef {
        public:
            ConstRef(int ii);
        private:
            int i;
            const int ci;
            int &ri;
    }
    
    ConstRef::ConstRef(int ii)
    : i(ii), ci(ii), ri(i) {

    }

8.7 Order of Member Initialization
- Members are initialized in the order they appear in class definition, NOT in the order they appear in the initializer list.
    class X {
        int i;
        int j;
        public:
            X(int val): j(val), i(j) {} // i will be initialized by undefined j!
    }

8.8 Default Arguments and Constructors
- A constructor that supplies default arguments for all its members also defines the default constructor.
    class Sales_data {
        public:
            Sales_data(std::string s = ""): bookNo(s) {}
    }

8.9 Delegating Constructors in C++ 11
    class Sales_data {
        public:
            Sales_data(std::string s, unsigned cnt, double price)
            : bookNo(s), units_sold(cnt), revenue(cnt * price) {} // A
            // delegating constructors
            Sales_data() : Sales_data("", 0, 0) {} // B
            Sales_data(std::string s): Sales_data(s, 0, 0) {} // C
            Sales_data(std::istream &is): Sales_data() { read(is, *this);} // D
    }
- In this class, B (default constructor) and C delegate to A, D delegate to B, which delegate to A
- If constructors that are being delegated by others have code, they will be run first


8.10 The Role of Default Constructor

- Classes must have a default initializer to be used in the cases below.
    1. Default initialization
    - define nonstatic variables or arrays at block scope without initializers
    - members of class type uses the synthesized default constructor
    - members of class type are not initialized in a constructor initializer list

    2. Value initialization
    - fewer initializers than array size during array initialization
    - define a local static object without an initializer
    - explicitly request value initialization using the form T() e.g. vector(size)

- `Sales_data obj;` not `Sales_data obj();`, which is a function but a default initializer of an object.

# 9. Copy, Assignment, Destruction

9.1 Vectors and strings can take care of copying, assigning, and destructing dynamic members.

# 10. Access Control and Encapsulation

10.1 Access specifiers: 
- public: accessible to all parts of the program
- private: accessible to member functions but not outside the class.

10.2 class or struct keyword: the only difference is that struct members are public by default, whereas class members are private by default.

10.3 Benefits of encapsulation:
- User code cannot corrupt an encapsulated object
- Implementation of an encapsulated class can change without requiring user code to change.

# 11. Friendship

11.1 Declaring another class or function under a "friend" access specifier can make the outside class or function access <b>all members, including non-public members</b>.

11.2 Friend declarations can appear only inside a class definition. Friends are not members of the class.

11.3 Friend declarations only specifies access. We must also declare them outside the class (usually in the same header file of the class) to use them.

11.4 A friend can also be defined inside the class body; such functions are implicitly inline.

11.5 Friendship is not transitive.

11.6 Making a member function a friend:
    class Screen {
        frined void Window_mgr::clear(ScreenIndex);
    }

Order:
1. define Window_mgr class, which declares but not defines clear, because Screen must be declared before clear can access all its members.
2. define Screen class, which includes a friend declaration of clear
3. define clear, which can now refer to members in Screen

11.7 For overloaded functions with the same functions names, only the one that has the same signature as the friend declaration holds the friendship.


# 12. Type Member

12.1 A class can define type as its members, either public or private

12.2 e.g.:

    class Screen {
        public:
            typedef std::string::size_type pos; // which is equivalent to "using pos = std::string::size_type;"
        private:
            pos cursor = 0; # members with type "pos", defined by a type member
            pos height = 0, width = 0;
            std::string contents;
    }

12.3 Type members must appear <b>before they are used.</b>

# 13. In-line Member ???

# 14. Overloading Member Functions

14.1 Compiler uses number of args to determine which member to run

14.2 E.g. 
- Screen::get()
- Screen::get(pos, pos)

# 15. Mutable Data Members

15.1 A mutable data member is never const and always can be changed, even when it is a member of a const object.

# 16. In-class Initializers for Data Members of Class Type

16.1 Either = or inside {}.

16.2 e.g.: `std::vector<Screen> screens{Screen(24, 80, ' ')}`

# 17. Functions that Return *this

17.1 e.g `Screen &set(pos, pos, char)`. This member function returns an lvalue reference, i.e. it returns the object itself, not a copy of it. 

In this case, `myScreen.move(4,0).set('#') = myScreen.move(4,0); myScreen.set('#');`, whereas if the declaration is `Screen set(pos, pos, char)`, it will return a copy of *this, not the object itself.

17.2 Returning *this from a const member function: this is a pointer to a const object; *this is a const object.
    class Screen {
        public:
            Screen &display (std::ostream os) {
                do_display(os);
                return *this;
            }

            const Screen &display (std::ostream os) {
                do_display(os);
                return *this;
            }
        private:
            void do_display(std::ostream &os) const {
                os << contents;
            }
    }

    Screen myScreen(5,3);
    const Screen blank(5,3);
    myScreen.set('#').display(cout); // calls nonconst version of display
    blank.display(cout); // calls const version of display


# 18. Class Types

18.1 Even if two classes have the same member list, they are different types.

18.2 Both cases are valid for referring to a class type:
- ClassName item1;
- class ClassName item1; // inherited from C

# 19. Class Declarations

19.1 A forward declaration: `class Screen;`, before class is defined. Screen here is an incomplete type.

19.2 Use of Incomplete Type:
- define pointers or references to such types
- declare (not define) functions that use an incomplete type as a parameter or return type

Otherwise, a class must be defined before use, or the compiler will not know how much storage this class object needs.

19.3

    class Link_screen {
        Screen window;
        Link_screen *next; // valid; the declaration of class "Link_screen" is seen after compiler sees its name.
        Link_screen *prev; 
        Link_screen ls; // invalid; the class "Link_screen" has not been defined before the class body finishes, 
                        // the compiler does not know the storage of a Link_screen object.
    }


# 20. Name Lookup

20.1 Order of name lookup in programs: block -> enclosing scope -> error

20.2 Order of name lookup in the body of a member function: member function definitions are processed <b>after</b> the compiler processes all of the class declarations. In this way, the member functions do not need to be ordered.

20.3 Order of name lookup for class member declarations, e.g. return type, types in parameter list: must be seen before they are used. For example:
    typedef double Money; // 2. look for Money declaration in an enclosing scope
    string bal;
    class Account {
        public:
            Money balance() {return bal;} // For return type Money: 1. consider declaration inside Account before the use of Money
                                          // For bal inside the function body: return the private member bal, not the string bal outside the class, because function body is processed after the entire class is seen.
        private:
            Money bal;
    }

20.4 Type names: In a class, if a member uses an outer scope name which is a type, then the class cannot redefine that name.

20.5 Look up a name used in the body of a member function: 
1. look for declaration inside the member function
2. look for declaration inside the class
3. look for declaration in scope before the member function definition

Example for illustrating step 1 and 2:
    int height;
    class Screen {
        public:
            typedef std::string::size_type pos;
            void dummy_fcn(pos height) {
                cursor = width * height; // here "height" is the dummy_fcn function's parameter
                cursor = width * this -> height; // here "height" is the private member of Screen class
                cursor = width * Screen::height; // same as above
                cursor = width * ::height; // here "height" is the global variable;
            }
        private:
            pos cursor = 0;
            pos height = 0, width = 0;
    }

Example for illustrating step 3:
    int height;
    class Screen {
        public:
            typedef std::string::size_type pos;
            void setHeight(pos);
            pos height = 0;
    }
    Screen::pos verify(Screen::pos);
    void Screen::setHeight(pos var) {
        height = verify(var); // here the declaration of "verify" function appears before "setHeight"'s definition, and therefore can be used.
    }


21. Implicit Class-Type Conversions

21.1 A constructor with a single argument defines an implicit conversion from the constructor's parameter type to the class type.

21.2 Only 1 class-type conversion is allowed.

21.3 Class type conversion are not always useful.

22. Explicit

22.1 Add `explicit` in front of class constructor declartions that <b>takes a single parameter</b> can avoid implicit conversions.

22.2 No need to add `explicit` if constructor takes more than 1 arguments.

22.3 Explicit is only added to constructor declarations in class body. It cannot be added to class constructor definitions outside the class body.

22.4 Only for direct initialization, not copying:
    Sales_data item1(null_book); // ok
    Sales_data item2 = null_book; // error: cannot use the copy form of innitialization with an explicit constructor

22.5 Explicitly using constructors for conversions:
    item.combine(Sales_data(null_book)); // ok, the argument is an explicitly constructed Sales_data object
    item.combine(static_cast<Sales_data>(cin)); // ok, static_cast can use an explicit constructor.

22.6 some library classes:
    - string constructor that takes a single parameter of type const char* is not explicit
    - vector that takes a size is explicit

23. Aggregate Classes

23.1 Gives direct access to members. Has special initialization syntax. 
- all data members are public
- does not define any constructors
- no in-class initializers
- no base classes or virtual functions

23.2 e.g.
    struct Data {
        int ival;
        string s;
    };

    Data val1 = { 0, "Anna" };
    Data val2 = { "Anna", 1024 }; // error, the order must be the same as declared

23.3 Number of elements in initializer list must be <= the number of members

23.4 Drawbacks
- require all data members to be public
- require users to correctly initialize every member of every object
- if a member is added or removed all initializations have to be updated


24. Literal Classes

24.1 Literal type classes can have member functions that are constexpr, which are implicitly const.

24.2 Types of literal classes:
- aggregate class whose data members are all of literal type
- nonaggregate class whose
    - data members all must have literal type
    - class must have at least one constexpr constructor
    - if a data member has an in-class initializer, the initializer for a member of built-in type must be a constexpr, or the initializer for a class type member must use the member's own constexpr constructor
    - must use default definition for its destructor

24.3 constexpr constructors
- constructors in a literal class cn be constexpr functions; a literal class must provide at least one constexpr constructor
- can be declared as = default
- must be without return statement, like a constructor, and be like a constexpr function, meaning the only statement it can execute is return. so usually a constexpr constructor is empty.
- must initialize every data member, with either constexpr constructor or constant expression


25. Static Class Members: associate with class, not individual objects, shared among all objects in this class

25.1 Declaration: add keyword "static"; can be either public or private; can be const, reference, array, class type, etc.

25.2 Static members exists outside any project, and are not members of objects.

25.3 Static member functions does not have a this pointer, and may not be declared as const.

25.4 Access static members:
- through scope operator: ClassScope::static_member()
- through object, reference, pointer: 
    Account ac1;
    Account *ac2 = &ac1;
    r = ac1.rate();
    r = ac2 -> rate();

- member functions can use static members without scope operator

25.5 Define static member
- static keyword do NOT repeat in definition outside class body; the keyword is only used for declaration inside the class body
- must be defined and initialized outside the class body
    - not defined when creating objects, and not initialized by constructors
    - may not initialize inside the class
- must be defined outside any function, continue to exist until program ends
- e.g `double Account::interestRate = initRate();`
- static members has access to private members of the class

25.6 In-class Initialization of static Data Members
- for members of const integral type and constexprs for literal types. both are only used in contexts where compiler can substitute the member's value
- if an initializer is provided within the class, the member's definition should not specify an initial value again

25.7 Difference Between Static vs. Non-Static Members
- Static data members can have incomplete type: can have the same type as its class. On the other hand, non-static data members are restricted to being declared as a pointer or reference to an object of its class.
- Static data members can be used as default arguments, nonstatic data members cannot.

