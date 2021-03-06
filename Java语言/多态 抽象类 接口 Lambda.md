### 多态
- 面向对象的语言的三大特性
    - 封装
    - 继承
    - 多态
 
- 多态分为编译时多态和运行时多态
- 编译时多态，其实就是基于方法的重载，根据不同参数具现化导致调用不同的函数。
```java
    class Person{  
    public String toString() {  
        String name = "Person";  
        return name;  
    }  
    public String toString(String str) {  
        String name = str;  
        return name;  
    } 
}  
  
class Man extends Person{  
    public String toString(){  
        String name = "Man";  
        return name;  
    }  
}
public class CompilePolymorphic {

	public static void main(String[] args) {
		Person p = new Person();         //对象引用本类实例  
        Man m = new Man();               
        System.out.println(p.toString());  //编译时多态，执行Person类的toString()  
        System.out.println(m.toString()); //编译时多态，执行Man类的toString()  
        System.out.println(m.toString("Who")); //编译时多态，执行Person类的toString(String)  
	}
}
```

- 运行时多态，也叫动态绑定，是基于类的继承。指在执行期间（非编译期间）判断引用对象的实际类型，根据实际类型判断并调用相应的属性和方法。主要用于继承父类和实现接口时，父类引用指向子类对象。
- 必要条件：继承， 重写， 向上转型
- 原则：当超类对象引用变量引用子类对象时，被引用对象的类型而不是引用变量的类型决定了调用谁的成员方法，但是这个被调用的方法必须是在超类中定义过的，也就是说被子类覆盖的方法。
    - 用一个父类引用指向子类的对象，调用方法仍是子类方法，如speak().
    - 父类中的属性和静态方法只能被隐藏，调用的属性和静态方法都是父类中的属性和静态方法, age, sing().
    - 在父类的方法中调用的方法，仍然是子类中的方法, shout().
```java
class Father {
	int age = 40;
	void speak() {
		System.out.println("I am father");
	}
	static void sing() {
		System.out.println("Father sing");
	}
	void shout() {
		System.out.println("Father angry");
		speak();
	}
}
class Son extends Father {
	int age = 20;
	void speak() {
		System.out.println("I am son");
	}
	static void sing() {
		System.out.println("Son sing");
	}
	void shout(String str) {
		System.out.println("Father angry" + str);
	}
}
public class RuntimePolymorphic {

	public static void main(String[] args) {
		Father father = new Son();
		father.speak();
		// 父类中属性只能被隐藏，而不能被覆盖；
		// 而对于方法来说，方法隐藏只有一种形式，就是父类和子类存在相同的静态方法。
		father.sing();
		System.out.println("I am " + father.age + " years old!");
		// 首先，利用编译时多态，执行父类中的shout()方法，然后仍然调用子类的speak()
		father.shout();
	}
}
```
- 输出
> I am son</br>
Father sing</br>
I am 40 years old!</br>
Father angry</br>
I am son</br>


### 接口
- Java接口是契约的集合
- C++中的多重继承模型，导致了钻石问题。
	- 因为Liger多重继承了Tiger和Lion类，因此Liger类会有两份Animal类的成员（数据和方法），Liger对象"lg"会包含Animal基类的两个子对象，编译器并不知道是调用Tiger类的getWeight()还是调用Lion类的getWeight()。
```c++
class Animal { /* ... */ }; // 基类  
{  
int weight;  
public:  
int getWeight() { return weight;};   
};  
  
class Tiger : public Animal { /* ... */ };  
class Lion : public Animal { /* ... */ }      class Liger : public Tiger, public Lion { /* ... */ };  

int main( )  
{  
Liger lg ;  
/*编译错误，下面的代码不会被任何C++编译器通过 */  
int weight = lg.getWeight();    
｝
```
- 
	- 解决方法
	- 现在类Liger对象将会只有一个Animal子对象，代码编译正常
	```C++
	class Tiger : virtual public Animal { /* ... */ };  
	class Lion : virtual public Animal { /* ... */ }  
	```
- 接口中可以包含
	- 成员变量（静态常量）
	- 方法（抽象实例方法，类方法，**默认方法**）
		- 默认方法。
			- Java8之前，接口不能升级。因为在接口中添加一个方法，会导致老版本接口的所有实现类的中断。于是引入了默认方法(defender methods,Virtual extension methods)。它是库/框架设计的程序员的后悔药。
			- 对于以前的遗留代码，大家都不知道有这个新方法，既不会调用，也不会去实现，如同不存在；编写新代码的程序员可以将它视为保底的方法体。类型层次中任何符合override规则的方法，优先于默认方法，因为遗留代码可能正好有同样的方法存在。
	- 内部类（内部接口，枚举）
- 接口中不能包含
	- 构造器
	- 初始化块定义
- 接口中所有的修饰符都是public。静态常量为public static final，除默认方法类方法都是public abstract，内部类包含的都是public static。默认方法是public default，类方法是public static。

- 接口支持多继承，获得父接口的所有抽象方法和常量。
```java
interface A {
	public static final int AAA = 0;
	public abstract void ATest();
}
interface B {
	public static final int BBB = 0;
	public abstract void BTest();
}
// C中可以获得AAA和BBB，以及抽象方法
interface C extends A,B {
	public static final int C = 0;
	public abstract void CTest();
}
```

