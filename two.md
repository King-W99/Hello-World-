# 第二章.Kotlin的基础类型学习

### 2.1 注释

~~~kotlin
/*
*文档注释
*/
fun main(args:Array<String){
  println("你好世界")//输出语句
}
~~~

## 2.2声明变量

~~~Kotlin
fun main(args:Array<String>){
    /**
     * 变量类型分为
     * var 可变类型
     * val 不可变类型相当于常量
     */
    var a:Int=23
    a=45
    val b=12//编译器根据初始化的值确定为Int型
    //b=13  b是常量不允许重新赋值

}
~~~

## 2.3整型

### Kotlin分为4中整型

**Byte 在内存中占8位  Short 在内存中占16位  Int在内存中占32位  Long在内存中占64位**

**一般在开发中首选Int  在Kotlin中整型不是基本数据类型 而是应用数据类型**

**因为Kotlin是null安全的语言，因此Byte，Short，Int，Long变量都不能接受null值，如果要接受就要加上？**

~~~kotlin
var a:Int=null//错误
var b:Int？=null//正确
----------------------------------------------------
fun main(args:Array<String>){
    var a:Int=234
    var b:Int=234
    println(a===b)//true
    var ob1:Int?=245
    var ob2:Int?=245
    println(ob1===ob2)//false 如果是在-128到127是true
    
}
~~~

## 浮点型

如果声明一个变量或常量没有具体指出什么数据类型，只是初始化为浮点数，那么自动判断为Double型

~~~kotlin
fun main(args:Array<String>){
var a=3.12
    println(a.javaClass)//double

}
~~~

注意使用正浮点数除以0.0会得到一个正无穷大的数，反之是负无穷大，0.0除以0.0得到是非数与任何数都不相等

~~~kotlin
fun main(args:Array<String>){
    var a:Float=2.345f
    println("a的值为:${a}")

   //var b:Float=23.23232 因为23.23232默认是double类型,所以编译错误
    var b:Float=2.45f
    println(b/0.0==50000/0.0)//Infinity 正无穷大数  true

    var b1:Float=-2.23f
    println(b1/0.0==-50000/0.0)//-Infinity 负无穷大数 true

    println(0.0/0.0==0.0/0.0)//NaN 非数  false 与任何数都不相等

}
~~~

---

## 数值之间的类型转换

在Kotlin中与java不同不支持范围小的数隐式转换为范围大的数，只支持显示装换

~~~kotlin

fun main(args:Array<String>){
    var a:Byte=23
   // var b:Short=a//错误
    var b:Short=a.toShort()//正确
    println(b)//23
}
----------------------------------
//基于上下代码 Byte+Short=Int，Long+Byte=Long  自动类型提升
fun main(args:Array<String>) {
    var a:Byte=23
    var b:Short=45
    var c=a+b
    println("${a}+${b}=${c}")
    println(c.javaClass)//int型
}
~~~

在Kotlin不支持char型值当整数型值用，但支持整数值当char值用

~~~kotlin
fun main(args:Array<String>) {
   var result=" "
    for (i in 0..5){
        //随机生成一个97-122之间的int正整数
        val intval=(Math.random()*26+97).toInt()
        //将inval墙转为char型拼接到result后面
        result+=intval.toChar()
    }
    println(result)
}
~~~

---

## Boolean类型

~~~kotlin
fun main(args:Array<String){
  var b1:Boolean=true
  var b2:Boolean=false
}


fun main(args:Array<String>) {
    var b1:Boolean=true
    var b2:Boolean=false
    //字符串中"true"和"false"不会直接转换为boolean类型，但boolean类型变量可以插值到字符串中
    var str:String="${b1}代表真"
    println(str)
}
---------------------------
fun main(args:Array<String>) {
    var i=1
//    if (i){
//        println('啊大大')错误
//    }
    if (i==1){
        println("ada")
    }
}
~~~

---

## null安全

Int？只有可空类型的变量才能接受null，Int不能接受null值，可空类型不可以直接调用方法和访问属性

