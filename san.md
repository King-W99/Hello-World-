# 第三章 运算符和表达式

## 与java相同的运算符

但是Kotlin不支持三目运算符，用if代替了

## 单目前缀运算符:有 +，-，！ 

~~~~kotlin
+a      a.unaryPlus()
-a      a.unaryMinus()
!a      a.not()
fun main(args:Array<String>){
   var a=20
    //使用运算符
   var b=-a
    //调用方法
    val c=a.unaryMinus()
    println("b:${b},c:${c}")//b:-20,c:-20

    val flag=true
    val notflag=!flag
    val notflag2=flag.not()
    println("notflag:${notflag},notflag2:${notflag2}")//false

}

~~~~

## 自加自减运算符

~~~kotlin
a++     a.inc()
a--     a.dec()
fun main(args:Array<String>){
  var a=12
   a++
   println(a)
    //大致相当于a++
    a=a.inc()
    println(a)
    
   var b=13
   b--
    println(b)
    b=b.dec()
    println(b)
}

~~~

##  双目算术运算符

~~~kotlin

fun main(args:Array<String>){
//    a+b     a.plus(b)
//    a-b     a.minus(b)
//    a*b     a.times(b)
//    a/b     a.div(b)
//    a%b     a.rem(b)
//    a..b    a.rangeTo(b) 
    var a=23
    var b=1
    var c=a.plus(b)
    println(c)//a+b

    var c2=a.minus(b)
    println(c2)//a-b

    var c3=a.times(b)
    println(c3)//a*b

    var c4=a.div(b)
    println(c4)//a/b
}
~~~

## in 和!in运算符

~~~kotlin
fun main(args:Array<String>){
  //a in b  b.contatins(a)
  //a !in b  !b.minus(a)
  var str="fkjava.org"
    //调用String方法contatins
    println(str.contains("java"))//true
    //使用in运算符判断
    println("java" in str)//true

    val array= arrayOf(24,45,100,-3,30)
    println(array.contains(100))//true
    //使用in运算符判断
    println(100 in array)//true
}

~~~

## 索引访问运算符

~~~kotlin
a[i]    a.get(i)
a[i,j]  a.get(i,j)
a[i_1,...,i_n]  a.get(i_1,...,i_n)
a[i]=b     a.set(i,b)
a[i,j]=b   a.set(i,j,b)
a[i_1,...,i_n]=b  a.get(i_1,...,i_n,b)

fun main(args:Array<String>){
  var str="fuckjava"
    println(str.get(2))
    println(str[2])

    //创建java的ArrayList的集合
    var list=java.util.ArrayList<String>()
    list.add("java")
    list.add("Kotlin")
    list.add("go")
    println(list[0])
    println(list[1])
}

~~~

## 相等和不相等运算符

~~~kotlin
//=== 才会要求s1，s2指向同一个对象
fun main(args:Array<String>){
    var s1=java.lang.String("java")
    var s2=java.lang.String("java")
    println(s1==s2)//true 相当于java的equals
    println(s1===s2)//false
}

~~~

## 比较运算符

~~~kotlin
a>b      a.compareTo(b)>0
对String Data两个类的实例大小比较
fun main(args:Array<String>) {
    var s1="ada"
    var s2="asdfafda"
    println(s1>s2)//false
    println(s1.compareTo(s2)>0)//false

    var data=java.util.Date()
    var data2=java.util.Date(System.currentTimeMillis()-1000)
    println(data>data2)//true
    println(data.compareTo(data2)>0)
}

~~~

## 区间运算符

a..b 相当于[a,b]

a until b=[a,b)

~~~kotlin
fun main(args:Array<String>) {
    var a=2..6//[2,6]
    for (a1 in a){
        println("${a1}*5=${a1*5}")
    }
}
-------------------------------------

fun main(args:Array<String>) {
    var books= arrayOf("Swift","Kotlin","C","C++")
    for (index in 0 until books.size){
        println("第${index+1}的语言是：${books[index]}")
    }
}

第1的语言是：Swift
第2的语言是：Kotlin
第3的语言是：C
第4的语言是：C++
//反向区间 downTO

fun main(args:Array<String>) {
    var b= 6 downTo 2
    for (b1 in b){
        println("${b1}*5=${b1*5}")
    }
}

6*5=30
5*5=25
4*5=20
3*5=15
2*5=10
------------------------
步长 step
for（ num in 7 downTo 2 step 2)

~~~

# Hello-World-
