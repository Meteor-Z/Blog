# 堆,栈,RAII

堆：分配内存之后需要手动释放，否则，将要造成内存泄漏

- `new`和`delete`操作的是`free store`
- `malloc`和`free`操作的是heap

`new`的底层就是`malloc`

动态获取内存都是在堆中进行的
`auto ptr = new std::vector<int>()`

# 智能指针

## unique_ptr

独享它所指向的指针，也就是说，同时只有一个unique_ptr指向一个对象，不能多个`unique_ptr`指向一个对象，
如果说`unique_ptr`被销毁，那么它所指向的对象也会被销毁

不能进行拷贝复制

## shared_ptr

共享他们呢所指向的指针，多个`shared_ptr`可以同时指向一个相同的对象，在计算机内部采用技术机制来实现
如果有对象关联的话，那么引用计数就会+1，如果智能指针减少，那么就会-1,当变成0,贼会将这个对象进行释放

## 删除器

在构造函数中第二个删除器就行了

1. 可以进行拷贝复制

## 两者区别

`unique_ptr` 占用资源比较小，所以大多数还是用`unique_ptr`吧

## 字面量

常量是固定的，在程序执行期间不会改变，这些固定的值，就是字面量

## 左值和右值

左值：一般指一个特定内存的具有名称的值
右值：不指向稳定内存空间地址的匿名值（不据名对象）

通过取地址来判断左值还是右值：
`如果能取到地址 就是左值，否则就是右值`

### x++和++x

x++:右值：首先生成一个x值的临时拷贝。然后才最x递增，最后返回临时复制内容
++x;左值：直接对x递增后马上返回自身，所以说++x是一个左值

注意：字符串常量存储在程序的数据段中，会在加载的时候开辟内存空间，可以使用&来获取字面两的地址

左值就比较强
如果函数返回的是一个值，return x 那么就会变成 右值

## 左值引用

传入函数的时候用 void swap(int &a,int &b); 加入

注意： 指针运算的时候会发生计算

## 文件操作

文件流都在`<fstream>`里面

## 复合

构造函数由内到外,析构函数由外道内,`c++`会默认进行执行这些代码,我们只要完成其中的函数
如果自己不写的话，那么就要调用默认的构造函数和析构函数

## 委托

指针，不同步，外边的函数有了，但是里面的函数有可能没有
什么是委托?
但是

## 继承

构造函数:先先构造自己的父类，然后再构造自己的
析构函数:先执行父类的析构函数，然后再析构自己

### 虚函数

`non-virtual`:不希望子类重新定义他
`virtual`:希望子类能够重新定义他
`pure virtual`:希望子类一定要重新定义他

```c++
#include <iostream>

class CDocument
{
private:
public:
    void on_file_open()
    {
        std::cout << "你好" << std::endl;
        std::cout << "check file status.." << std::endl;
        std::cout << "open file" << std::endl;
        Serialize();
        std::cout << "close file" << std::endl;
        std::cout << "update all views" << std::endl;

    }
    virtual void Serialize() = 0; // =0 就是纯虚函数 需要子类亲自实现进行实现
};
class CMyDoc : public CDocument
{
private:
public:
    virtual void Serialize()
    {
        std::cout << "CMyDoc::Serialize()" << std::endl;
    }
};
int main()
{
    CMyDoc myDoc;
    myDoc.on_file_open();
}

```

定义虚函数是允许用基类的指针来调用子类的函数
纯虚函数是为了实现一个接口，起到一个规范的作用，规范是：子类必须继承这个函数

## 转换函数

```c++
class Fraction
{
private:
    int a, b; // a 是分子, b 是分母
public:
    Fraction(int a, int b = 1) : a(a), b(b)
    {}

    // 当做成 double 类型的时候，就会被调用起来
    operator double() const
    {
        return 1.0 * a / b;
    }
};

### explicit

如果加上的话，就是当正确的调用构造函数的时候才会出现，不会隐式的出现这个函数，
所以只要是隐式转换，就会寄了，

`注意`: 这个函数只在构造函数的时候会出现，十分明确当正确的时候才会出现这个函数

```c++
class Fraction
{
private:
    int a;
public:
    Fraction(int a = 1) : a(a)
    {}

    Fraction operator+(const Fraction& other)
    {
        return a + other.a;
    }
};


```

## 迭代器

## 仿函数

不懂

## namespace

如果使用`using namespace std`,那么就会发生冲突，所以说还是用namespace 包起来，然后再使用吧

## 成员模板

在模板里面的模板，在模板里面发生过变化

## 函数特化

面对

## 友元函数

外面的函数不能直接使用`private`里面的东西，需要加入friend 才能，否则第一个函数的自变量是自己this，

## noexcept

```c++
void f() noexcept
{

}
```

保证这个函数不会抛出异常，

## Variadic Template (不定参数的模板)

```c++
void print()
{

}
template<typename T, typename... Types>
void print(const T& first_arg, const Types& ... args)
{
    std::cout << first_arg << std::endl;
    print(args...);
}
```

参数可以直接输出递归输出，最后一个是 变成（分解） 1 和 0 ，就会调用第一个print()这个
（这样是处理边界条件）
注意： 应该使用

但是他可以跟这样并存，如

```c++
template<typename... Types>
void print(const Types&... args)
{
}
```

## nullptr和 std::nullptr_t

在 c++ 中 还是建议使用 nullptr 使用，否则会有奇异

## 强制类型转换

1. static_cast<T>(expression): 编译时
    1. 将一个指向基类的指针转换为指向子类的指针，但是这样的转换不总是安全的
    2. 基本类型之间的转换(int,float,double)之间的转换
2. const_cast<T>(expression)
    1. 用于去除变量的只读属性
    2. 强制转换的目标类型只能是指针或者引用
3. reinterpret_cast<T>(expression) 将一个指针重新解释成另一个指针类型
    1. 用于指针类型之间的转换
    2. 用于整数和指针类型之间的强制转换

4. dynamic_cast<T>(expression):运行时
   1. 有继承关系的类指针之间的转换
   2. 有交叉关系的类指针之间的转换
   3. 具有类型检查的功能
   4. 需要虚函数的支持


## 防卫式声明

防止重复命名，如果多次引用，那么就会寄了
基本格式是： （注意要规范： 要大写）

```c++
#ifndef XXXXX
#define XXXXX

内容

#endif
```

## C++线程

注意一点：
执行类的成员函数的时候，如果不是静态函数的话，那么就要保证创建一个对象，然后才能调用
`必须保证对象的生命周期比自线程要长`

子线程一定要回收，使用`xxx.join()`来回收线程

多线程会产生冲突，我们要避免冲突

`std::cout`是全局对象，当前对象如果全部都在搞这个，那么就寄了

我们要保证访问这个信息数据的时候，要保证线程安全，

所以说我们要用互斥锁来保证

### 条件变量-生产/消费者模型

当前函数的额度所有

## 移动构造

## RAII 与 智能指针
