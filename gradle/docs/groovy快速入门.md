# groovy快速入门

### 前言
开源软件使用gradle构建项目越来越多，所以开始学习gradle，更新一下知识；
由于gradle是基于groovy语言使用的，需要具备一些groovy语言的知识，所以记录一下对groovy这门语言的理解；

### 简介
Groovy 是 Apache 旗下的一门基于 JVM 平台的动态/敏捷编程语言，在语言的设计上它吸纳了 Python、Ruby 和 Smalltalk 语言的优秀特性，语法非常简练和优美，开发效率也非常高（编程语言的开发效率和性能是相互矛盾的，越高级的编程语言性能越差，因为意味着更多底层的封装，不过开发效率会更高，需结合使用场景做取舍）；
Groovy 可以与 Java 语言无缝对接，在写 Groovy 的时候如果忘记了语法可以直接按Java的语法继续写，也可以在 Java 中调用 Groovy 脚本，都可以很好的工作，这有效的降低了 Java 开发者学习 Groovy 的成本。Groovy 也并不会替代 Java，而是相辅相成、互补的关系，具体使用哪门语言这取决于要解决的问题和使用的场景。

### 快速开始
- 下载Groovy开发工具包（GDK）  
http://www.groovy-lang.org/download.html  
- 创建groovy项目  
确认idea中安装了groovy插件，在创建项目的时候，选择groovy就ok，和创建java项目一样  
- 编写 Hello World 
创建一个groovy文件，写下以下代码
```groovy
System.out.println("hello world");
System.out.println "hello world";

println("hello world")
println 'hello world'
```

以上是打印字符串的4种方式  
如果 Groovy 脚本文件里只有执行代码，没有类的定义，则 Groovy 编译器会生成一个 Script 的子类，类名和脚本文件的文件名一样，而脚本中的代码会被包含在一个名为run的方法中，同时还会生成一个main方法，作为整个脚本的入口；
所以，作为 JVM 平台语言，与 Java 本质上还是一样的。

### 与java的对比学习  
- 基础语法  
Groovy 语句无需使用分号（;）结尾，当然加上也不会报错，毕竟完全兼容 Java 的语法；  
Groovy 中==等价于 Java 中的equals方法；  
注释和 Java 一样，支持单行（//）、多行（/* */）和文档注释（/** */)；  

- 变量  
Groovy 中定义变量默认访问修饰符是public，变量定义时遵循 Java 变量命名规范，变量名以字母、下划线或美元符号$开始，可以包含字母、数字、下划线和美元符号$，但关键字除外。除了这些规则之外，Groovy 定义变量时如果是一行定义一个类型，末尾的分号可以省略，但是如果多个变量占一行，变量之间必须以分号分割  
Groovy 定义变量的方式和 Java 是类似的，区别在于 Groovy 提供了def关键字供使用，它可以省略变量类型的定义，根据变量的值进行类型推导  

- 字符串   
在Groovy种有两种字符串类型，普通字符串（java.lang.String）和插值字符串（groovy.lang.GString）  
普通字符串使用单引号  
groovy由于双引号所包括起来的字符串字面量会被解释为 GString 值（即 “Groovy 字符串”的简称），所以如果当某个类中的 String 字面量含有美元字符（$）时，那么利用 Groovy 和 Java 编译器进行编译时，Groovy 很可能就会编译失败，或者产生与 Java 编译所不同的结果
groovy带双引号的字符串使用等同于scala中的字符串扩展    
```groovy
def name = "abc"
println "nihao ${name}"
```

- 方法  
Groovy 方法的默认访问修饰符是public，方法的返回类型可以不需要声明，但需添加def关键字。有返回值的方法return可以被省略，默认返回最后一行代码的运行结果，如果使用了return关键字则返回指定的返回值
```groovy
String method1() {
    return 'hello'
}
assert method1() == 'hello';

def method2() {
    return 'hello'
}
assert method2() == 'hello';

def method3() {
    'hello'
}
assert method3() == 'hello';
```

Groovy 方法的参数类型可以被省略，默认为Object类型  
```groovy
def add(int a, int b) {
    return a + b
}
//与上面的等价
def add(a, b) {
    a + b
}
```


 

- 默认导入包  
Groovy 会默认导入下面这些包、类，不需要使用import语句显式导入  
```groovy
java.io.*
java.lang.*
java.math.BigDecimal
java.math.BigInteger
java.net.*
java.util.*
groovy.lang.*
groovy.util.*

```

- 动态语言的特性，在运行时才能确定数据类型  
在 Groovy 中，调用的方法将在运行时被选择。这种机制被称为运行时分派或多重方法（multi-methods），是根据运行时实参（argument）的类型来选择方法。Java 采用的是相反的策略：编译时根据声明的类型来选择方法。

下面是一个例子，同样的 Java 代码在 Java 和 Groovy 环境中运行结果是不同的；  
```groovy
int method(String arg) {
    return 1;
}
int method(Object arg) {
    return 2;
}

Object o = "Object";
int result = method(o);
// In Java
assertEquals(2, result);
// In Groovy
assertEquals(1, result);
```
产生差异的原因在于，Java 使用静态数据类型，o 被声明为 Object 对象，而 Groovy 会在运行时实际调用方法时进行选择。因为调用的是 String 类型的对象，所以自然调用 String 版本的方法。

