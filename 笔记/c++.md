# 面向对象

## 类：
定义一个类需要用到 class 关键字，首先说明类的名称，主体包含在{ } 之中，包括成员变量和成员函数


![[Pasted image 20260711233125.png]]
###### 关键字（private/public/protected)：
public：成员在外部是可访问的
protected：仅类内部和子类可访问，主要用于继承
private：仅限类内部，用于不想被随便修改的变量

##### 定义对象：
例如：
```C++
box box1  //声明 box1 类型为box
box box2  //声明 box2 类型为box
```
box 是类，box1 是对象的名称
#### 成员函数：
类的成员函数是把声明和定义写在类定义内部的函数，是类的一个成员，可以操作类的任意对象、访问对象中的所有成员。

类似下面的例子，可以直接在类内部定义，此时即便没有使用 *inline* 标识符，函数也是默认内联的。
```C++
class Box { 
	public: 
	double length; // 长度 
	double breadth; // 宽度 
	double height; // 高度 
	
	double getVolume(void) 
		{ 
		return length * breadth * height; 
		} 
};
```

也可以在类外部定义：

```C++
double Box::getVolume(void) 
{ 
return length * breadth * height; 
};
```
在 :: 之前一定要加上类名，调用成员函数是在对象后使用点运算符 . ，这样就可以操作与类相关的数据了。

##### 构造函数：
常用于初始化成员变量，没有任何类型
构造函数与类的名称一样，在创建对象的时候就会执行构造函数，即便不进行任何操作


### 继承

在写多个功能类似的类时，如果把每一个类单独编写则会过于麻烦，这时就要用到继承
继承可以让一个类获得另一个类的所有方法，除了构造函数、析构函数、重载运算符和友元函数，会受到基类中访问修饰符限制，也会受到继承时的访问修饰符的限制，但多数情况用public继承
语法如下：
```
class class2 :: public class1{

}
```

### 多态

在继承之后，可能需要的函数与基类的函数之间有差别，这就需要对派生类的函数进行重写
##### 虚函数
虚函数是实现多态的关键，通过关键词 virtual ，使函数可以被重写。
```c++
class class1{

	virtual void fun();
};
```
在派生类中，使用 override 关键字重写函数，声明文件语法如下
```C++
class class2 : public class1{

public:
	void attack() override;

};
```
在实现文件中则不用再写override，直接定义即可。

虚函数的实现依靠于虚函数表和虚指针，使用virtual之后，类中会生成一个虚函数表，包含了所有虚函数，创建的对象中则包含了一个虚指针指向虚函数表，在调用函数时，会找到对象所属类型的真正的函数。
###### 虚析构
子类创建对象时，通常是先创建父类部分，再创建子类部分，先销毁子类部分再销毁父类部分
```C++
int main() { 
	Character* c = new Warrior(); 
	delete c; 
}
```
但是当用基类指针管理派生类时，delete只会看到基类部分，从而导致子类析构没有执行，可能会造成内存泄漏等问题
这时就要用到虚析构，即在析构函数前也加上virtual，因为虚函数机制同样适用于析构函数，这样delete也会通过虚指针来找到真正的析构函数
##### 纯虚函数和抽象类
有的时候我们不会用基类去创建对象，基类只是来提供一些**接口**
这是就要用到纯虚函数，包含纯虚函数的类叫抽象类，无法创建对象
例如：
```C++
class IWeapon { 
	public: 
		virtual void fire()=0; 
```
这里的fire()就是纯虚函数，他会强制子类提供一个具体的实现

# 基础

### vector
相对于数组，vector可以自动调整数组大小，连续内存（调用效率高，支持通过下标快速访问

使用vector时，需先包含头文件
```C++
#include<vector>

//创建空vector
 std::vector<int>vec;
 std::vector<int>vec(5,0);
 std::vector<int>vec = {1,2,3,4}
 //添加元素
 vec.push_back(0);
 vec.emplace_back(0);//更推荐
 
 //两种访问操作  []不检查越界，更快  .at（）检查越界，更安全
 int y = vec[0];
 int y = vec.at(1);
 
 //大小  size是实际大小  capacity()是已分配的内存容量
 vec.size()
 vec.capacity()
 
 //删除第三个元素  删除中间元素时，后面的元素会整体前移
 vec.erase(vec.begin() + 2);
 
 vec.clear()//清除，只会清空元素，内存会保留
 std::vector<int>().swap(vec);//释放内存
 
 
```

vector在扩容时代价很大，所以常见操作是先分配内存
```
vec.reserve(10000)
```

### 引用
```C++
void character_manager::all_attack(){

for(auto& c:characters){

c->attack();

	};
};
```

&表示引用，代表直接操作数组中的元素，如果不加的话是值拷贝，会生成副本，较慢，而且会发生对象切片，即多态失效。

### 循环
基于范围的循环，C++11新特性
```
for (int &x : my_array) { 
	x *= 2; 
	cout << x << endl; 
} 
for (auto &x : my_array) { 
	x *= 2; 
	cout << x << endl; }
```
 ==auto 类型也是 C++11 新标准中的，用来自动获取变量的类型==，&依旧表示引用，可以直接操作数组中的元素


##### 对引用&的看法
函数接受变量，有的只是为了在函数中使用该变量，但是有一些函数的功能是修改被传入的变量，这时就要用到引用，因为正常的传入是将该变量拷贝了一份给函数，而用引用则是直接操作变量的值，更深入的讲，在函数不需要修改变量时，可以用 const只读修饰符加上引用&，对于一些大型的对象来说会更快，特别是结构体，但是对于一些比较小的类型，直接传入可能更快，因为引用的底层是指针

##### argc argv
一般程序的入口都是这样
```C++
int main(){
}
```
但是在真实开发中通常是下面的情况：
```C++
int main(int argc , char*argv){
}
```
argc 是保存运行时传入的参数的个数
argv是保存运行时传入的参数
