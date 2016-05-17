title: c++基础：关键字与限定符
date: 2016-04-23 18:24:33
tags: [c++,c,编程语言]
toc: true
---

[TOC]
## const 限定符
const限定符用于限定一个对象不能发生改变。
### const修饰对象
> - const对象必须被初始化，初始值可以使任意复杂的表达式
> - const对象只能执行不改变其值得操作
> - const对象被设定为仅在文件内有效
> - const对象以及常量对象的引用或者指针只能调用常量成员函数

### const修饰类成员函数
const修饰类成员函数相当于把修改隐式this指针的类型，this指针默认情况下是一个指向非常量对象的常量指针。const修饰成员函数之后，this指针的类型被改变为指向常量对象的常量指针。所以const修饰函数无法改变类的成员。
> - const修饰类成员函数时，表明该函数不能修改类成员，否则编译器会报错
> - **特别值得注意的是当const引用只能调用const方法，否则会编译报错**

### const修饰引用与指针
详见 [c++基础：指针与引](http://bu540402.github.io/2016/04/23/c++%E5%9F%BA%E7%A1%80-%E6%8C%87%E9%92%88%E4%B8%8E%E5%BC%95%E7%94%A8/)

### const与函数重载

> - 在函数重载中，顶层const（自身是常量的对象：常量对象或者本身是常量的指针）的形参无法与非顶层const的形参区分开来。
> - 在函数重载重，底层 const（常量引用以及指向常量的指针）可以与非常量对象区别从而实现重载

## static限定符

## 访问说明符
### public
1. 修饰类成员：可以被1.该类中的函数、2.子类的函数、3.其友元函数访问，也可以由4.该类的对象访问。
2. 公有继承：父类中的protected和public属性不发生改变

### private
1. 修饰类成员：只能由1.该类中的函数、2.其友元函数访问。不能被任何其他访问，该类的对象也不能访问(除非该对象位于1或者2中)。**private 属性不能够被继承。**
2. 私有继承：父类的protected和public属性在子类中变为private

### protected
1. 修饰类成员：可以被1.该类中的函数、2.子类的函数、以及3.其友元函数访问(除非该对象位于1或者2中)。但不能被该类的对象访问。
2. 保护性继承：父类的protected和public属性在子类中变为protected

### 深入探讨
首先看一个例子：
```
class A {
public:
	A(int i_, int j_) {
		i = i_;
		j = j_;
	}
	void disp(A a) {
		cout <<"disp : "<<endl;;
		cout << "\t" << a.i << endl;
		cout << "\t" << a.j << endl;
	}

private:
	int i;
protected:
	int j;
};
class B:public A{
	public:
		B():A(0,1){}

};
class c:public A{
	public:
		C():A(0,1){}
		void disp(A& a) {
			cout <<"disp : "<<endl;;
			cout << "\t" << a.i << endl;
			cout << "\t" << a.j << endl;
		}

};
int main(int argc, char* argv[]) {
	A a1(123, 456);
	A a2(789, 543);
	B bb;
	a1.disp(a2);//正确
	a2.disp(a1);//正确
	bb.disp(a1);//正确
	C c;//类C会编译错误 c，C没有权限访问通过A的对象的privaite以及protected成员
	return 0;

}

```
类的本质是对数据成员以及进行于其上的一系列操作（成员函数）的一种封装。对象是类的实例化，在内存映射中每个对象仅仅保留属于自己的那份数据成员副本。而成员函数对于整个类是共享的，也就是说一个类只保留一份成员函数。成员函数通过this指针来操作同一个类的不同实例的数据成员。

## 友元 friendly
友元函数包括3种：设为友元的普通的非成员函数；设为友元的其他类的成员函数；设为友元类中的所有成员函数。

## 可变数据成员 mutable