- 数组初始化语法  
在groovy中， {...}代码块是留给闭包(Closure)使用的，所以存在一些语法区别  
groovy的def 关键字用来定义动态类型的变量，和python，scala是有区别的  
groovy在使用[]初始化的时候，默认创建的是java的java.util.ArrayList对象   
```groovy
// java
int[] javaArray = {1,2,3}

// groovy 
def groovyArray = [1,2,3]

println groovyArray.class
```

- get/set方法
groovy默认会隐式的创建getter、setter方法，并且会提供带参的构造器，下面两者是等价的  
groovy字段权限默认是public
```groovy
// In Java
public class Person {
    private String name;
    Person(String name) {
        this.name = name
    }
    public String getName() {
        return name
    }
    public void setName(String name) {
        this.name = name
    }
}

// In Groovy
class Person {
    String name
}

def person = new Person(name: '张三')
assert '张三' == person.name
person.name = '李四'
//person.setName('李四')
assert '李四' == person.getName()

```

- 闭包  
Groovy 提供了闭包的支持，语法和 Lambda 表达式有些类似，简单来说就是一段可执行的代码块或函数指针。闭包在 Groovy 中是groovy.lang.Closure类的实例，这使得闭包可以赋值给变量，或者作为参数传递。Groovy 定义闭包的语法很简单，就像下面这样  
闭包可以访问外部变量，而方法（函数）则不能  
```groovy
def str = 'hello'
def closure={
    println str
}
closure()//hello 
```

闭包调用的方式有两种，闭包.call(参数)或者闭包(参数)，在调用的时候可以省略圆括号  
```groovy
def closure = {
    param -> println param
}
 
closure('hello')
closure.call('hello')
closure 'hello'
```

闭包的参数是可选的，如果没有参数的话可以省略->操作符  
```groovy
def closure = {println 'hello'}
closure()
```

多个参数以逗号分隔，参数类型和方法一样可以显式声明也可省略  
```groovy
def closure = { String x, int y ->                                
    println "hey ${x} the value is ${y}"
}
```

如果只有一个参数的话，也可省略参数的定义，Groovy提供了一个隐式的参数it来替代它  
```groovy
def closure = { it -> println it } 
//和上面是等价的
def closure = { println it }   
closure('hello')  
```

闭包可以作为参数传入，闭包作为方法的唯一参数或最后一个参数时可省略括号  
```groovy
def eachLine(lines, closure) {
    for (String line : lines) {
        closure(line)
    }
}

eachLine('a'..'z',{ println it }) 
//可省略括号，与上面等价
eachLine('a'..'z') { println it }
```

- list  
Groovy 定义 List 的方式非常简洁，使用中括号([])，以逗号(,)分隔元素即可。Groovy中的 List 其实就是java.util.List，实现类默认使用的是java.util.ArrayList

```groovy
def numbers = [1, 2, 3]         

assert numbers instanceof List  
assert numbers.class == java.util.ArrayList  
assert numbers.size() == 3 
```

- arrays  
Groovy 定义数组的语法和 List 非常类似，区别在于数组的定义必须指定类型为数组类型，可以直接定义类型或者使用def定义然后通过as关键字来指定其类型  
```groovy
String[] arrStr = ['Ananas', 'Banana', 'Kiwi'] //直接声明类型为数组类型  String[]

assert arrStr instanceof String[]    
assert !(arrStr instanceof List)

def numArr = [1, 2, 3] as int[]     //痛过as关键字指定类型为数组类型 int[] 

assert numArr instanceof int[]       
assert numArr.size() == 3
```

- map  
Groovy 定义 Map 的方式非常简洁，通过中括号包含key、val的形式，key和value以冒号分隔([key:value])。Groovy中的Map其实就是java.util.Map，实现类默认使用的是java.util.LinkedHashMap

```groovy
// key虽然没有加引号，不过Groovy会默认将其转换为字符串
def colors = [red: '#FF0000', green: '#00FF00', blue: '#0000FF']

assert colors['red'] == '#FF0000' // 使用中括号访问
assert colors.green == '#00FF00' // 使用点表达式访问

colors['pink'] = '#FF00FF' // 使用中括号添加元素，相当于Java Map 的 put(key,value)方法
colors.yellow = '#FFFF00'// 使用点表达式添加元素
assert colors.pink == '#FF00FF'
assert colors['yellow'] == '#FFFF00'
assert colors instanceof java.util.LinkedHashMap // 默认使用LinkedHashMap类型

// Groovy Map的key默认语法不支持变量，这里的key时间上是字符串'keyVal'而不是keyVal变量的值'name'
def keyVal = 'name'
def persons = [keyVal: 'Guillaume'] 
assert !persons.containsKey('name')
assert persons.containsKey('keyVal')

//要使用变量作为key，需要使用括号
def keyVal = 'name'
def persons = [(keyVal): 'Guillaume'] 
assert persons.containsKey('name')
assert !persons.containsKey('keyVal')
```

- range  
在 Groovy 中可以使用..操作符来定义一个区间对象，简化范围操作的代码  
```groovy
def range = 0..5
assert (0..5).collect() == [0, 1, 2, 3, 4, 5]
assert (0..<5).collect() == [0, 1, 2, 3, 4] // 相当于左闭右开区间
assert (0..5) instanceof List // Range实际上是List接口的实现
assert (0..5).size() == 6
assert ('a'..'d').collect() == ['a','b','c','d']//也可以是字符类型

//常见使用场景
for (x in 1..10) {
    println x
}

('a'..'z').each {
    println it
}

def age = 25;
switch (age) {
    case 0..17:
        println '未成年'
        break
    case 18..30:
        println '青年'
        break
    case 31..50:
        println '中年'
        break
    default:
        println '老年'
}
```
