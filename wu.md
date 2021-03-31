# 数组与结合

kotlin创建数组大致分为两种

1.使用arrayOf(),arrayOfNulls,emptyArray()工具类

2.使用Array(size:Int,init:(Int)->T)构造器

~~~kotlin
import java.util.*

fun main(args:Array<String>) {
    //相当于java的静态初始化
    var arr1= arrayOf("java","Kotlin","C","C++")
    var arr2=arrayOf(5,23,34,34,34)
    //相当于java的动态初始化
    var arr3= arrayOfNulls<Double>(4)
    var arr4= arrayOfNulls<Int>(5)
    //创建长度为零的空数组
    var arr5= emptyArray<String>()
    var arr6= emptyArray<Int>()
    //创建指定长度，使用Lambda表达式初始化数组元素
    var arr7=Array(5,{(it*2+97).toChar()})
    var arr8=Array(6,{"fkit"})
}
----------------------------------------------------------
fun main(args:Array<String>) {
    //相当于java的静态初始化
    var intarr= intArrayOf(12,23,34,34,45,54,23)
    var douarr= doubleArrayOf(2.3,4.2)
    //创建指定长度，使用Lambda表达式初始化数组元素的数组
    var intarr2= IntArray(5,{it*it})
    var chararr=CharArray(5,{(it*2+97).toChar()})
    println(Arrays.toString(intarr))
    println(Arrays.toString(chararr))
}
~~~

##  使用数组 便利 关键字 ：indices直接返回数组的区间

lastIndex返回数组最后一个元素的索引

withIndex()同时获取数组的索引和元素

~~~kotlin
import com.sun.scenario.effect.impl.sw.sse.SSEBlend_SRC_OUTPeer
import java.util.*

fun main(args:Array<String>) {
  var arr =arrayOf("Kotlin","java","C")
    //两种方式来获取数组元素
    println(arr[0])
    println(arr.get(0))
    //两种方式来修改数组元素
    arr[1]="Kotlin"
    arr.set(2,"kOTLIN")
    println(java.util.Arrays.toString(arr))
}
--------------------------------
//便利数组
fun main(args:Array<String>) {
  //动态创建一个数组并便利
   var arr = arrayOfNulls<Double>(5)
    for (i in 0 until arr.size){
        println(arr[i])//5个null
    }
}
-----------------------------------
fun main(args:Array<String>) {
     var books= arrayOf("清廉阿无法访问","额GV额AV阿eve爱国")
    for (i in books){
        println(i)
    }
}
------------------------------------------

fun main(args:Array<String>) {
   var books= arrayOf("安抚挖法","啊发发飞无法","afeafawfea")
    for (i in books.indices){//直接返回数组的区间  indices
        println(books[i])
    }
  var i=java.util.Random().nextInt(10)//生成10以内的随机数
    println(i in books.indices)//false
  
    println(books.lastIndex)//2  lastIndex返回数组最后一个元素的索引
    println(books.size-1==books.lastIndex)//true
}
---------------------------------------------------
//withIndex()同时获取数组的索引和元素
import com.sun.scenario.effect.impl.sw.sse.SSEBlend_SRC_OUTPeer
import java.util.*

fun main(args:Array<String>) {

        var books= arrayOf("安抚挖法","啊发发飞无法","afeafawfea")
        for ((index,value) in books.withIndex()){
            println("索引为:${index}的元素为:${value}")
        }
    }
索引为:0的元素为:安抚挖法
索引为:1的元素为:啊发发飞无法
索引为:2的元素为:afeafawfea
----------------------------------------------



~~~

## 多维数组

~~~kotlin
fun main(args:Array<String>) {
    //a的数组元素是Array<Int>类型
    //相当于java中动态初始化，初始化了长度为4的一维数组
    var a= arrayOfNulls<Array<Int>>(4)
    for (i in a.indices){
        println(a[i])
    }
    //初始化数组的第一个元素，相当于java的静态初始化
    a[0]= arrayOf(2,5)
    //访问a数组的第一个元素所指数组的第二个元素
    a[0]?.set(1,6)//这里的1是索引，把这个索引位置的数该为6
    //a数组的第一个元素是一个一位数组，遍历这个一维数组
    for (i in a[0]!!.indices){
        println(a[0]?.get(i))
    }
    }

~~~

# Hello-World-
