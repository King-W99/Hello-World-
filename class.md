[toc]



## 类与对象

open是final反义词，使用open一个类，方法，属性，表明该类可派生子类，方法和属性可被重写

~~~kotlin
//空类:没有类体，可省越花括号{} 
class Empty
-----------------
import java.net.ContentHandler

class Peston{
    var name:String=""
    var age:Int=0
    //定义一个say方法
    fun  say(content:String){
        println(content)
    }
}
fun main(args: Array<String>) {
     var p=Peston()
        p.name="韬哥"
        p.age=12
        p.say("韬哥最厉害")
    println(p.name)
    println(p.age)
}
~~~

~~~kotlin
class Dog{
    fun jump()
    {
        println("正在执行jump方法")
    }
    //定义一个run()的方法，run()方法需要借助jump方法
    fun run()
    {
        this.jump()//this可以不写
        jump()
    }
}

~~~

使用this作为方法的返回值，可以使代码更简洁，但容易造成实际意义的模糊

~~~kotlin
class ReturnThis{
    var age=0
    fun grow():ReturnThis{
        age++
        return this
    }
}

fun main(args: Array<String>) {
    val rt=ReturnThis()
      rt.grow()
              .grow()
              .grow()
    println(rt.age)//3
}
~~~

方法与函数的关系

如果要获取某个方法的应用方法，同样需要在方法名前面添加两个冒号：：

~~~kotlin
class Dog{
    fun run(){
        println("run方法")
    }
    fun eat(food:String){
        println("正在吃："+food)
    }
}

fun main(args: Array<String>) {
    var rn:(Dog)->Unit=Dog::run
    val d=Dog()
    rn(d)
    var et=Dog::eat
    et(d,"肉骨头")

}
~~~

## 中缀表示法

关键字infix infix方法只能有一个参数

~~~kotlin
class ApplePack(weight:Double){
    var weight=weight
    override fun toString(): String {
        return "Applepack[weight=${this.weight}]"
    }
}
class Apple(weight: Double){
    var weight=weight
    override fun toString(): String {
        return "Applepack[weight=${this.weight}]"
    }
    //定义中缀方法，使用infix修饰
    infix fun add(other:Apple):ApplePack{
         return ApplePack((this.weight+other.weight))
    }
    //定义中缀方法，使用infix修饰
    infix fun drop(other: Apple):Apple{
        this.weight-=other.weight
        return this
    }
}

fun main(args: Array<String>) {
    var origin=Apple(3.4)
    //使用add方法
    val ap=origin add Apple(2.4)
    println(ap)
    origin drop Apple(2.4)
    println(origin)//1.0
}
~~~

## componentN

kotlin中允许将一个对象的N个属性“结构”给多个变量

var (name,pass)=user

var name=user.component1()

var pass=user.component2()



~~~kotlin
//如果不想要用前面某个属性可以用_来代替
var(_,pass2,ag22)=user
---------------------------
import com.sun.jdi.connect.Connector

class User(name:String, pass:String, age:Int){
    var name=name
    var pass=pass
    var age=age
    //定义operator修饰的compinentN方法，用于解构
    operator fun component1():String{
        return this.name
    }
    operator fun  component2():String{
        return this.pass
    }
    operator fun component3():Int{
        return this.age
    }
}
fun main(args:Array<String>){
    //创建User对象
    val user=User("yeeku","668899",29)
   //只利用user对象的componment1()和compent2()方法
    var(name,pass)=user
    println(name)
    println(pass)
    //利用user对象的三个方法
    var(name2,pass2,age)=user
    println(name2)
    println(pass2)
    println(age)


}
~~~

## 数据类和返回多个值得函数

数据类：需要使用data修饰，主构造器所有参数必须使用var，val来修饰

数据类不能使用abstract，open，sealed修饰，也不能定义类部类

定义数据类后自动生成内容有

1.生成equals()/hashCode()方法

2.自动重写toString方法

3.为每个属性自动生成operator修饰的componentN方法

4.生成copy()方法，用于完成对象的复制



~~~kotlin
//定义一个数据类
data class Result(val result:Int,val status:String)
fun factorial(n:Int):Result{
    if (n==1){
        return  Result(1,"成功")
    }else if (n>1){
        return Result(factorial(n-1).result*n,"成功")
    }else{
        return Result(-1,"参数必须大于0")
    }

}
fun main(args: Array<String>) {
    //通过解析获取函数的返回值
    var(rt,status)=factorial(6)
    println(rt)
    println(status)

    var result=Result(2,"未知结果")
    //调用copy()方法完成复制
    val oldRt=result.copy()
    println(oldRt)
}
}
~~~



## lambda表达式的结构

一下两种写法是一致的

