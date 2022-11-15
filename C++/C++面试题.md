1. C++中const的作用是什么：

- 定义一个常量，这个被定义的常量可以被赋值给其他变量，但是常量本身不能被修改

    const int  a = 7; 
    int  b = a;  // b可以被赋值
    a = 8;       // 这里a不能改变

- 修饰函数的参数

    - 直接修饰传递的值，一般这种情况不需要const，因为函数会自动产生临时变量复制实参值

        void Cpf(const int a)
        
    
    - 修饰指针参数，可以防止指针被意外篡改

        void Cpf(int *const a)

    - 修饰自定义类参数，这个时候应该把值传递改为const & 传递，引用传递在函数体内不需要产生临时对象，节省了临时对象的构造、复制、析构过程消耗的时间，可以提高效率，但是引用传递可能会意外地改变实参值，所以加const保护一下

        void fun(A a);
        void fun(A const &a);
    

- 修饰函数的返回值

    - 修饰内置类型的返回值，与不修饰的作用一样
        
        const int A();
    
    - 修饰自定义类型函数的返回值，此时返回的值不能作为左值使用，不能被赋值也不能被修改

        const A func();
    
    - 修饰返回的指针或者引用，则返回值不能被直接修改，且该返回值只能被赋值给加const修饰的同类型指针

        const char *GetChar(void){};
        char *ch = GetChar(); // error
        const char *ch = GetChar(); // correct

- 修饰类的成员函数，可以防止成员函数修改这个类的成员变量

    class C
    {
        private:
            int count;
        
        int GetCount() const
        {
            return count;
        };
    };


资料来源：
https://www.runoob.com/w3cnote/cpp-const-keyword.html
https://weread.qq.com/web/reader/60c32ad0715a382160c9d1ck16732dc0161679091c5aeb1
https://blog.csdn.net/u013270326/article/details/78388461


2. 宏和const的区别
- 宏用#define命令来定义，它用来将一个标识符定义为一个字符串，该标识符被称为宏名，被定义的字符串称为替换文本。
- #define的宏只是用来做文本替换的，例如：
    
        #define PI 3.1415926
        float angel;
        angel = 30 * PI / 180;

    程序在编译的时候，编译器会将#define PI 3.1415926后所有的PI全替换为字符串“3.1415926”，然后进行编译，
    所以能用const就不要用宏，因为宏是文本替换，很容易出错
    

- 宏常量存在于程序的代码段，其生命周期只存在于compile time，在程序中它只是一个常数、一个命令的参数，并没有实际存在，宏常量没有数据类型。
const常量存在于程序的数据段，并在堆、栈分配了空间，其生命周期存在于run time，在程序中确实存在并可以被调用、传递，const常量有数据类型，编译器可以对const常量进行安全检查。


资料来源：
https://www.cnblogs.com/fnlingnzb-learner/p/6903966.html
https://weread.qq.com/web/reader/60c32ad0715a382160c9d1ck16732dc0161679091c5aeb1


3. 引用和指针的区别
- 初始化：引用在创建/定义的时候必须被引用到一个有效的对象，指针在定义的时候不必初始化，可以在定义后面的任何地方重新赋值
- 可修改性：引用一旦被初始化指向一个对象，它就不能引用另一个对象了；而指针在任何时候都可以指向另一个对象
- NULL：引用不能指向NULL，它必须总是指向某个对象；而指针可以是nullptr
- 测试：由于引用不会指向NULL，所以使用引用之前不需要测试它的合法性，而指针则经常需要测试，所以引用的代码效率>指针的代码效率
- 应用：如果一旦指向一个对象，再也不会改变指向的对象，用引用；如果有可能指向NULL或者改变指向的对象，用指针

资料来源：
https://weread.qq.com/web/reader/60c32ad0715a382160c9d1ck8f132430178f14e45fce0f7

4. 有几种new，这几种new的机制是什么？
- new operator（操作符）：1.申请动态内存 2.调用类的构造函数，用来初始化对象
- operator new: 调用malloc申请一块原始的、未命名的内存空间
- placement new: 在已经分配好的空间里，调用构造函数，创建一个对象

5. 虚函数和纯虚函数的区别
- 虚函数是在成员函数的声明前加virtual(virtual void func())，每个子类可以也可以不override基类的虚函数，子类如果不提供虚函数的实现，将会自动调用基类里的虚函数
- 纯虚函数是在虚函数的声明后加"=0"（virtual void func() = 0），每个子类必须override基类里的纯虚函数，子类如果不提供纯虚函数的实现，编译会失败

https://zhuanlan.zhihu.com/p/37331092

资料来源：
https://blog.csdn.net/xiaorenwuzyh/article/details/44514815
https://yosef-gao.github.io/2016/10/01/cpp-new/
https://cloud.tencent.com/developer/article/1496771


6. 指针常量和常量指针的区别
指针常量：不能改变指向，但是能修改指向变量的值 （int *const p)
常量指针：可以修改指向，但是不能修改指向变量的值 (const int *p, int const *p)

资料来源：
https://blog.csdn.net/qq_36132127/article/details/81940015


7. Map和哈希Map的区别是什么？它们各自的使用场景是什么？
1. map和hashmap的底层数据结构不同：map是红黑树实现的，而hashmap是用哈希表实现的
2. map的优势是元素可以自动按照键值排序，查找的时间复杂度是O(log(n))。hashmap的优势是各项操作的时间复杂度接近O(1)。
3. map属于STL标准的一部分，hashmap不是

资料来源：
https://weread.qq.com/web/reader/60c32ad0715a382160c9d1ck9bf32f301f9bf31c7ff0a60

8. public, protected, private之间的区别
一个类的public成员可以在整个程序的任何地方被访问到；
一个类的protected成员只能被这个类本身和它的子类访问到；
一个类的private成员只能被这个类内部的成员函数访问到


9. 静态成员函数和普通成员函数的区别
一个类的静态成员函数是被这个类的所有对象共享的，而不属于某一个对象，所以静态成员函数没有this指针，无法访问非静态的成员。
而普通成员函数因为有this指针，所以既可以访问静态成员也可以访问非静态成员。

资料来源：
https://blog.csdn.net/qq_22238021/article/details/79546270




