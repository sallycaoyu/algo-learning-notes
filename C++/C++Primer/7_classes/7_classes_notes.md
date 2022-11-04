1. Struct vs. Class

1.1. Struct: Public by default
Class: Private by default

1.2 Both struct and class can be used to define a type. Usually a class is used for defining a type, unless we want all variables to be accessed directly from outside. 

1.3 In C++, both struct and class can be used to define member variables and functions in them. Note that we can only define member variables but not member functions in C structs.

2. Declaration vs. Definition, of Member Functions vs. Non-member Functions

2.1 Declaration of a member function must be inside the class body. Definition of a member function may be either inside or outside the class body.

2.2 Definition <b>and</b> Declaration of a non-member function should be outside the class body.

3. this

3.1 Member function access the object on which it is called by an implicit "this" parameter. Calling "this" returns the <b>address</b> of the object itself.

3.2 "this" is implicitly defined. We cannot define "this" as a parameter or variable.

3.3 "this" is a const pointer, meaning we cannot change the address it points to.

4. Const Member Functions

4.1 Example: `std::string isbn() <b>const</b> {return this->bookNo;}`. The const here after parameter list is to <b>change the type of "this"</b>, so that "this" can be binded to a const object.

4.2 By default, the type of "this" is `ClassType *const`, so "this" cannot be binded to a <b>const object</b>. But if we make isbn a const member function, then:
`std::string isbn() const {return this->bookNo;}` is equivalent to `std::string isbn(const ClassType *const this) {return this->bookNo;}`. The const member function cannot change the object ("this") it is called on.

5. Class Scope and Member Functions

5.1 A class itself is a scope. A member function is defined in the scope of class it belongs to. 

5.2 Even if something A is defined in the class scope but <b>after</b> a member function B, the member function B can still access A because the member declarations are compiled at first.

6. Defining a Member Function outside the Class

6.1 Example:
    double Sales_data::avg_price() const {
        if (units_sold)
            return revenue/units_sold;
        else
            return 0;
    }

After seeing <b>Sales_data::</b>, compiler will interpret the function in the scope of Sales_data class. The variables revenue and units_sold will implicitly refer to the member variables in Sales_data class.

7. Defining a Function to Return "this" Object

7.1 We don't need "this" to access a member variable, but we need it to access the object as a whole, like `return *this`.

8. Constructors

8.1 Constructors do not have a return type.

8.2 Constructors <b>cannot be defined as const</b>. This is because when we construct a const object, we still need to store values into it. A const object becomes "const" <b>after its initialization is finished.</b>