~~~kotlin
map.mapvalues{entry->"${entry.value}!"}
//使用结构，将entry解构成（key，value)
map.mapvalues{key,value->"$value"}
//如果解构参数中一个组件未使用，那么可以代替为下划线
map.maoValues{(_,value)->"$value!"}
~~~

## 属性和字段

kotlin中定义一个属性，相当于java中private修饰的field，以及public，final修饰的getter，setter方法

~~~kotlin
class Adder{
    var street:String=""
    var citry=""
    var province=" "
    var postCot:String?=null
}

fun main(args: Array<String>) {
    var a=Adder()
    //使用点语发对属性赋值相当于Setter
    a.street = "大纲"
    a.citry="集"
    //使用点语法访问属性相当于getter方法
    println(a.street)
    println(a.citry)

}
~~~

val声明只读属性

~~~kotlin
class Item(barcode:String,name:String,price:Double){
    val barcode=barcode
    val name=name
    val price=price
}

fun main(args: Array<String>) {
    val vm=Item("按时","阿凡达",3.45)
    println(vm.barcode)
    println(vm.name)
    println(vm.price)
}
~~~

## 自定义getter和setter方法无需使用fun来修饰

~~~kotlin
import com.sun.tools.internal.xjc.reader.xmlschema.bindinfo.BIConversion

class Uesr(first:String, last:String){
    var first:String=first
    var last:String=last
    val fullName:String
    //自定义gett方法
    get(){
        println("执行fullName的getter方法")
        return "${first}.${last}"
    }
}

fun main(args: Array<String>) {
    var user =Uesr("悟空","孙")
    println(user.fullName)
}
~~~

~~~~kotlin
import com.sun.tools.internal.xjc.reader.xmlschema.bindinfo.BIConversion

class Uesr2(first:String, last:String){
    var first:String=first
    var last:String=last
    var fullName
    //使用单表达式定义getter方法的方法体
    get()="${first}.${last}"
    set(value){
        println("执行fullName的sette方法")
        //value字符串不包含"."或包含几个"."都不行
        if ("." !in value||value.indexOf(".")!=value.lastIndexOf(".")){
            println("你输入的fullName不合法")
        }else {
            var tokens=value.split(".")
            first=tokens[0]
            last=tokens[1]
        }

    }
}

fun main(args: Array<String>) {
    var u =Uesr2("悟空","孙")
     //使用点属性来赋值
    u.fullName="八戒.猪"
    println(u.first)
    println(u.last)
}
~~~~

## 幕后字段

关键字field

~~~kotlin
class Person(name:String,age:Int) {
    //使用private修饰属性，就这些属性隐藏起来
    var name = name
        set(newName) {
            //执行合理性校验，要求用户名必须在2到6之间
            if (newName.length > 6 || newName.length < 2) {
                println("你设置的人名不合理")

            } else {
                field = newName
            }
        }
    var age = age
        set(newAage) {
            //执行合理性校验，要求用户年龄必须在0到100之间
            if (newAage > 100 || newAage < 0) {
                println("你设置的年龄不合法")
            } else
                field = newAage
        }
}

fun main(args: Array<String>) {
    var a=Person("aaa",12)
    a.age=101//赋值非法，赋值失败
    println(a.age)
    a.age=30//赋值成功,合法
    println(a.age)
}

~~~

## 幕后属性

幕后属性都是使用private修饰的属性，也就是幕后属性

~~~~kotlin
class BackingProperty(name:String){
    //定义private修饰的属性，该属性是幕后属性
    private var _name:String=name
    var name
    //重写getter方法，返回幕后属性的值
    get() =_name
    set(newName){
        //2-6
        if (newName.length>6||newName.length<2){
            println("不合理")
        }
        else{
            //对幕后属性赋值
            _name=newName
        }
    }
}

fun main(args: Array<String>) {
    var p=BackingProperty("aa")
    //访问p.name实际会转成访问其幕后属性
    println(p.name)
    //对p.name赋值等于对幕后属性_name赋值
    p.name="bbb"
    println(p.name)
}
~~~~

## 延迟初始化属性

关键字 lateinit

1.只能修饰在类体中声明的可变属性（使用val声明的属性不行）

2.它修饰属性不能有自定义getter和setter方法

3.修饰属性必须是非空的

~~~~kotlin
import com.sun.tools.internal.xjc.reader.xmlschema.bindinfo.BIConversion
import java.util.*

class Uest{
    //延迟初始化属性
     lateinit var name:String
    lateinit var birch:Date
}

fun main(args: Array<String>) {
     var uesr=Uest()
    uesr.name="aaaa"
     println(uesr.name)
  //  println(user.birch)
//    uesr.name="小悟空"
//    uesr.birch= Date()
//    println(uesr.name)
//    println(uesr.birch)
}
~~~~

## 内联属性

关键字inline修饰符可修饰没有幕后字段的属性

~~~~kotlin
import java.util.*