- 接口的继承
	- 子类重写父类方法时访问控制权限只能更大或相等
	- 实现抽象方法的访问控制修饰符只能是public

### 内部类
- 作用
	- 更好的封装，把内部类隐藏在外部类之内
	- 非静态内部类成员可直接访问外部类私有数据，但外部类不能访问内部类的实现细节。
		- 因为非静态内部类实例必须寄生在一个外部类实例中，因此非静态内部类保存了寄生的外部类的引用，因此可以访问其private成员。
		- 若在内外类中存在同名变量，可用this.member和OuterClass.this.member来区别。
	- 匿名内部类适用于仅需要一次使用的类。
- 限制
	- 内部类可以用private，protected，static来修饰
	- 非静态内部类不能拥有静态成员

- 静态内部类
	- 这个内部类属于外部类本身，而不是属于某个实例。
	- 静态内部类无法访问外部类的实例变量，只可以访问外部类的类变量。

#### 匿名内部类-Java 8
- 匿名内部类不能是抽象类，不能定义构造器
- 匿名内部类可以访问局部变量，此时局部变量自动变为final
```java
interface BaseClass {
	void test();
}
public class AnonymousInnerClass {

	public static void main(String[] args) {
		int age = 8;
		BaseClass bc = new BaseClass() {
			
			@Override
			public void test() {
				System.out.println(age);
				
			}
		};
		bc.test();
		// 编译报错，因为匿名内部类使用了age，则age自动变成final
		// System.out.println(age++);
	}
}
```

### Java 8新增 Lambda表达式
- 由三部分组成
	- 形参列表
	- 箭头 ->
	- 代码块

```java
interface Eatable {
	void taste();
}
interface Flyable {
	void fly(String weather);
//	为了使用lambda表达式，这个接口必须函数式接口，
//	即除了从父接口继承过来的抽象方法，只包含一个自己特有的抽象方法
//	void down(String tsyStr);
//	这个抽象方法时Object的方法
	@Override
	boolean equals(Object obj);
}

public class LambdaExp {
	public void eat(Eatable e) {
		System.out.println(e);
		e.taste();
	}
	public void fly(Flyable f) {
		System.out.println("I am fly " + f);
		f.fly("the sky is clear");
	}
	public static void main(String[] args) {
		LambdaExp lambdaExp = new LambdaExp();
		lambdaExp.eat(()->{
			System.out.println("The apple is taste good");
		});
		lambdaExp.fly((weather)->{
			System.out.println(weather);
		});
	}
}
```

- Lambda  表达式与函数式接口
	- 函数式接口
		- 只包含一个抽象方法的接口，可以包含多个默认方法，类方法，但只能声明一个抽象方法。
		- 可用@FunctionalInterface注解
	
```java
	Runnable runnable = ()->{
			for (int i = 0; i < 100; i++) {
				System.out.println(i);
				
			}
		};
```
- Lambda 限制
	- Lmabda表达式目标类型必须是明确的函数式接口
	- 只能为函数式接口创建对象

- 使用
	- 把Lambda表达式赋值给函数式接口类型的变量或参数
	- 函数式接口类型对Lambda表达式进行强制类型转换

- 如果Lambda只有一条代码，可以在代码中使用方法引用和构造器引用

| 种类 | 示例 | 对应的Lambda表达式 |
| :---: | :---: | :---: |
|  引用类方法 | 类名::类方法 | (a,b...)->类名.类方法(a,b...)|
|引用特定对象的实例方法|特定对象::实例方法|(a,b..)->特定对象.实例方法(a,b..)|
|引用某类对象的实例方法|类名::实例方法|(a,b..)->a.实例方法(b..)|
|引用构造器|类名::new|(a,b..)->new  类名(a,b..)|
```java
@FunctionalInterface
interface Converter {
	Integer convert(String from);
}

@FunctionalInterface
interface MyTest {
	String test(String a, int b, int c);
}

@FunctionalInterface
interface MyTest1 {
	String test(String a);
}
public class LambdaReference {

	public static void main(String[] args) {
		// 类名::类方法
		Converter converter1 = (from)->
			Integer.valueOf(from); // 自动return
		System.out.println(converter1.convert("22"));
		Converter converter2 = Integer::valueOf;
		System.out.println(converter2.convert("33"));
		
		// 特定对象::实例方法
		Converter converter3 = from->
		"Hello World".indexOf(from);
		System.out.println(converter3.convert("or"));
		Converter converter4 = "Hello World"::indexOf;
		System.out.println(converter4.convert("Wo"));
	
		//类名::实例方法
		MyTest myTest1 = (a, b, c)->a.substring(b, c);
		MyTest myTest2 = String::substring;
		System.out.println(myTest2.test("Ni Hao China!", 2, 3));
		
		//类名::new
		MyTest1 test1 = (from)->new String(from);
		MyTest1 test2 = String::new;
		System.out.println(test2.test("Haha"));
	}
}
```

#### Lambda 和匿名内部类
- 相同点
	- 都可以访问final局部变量，及外部类的成员变量
	- 都可以从接口中继承默方法
- 区别
	- Lambda只能为函数式接口创建实例
	- 匿名内部类可以为抽象类甚至普通类创建实例 
	- Lambda的代码块不能调用接口中定义的默认方法