~~~kotlin
fun main(args:Array<String>) {
    var sk:String="skit"
    var sk2:String?="skit"
   // sk=null//错误
      sk2=null//正确
    println(sk.length)
    //println(sk2.length) 可空类型

}
~~~

可空类型要调用方法就必须先判断避免NUE

~~~kotlin
fun main(args:Array<String>) {
    var sk:String?="skit"
    var len=if (sk!=null) println(sk.length)else -1
    println(len)

    sk=null
    if (sk!=null&&sk.length>0){
        println(sk.length)
    }else
        println("空字符串")
}
~~~

对于非空的boolean类型只接受三个值：true，false，null

~~~kotlin
fun main(args:Array<String>) {
    var a:Boolean?=null
   if (a==true){
        println("为真")
    }
  /**
   应为if后面必须是boolean类型但是boolean和boolean？本质是两种不同类型
//    if (a){
//        println("为真")//错误
//    }
}
~~~

**安全调用**

例如 user?.dog?.name 如果user不为空则返回user.name的属性，如果dog也不为空，则获取dog.name

~~~kotlin
fun main(args:Array<String>) {
    var sk:String?="skit"
    println(sk?.length)//4

    sk=null
    println(sk?.length)//null
}
~~~

**强制调用 ！！** 容易发生NPE

~~~kotlin
import com.sun.prism.null3d.NULL3DPipeline

fun main(args:Array<String>) {
    var a:String?="skit"
    println(a!!.length)

    a=null
    println(a!!.length)//NPE
}
~~~



**Elvis运算**

~~~kotlin

fun main(args:Array<String>) {
    var b:String?="skit"
    val len=if (b!=null) println(b.length)else -1
    println(len)//4
    b=null
  /**
  如果"?."左边表达式不为空，则返回左边表达式的值，否则返回右边的
  */
    val len2=b?.length?:-1
    println(len2)//-1
}
~~~



## 字符串

可以通过索引来访问指定索引处的字符

~~~kotlin
fun main(args:Array<String>) {
    var sk="asdfadfafsa"
    println(sk[0])
    //便利
    for (s in sk){
        println(s)
    }
}
~~~

普通字符串 var a ="sdsdsdsdsd"  

~~~kotlin
fun main(args:Array<String>) {
    var str="sfsfsfsfsf"
    print(str.length)
    //原始字符串
    var txt=""" 
        ~阿尔法法无法玩
        ~驱蚊器无群无去
        ~千万千万千万
        ~千万千万千万
    """.trimMargin("~")
    println(txt)
}

~~~

---

## 字符串模板

~~~kotlin
fun main(args:Array<String>){
    var temp="印度阿三"
    var txt=""" 
        ${temp}阿达瓦大无多
        ${temp}阿发我翻翻
        ${temp}阿瓦我凤娃 
    """.trimIndent()
    println(txt)
}
------------------------
fun main(args:Array<String>){
     val bookPrice=79
     //在字符串模板中嵌入变量
    var s="图书的价格为：${bookPrice}"
    println(s)

    val rand=java.util.Random()//创建java Random的对象
    val s2="伪随机数是：${rand.nextInt(10)}"
    println(s2)//9

    val mtstr="fkjava.org"
    println("${mtstr}的长度为${mtstr.length}")


}

~~~

## Kotlin字符串的方法

~~~kotlin
fun main(args:Array<String>){
  //将字符串转换为double
    var a="23.345"
    var a2 =a.toDouble()
    println(a2)
 //将字符串装换为int
    var b="23"
    var b2=b.toInt()
    println(b2)
  
  //将字符串首字母大写
   val str="fssdsds"
    println(str.capitalize())//首字母大写
    println(str.decapitalize())//首字母小写
}
---------------------------------------------
fun main(args:Array<String>){
  //返回两个字符串相同的前和后
    var str2="crazyit.org"
    println(str2.commonPrefixWith("crazyjava.org"))//crazy
    println(str2.commonSuffixWith("fkit.org"))//it.org
    //正则表达式匹配
    var str3="java886"
    //判断str3是否包含3个连续的数字
    println(str3.contains(Regex("\\d{3}")))

}


~~~

##  类型别名：关键字 typealias   typealias 类型别名=已有类型