class Name(name:String, desc:String){
    var name=name
    var desc=desc

}
class Product{
    var productor:String?=null
    //inline修饰的getter方法，表明读取属性时会内联话
     val proName:Name
    inline get()=Name("疯狂的kotlin讲义","最系统的kotlin书")
     //inline修饰属性setter方法，表面设置属性时会内连化
        var author:Name
         get()=Name("李工","无")
    inline set(v){
        this.productor=v.name
    }
    //inline修饰属性本身，表面读取和设置属性时会内莲化
    inline var pubHouse:Name
    get()=Name("电子工业","无")
    set(v){
        this.pubHouse=v.name
    }
}
~~~~

## 隐藏与封装

包与导包

~~~kotlin
 意思是把test函数和Foo类都放到lee包下
package lee
fun test1(){
    println("简单的test函数")
}
class Foo(name:String){
    var name=name

}

fun main(args: Array<String>) {
    println("带包的主函数")
}
~~~

在kotlin中如果没有显示指定创先修饰符默认为public 

~~~kotlin
//文件名example.kt
package lee

private fun  foo(){}//仅在example.kt内可访问
internal fun  test(){}//该函数在相同模块可以反问

public var a:Int=5//任何都可以
private set//setter仅在example.kt可反问

internal val v=6//该属性在相同模块可以反问
~~~

如果重写一个protected成员，且没有显示指定访问权限修饰符，那么该成员依然是protected修饰

~~~kotlin
open class Outer{
    private val a=1
    protected open val b=2
    internal  val c=3
    val d=4//默认权限是public

    protected class Nested{
        public val e:Int=5
    }
}
class Subclass :Outer(){
    //a不可访问
    //b c d可以访问
    //Nested和e可访问
    override val b=5//被重写的b依然是pritected访问权限
}
class Other(o:Outer){
    //o.a,o.b不可访问
    //o.c与Other在同一个模块中，可以被访问
    //o.d可以反问
    //Outher.Nested不可访问，Nested：：e不可反问
}open class Outer{
    private val a=1
    protected open val b=2
    internal  val c=3
    val d=4//默认权限是public

    protected class Nested{
        public val e:Int=5
    }
}
class Subclass :Outer(){
    //a不可访问
    //b c d可以访问
    //Nested和e可访问
    override val b=5//被重写的b依然是pritected访问权限
}
class Other(o:Outer){
    //o.a,o.b不可访问
    //o.c与Other在同一个模块中，可以被访问
    //o.d可以反问
    //Outher.Nested不可访问，Nested：：e不可反问
}open class Outer{
    private val a=1
    protected open val b=2
    internal  val c=3
    val d=4//默认权限是public

    protected class Nested{
        public val e:Int=5
    }
}
class Subclass :Outer(){
    //a不可访问
    //b c d可以访问
    //Nested和e可访问
    override val b=5//被重写的b依然是pritected访问权限
}
class Other(o:Outer){
    //o.a,o.b不可访问
    //o.c与Other在同一个模块中，可以被访问
    //o.d可以反问
    //Outher.Nested不可访问，Nested：：e不可反问
}open class Outer{
    private val a=1
    protected open val b=2
    internal  val c=3
    val d=4//默认权限是public

    protected class Nested{
        public val e:Int=5
    }
}
class Subclass :Outer(){
    //a不可访问
    //b c d可以访问
    //Nested和e可访问
    override val b=5//被重写的b依然是pritected访问权限
}
class Other(o:Outer){
    //o.a,o.b不可访问
    //o.c与Other在同一个模块中，可以被访问
    //o.d可以反问
    //Outher.Nested不可访问，Nested：：e不可反问
}
~~~



## 构造器

关键字constructor

初始化代码块init{

}

```kotlin
//如果构造方法没有任何注解和可见性关键字修饰constructor可以省略
class sest constructor(name:String){
    private val name=name.toUpperCase()
    init {//初始化代码块给参数赋初值
        println(name)
    }
}
```

如果一个类中定义多个普通初始化块，则先定义先执行

~~~~kotlin
class Person(name:String){
    //下面定义一个初始化代码块
     init{
        var a=6
        if (a>4){
            println("Person初始化值：局部变量a的值大于4")
        }
        println("Person初始化代码块")
        println("name的参数为：${name}")

    }
    init {
        println("第二个初始化代码块")
    }
}

fun main(args: Array<String>) {
    Person("素悟空")
}
~~~~

初始化块对类的自定义进行初始化操作

~~~kotlin
//提供了主构造器，该函数包含两个参数
class ConstructorTest(name:String,count:Int){
    var name:String
    var count:Int
    init {
        this.name=name
        this.count=count
    }

}

fun main(args: Array<String>) {
    var tc= ConstructorTest("疯狂的Kotlin讲义",9000000)
    println(tc.name)
    println(tc.count)
}
~~~

## 次构造器和构造器的重载

koutlin允许使用constructor关键字定义n个次构造器

构造器重载

系统会根据传入的实参来判断调用那个构造器

**注意：不管调用那个构造器系统都会调用初始化块**

**这个类没有定义主构造器，所以次构造器需要委托主构造器**

~~~~kotlin
class constructorTest{
    var name:String?
    var count:Int
    init {
        println("初始化代码块")
    }
    //提供无参数的次构造器
    constructor(){
        name=null
        count=0
    }
    //有参次构造器
    constructor(name:String,count:Int){
        this.name=name
        this.count=count
    }
}

fun main(args: Array<String>) {
    //通过无参数的构造器来创建coustructorTest对象
      var oc1= constructorTest()
    //通过有参数的构造器来创建coustructorTest对象   
      var oc2=constructorTest("涛哥",23)
  
    println("oc1:${oc1.name},oc1:${oc1.count}")//oc1:null,oc1:0
    println("oc2:${oc2.name},oc2:${oc2.count}")//oc2:涛哥,oc2:23
}
//初始化代码块
//初始化代码块
//oc1:null,oc1:0
//oc2:涛哥,oc2:23
~~~~

~~~kotlin
import com.sun.tools.internal.xjc.reader.xmlschema.bindinfo.BIConversion
import java.lang.management.LockInfo

//定义主构造器
class Uest(name:String){
    var name:String
    var age:Int
    var info:String?=null
    init {
        println("Uest初始化块")
        this.name=name
        this.age=0
    }
    //委托给主构造器
    constructor(name: String,age: Int):this(name){
        this.age=age
    }
    //委托给（String,Int)构造器
    constructor(name: String,age:Int,info: String):this(name,age){
        this.info=info
    }
}

fun main(args: Array<String>) {
    //调用主构造器
    var us1=Uest("熏悟空")
    println("${us1.name}->${us1.age}->${us1.info}")
    //调用（String，Int）构造器
    var us2=Uest("老(• •)",23)
    println("${us2.name}->${us2.age}->${us2.info}")
    //调用（String，age，info)构造器
    var us3=Uest("无赖",24,"无法")
    println("${us3.name}->${us3.age}->${us3.info}->${us3.info}")

}
//Uest初始化块
//熏悟空->0->null
//Uest初始化块
//老(• •)->23->null
//Uest初始化块
//无赖->24->无法->无法
~~~

## 主构造器声明属性

kotlin中允许给主构造器声明属性。直接在参数是声明var，val来声明

~~~kotlin
class Person(var name:String,val age:Int){
   
}
---------------------------------------------------
//使用主构造器声明属性
class Customer(val name:String="匿名",
var addr:String="天河"){

}

fun main(args: Array<String>) {
    var vt=Customer("熏悟空","划过上")
    println(vt.name)//熏悟空
    println(vt.addr)//划过上
    //调用默认
    var mu=Customer()
    println(mu.name)//匿名
    println(mu.addr)//天河
}
~~~

## 类的继承

语法

~~~kotlin
//koutlin子类继承父类的语法格式·
修饰符 class SubClass:SuperClass{
  //类定义部分
}
//定义父类
open class BaseClass{
  
}
class SubClass :BaseClass{
  
}
~~~

## 子类的主构造器

 如果子类定义主构造器，因此主构造器必须在继承父类同时委托调用父类的构造器

~~~kotlin
open class BaseClass{
    var name:String
    constructor(name:String){
        this.name=name
    }
}
//子类有默认主构造器，声明是委托调用父类构造器
class suberClass:BaseClass("fo"){
    
}
//子类显示主构造器，声明是委托调用父类构造器
class subClass2(name: String):BaseClass(name){
    
}
~~~

##  子类的次构造器

**次构造器调用同样也要委托

1.this参数显示调用本类的重载构造器，最终也会调用父类构造器

2.super参数，系统将会执行子类构造器之前，隐士调用父类无参数的构造器

**koutlin中所有子类构造器都必须调用父类构造器一次**

~~~kotlin
open class Base{
    constructor(){
        println("base无参构造器")
    }
    constructor(name:String){
        println("Base的带有一个String参数：${name}的构造器")
    }
}
class sub:Base{
    //构造器没有显示委托
    //因此这次构造器隐士调用父类无参构造器
    constructor(){
        println("sub无参数的构造器")
    }
    //显示调用父类
    constructor(name:String):super(name ){
        println("sub的String构造器，String的参数为：${name}")
    }
    //构造器用this（name）显示委托本类带String参数构造器
    constructor(name: String,age:Int):this(name){
        println("sub的string,int构造器，int参数为：${age}")
    }
}

fun main(args: Array<String>) {
    sub()
    sub("Sub")
    sub("子类",23)
}
~~~

