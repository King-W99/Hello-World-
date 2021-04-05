## 函数

1.函数入门

定义和调用函数

~~~kotlin
fun 函数名(形参列表)[:返回值类型]{
  //由零条或多条可执行语句组成的函数
}
~~~

2.定义两个函数并调用

~~~kotlin
import kotlin.jvm.internal.Intrinsics

fun max(x:Int,y:Int):Int{
   val z=if (x>y) x else y
    return  z
}
fun sayHi(name:String):String{
    println("===正在执行sayH语句=======")
    return "${name},你好"
}
fun main(args: Array<String>) {
    var a=11
    var b=12
    var vesult=max(a,b)
    println("最大值为：${vesult}")//最大值为：12
    println(sayHi("陶二阁"))//===正在执行sayH语句=======
 													//陶二阁,你好

}
~~~

3.定义无返回值的函数可以Uint代表没有返回值，也可以直接省略，实例如下

~~~kotlin
fun test(){
  println("执行test函数")
}
//定义了一个函数，该函数没有返回值，但是有一个形参
fun test2(name:String)：Uint{
  println("${name}，你好欢迎光临")
}

~~~

## 递归函数

在一个函数体内调用它本身，被称为递归函数

~~~kotlin
import kotlin.jvm.internal.Intrinsics
fun fn(n:Int):Int{
    if (n==0){
        return 1
    }else if (n==1){
        return 4
    }else{
        return 2*fn(n-1)+fn(n-2)
    }
}
fun main(args: Array<String>) {
    //输出f(10)的结果
    println(fn(10))
}
~~~



## 单表达式函数

函数只是返回单个表达式，并在（=）指定函数体就可以

~~~kotlin

fun manji(x:Double,y:Double):Double=x*y
fun main(args: Array<String>) {
    println(manji(1.2,2.3))
}
~~~

函数的形参

~~~kotlin
//定义一个函数
fun grith(width:Double,height:Double):Double{
    println("width:${width}")
    println("height:${height}")
    return 2*(width+height)
}
fun main(args: Array<String>) {
    //使用传统方式，根据位置传入参数
    grith(2.4,5.5)
    //根据形参名来传入参数
    grith(width = 2.5,height = 2.34)
    //根据形参传入，还可以更改位置
    grith(height = 2.33,width = 4.55)
    //部分使用命名参数，部分使用位置参数
    grith(width = 2.3,1.2)
    println( grith(1.23,height = 3.4444444))
}
~~~

形参默认值

形参名：形参类型=默认值

**一般默认参数定义在形参列表最后**

~~~kotlin
fun sayHi(name:String="孙悟空",massge:String="欢迎来到Kotlin世界"){
    println("${name}:你好")
    println("消息是:${massge}")
}
fun main(args: Array<String>) {
    //全部使用默认参数
      sayHi()
    //只对massge使用默认值
      sayHi("白骨精")
    //全不使用默认值
    sayHi("白虎精","欢迎使用Kotlin")
    //只有name参数使用默认值
    sayHi(massge = "Kotin")
}

~~~

## 个数可变的形参

vararg修饰

kotlin允许可变形参在形参列表的任意位置，但是只允许存在一个个数可变的形参

~~~kotlin
fun test(a:Int,vararg books:String){
    for (b in books){
        println(b)
    }
    println(a)
}
fun main(args: Array<String>) {
    test(5,"java","kotlin","c++","c")
      //将数组多个元素传给个数可变的参数
    var arr= arrayOf("kotlin","java","c++")
    test(12,*arr)

}
~~~

## 函数的重载

~~~kotlin
kfun test(){
    println("无参数的函数")
}
fun test(age:Int){
    println("重载有参数的函数")
}
fun test(age:Int,name:String){
    println("重载test函数")
}

fun main(args: Array<String>) {
    //函数重载
    test()
    test(12)
    test(12,"嗷嗷")
}
---------------
fun test(mag:String){
    println("只有一个字符串参数的test()函数：${mag}")
}
fun test(vararg books:String){
    println("***形参可变的test函数*****${books.contentDeepToString()}")
}
fun main(args: Array<String>) {
    test()//***形参可变的test函数*****[]
    test("aa","bb")//***形参可变的test函数*****[aa, bb]
    test("aa")//只有一个字符串参数的test()函数：aa

}
~~~

## 局部函数

kotlin支持在函数体内部定义函数，称为局部函数

在默认情况下，局部函数对外部隐藏，

~~~kotlin
//定义函数该函数的返回值为Int型
fun getMathFunc(type:String,nn:Int):Int{
    //定义一个计算平方的局部函数
    fun square(n:Int):Int{
        return n*n
    }
    //定义一个计算立方的局部函数
    fun cube(n:Int):Int{
        return n*n*n
    }
    //定义一个计算阶乘的局部函数
    fun factorial(n:Int):Int {
        var result = 1
        for  (index in 2..n) {
            result *= index
        }
      return result
    }
    when(type){
        //调用局部函数
        "square"->return square(nn)
        "cube"->return cube(nn)
        else ->return factorial(nn)
    }
}
fun main(args: Array<String>) {
    println(getMathFunc("square",3))//9
    println(getMathFunc("cube",3))//27
    println(getMathFunc("",3))//6
}
~~~

## 高阶函数

Var a=::pow  ::访问函数的引用

将pow函数辅助给a使用

~~~kotlin
//定义一个计算乘方的函数
fun pow(base:Int,exprment:Int):Int{
    var result=1
    for (i in 1..exprment){
        result*=base
    }
    return result
}
//定义一个计算面积的函数
fun test(width:Int,height:Int):Int{
    return width*height
}
fun main(args: Array<String>) {
    var myfun=::pow
//将pow函数赋值给myfun，则myfun可当做pow使用
    println(myfun(3,4))//81
    //将test函数赋值给testfun
    var testfun=::test
    println(testfun(2,3))//6
}
~~~

~~~kotlin
//定义函数类型的形参，其中fn是(Int)->Int类型的形参
fun map(data:Array<Int>,fn:(Int)->Int):Array<Int>{
    var result=Array<Int>(data.size,{0})
    //遍历data数组元素，并用fn函数对data[i]进行计算
    //然后将计算的结果作为新数组的元素
    for (i in data.indices){
        result[i]=fn(data[i])
    }
    return result
}
//定义一个计算平方的函数
fun square(n:Int):Int{
    return n*n
}
//定义一个计算立方的函数
fun cube(n:Int):Int{
    return n*n*n
}
//定义一个计算阶乘的函数
fun  factorial(n:Int):Int{
    var result=1
    for (index in 2..n){
        result*=index
    }
    return result
}
fun main(args: Array<String>) {
    var data= arrayOf(3,4,9,5,8)
    println("原数据${data.contentDeepToString()}")//原数据[3, 4, 9, 5, 8]
    //下面函数代码调用map()函数3次，每次调用时都传入不同的函数
    println("计算数组元素的平方")
    println(map(data,::square).contentToString())//[9, 16, 81, 25, 64]
    println("计算数组元素的立方")
    println(map(data,::cube).contentToString())//[27, 64, 729, 125, 512]
    println("计算数组元素的阶乘")
    println(map(data,::factorial).contentToString())//[6, 24, 362880, 120, 40320]
}
~~~

~~~kotlin
使用函数类型作为返回值
fun getMathFunc(type:String):(Int)->Int{
    //定义一个计算平方的局部函数
    fun p(n:Int):Int{
        return n*n
    }
    //定义一个计算立方的局部函数
    fun l(n:Int):Int{
        return n*n*n
    }
    //定义一个计算乘阶局部函数
    fun fact(n:Int):Int{
        var result=1
        for (i in 2..n){
            result*=i
        }
        return result
    }
    when(type){
        "p"->return ::p
        "l"->return ::l
        else ->return ::fact
    }
}
fun main(args: Array<String>) {
    //调用第一个函数
    var pf=getMathFunc("p")
    println(pf(2))//4
    //调用第2个函数
    var lf=getMathFunc("l")
    println(lf(3))//27
    //调用第3个函数
    var cj=getMathFunc("fact")
    println(cj(5))//120
}
~~~

## 使用lambda表达式来替代局部函数

~~~kotlin
fun getMathFanc(type:String): (Int) -> Int {
    when(type){
        "square"->return {n:Int->
            n*n}
        "cube"->return {n:Int->
            n*n*n}
         else->return {n:Int->
             var result=1
             for (index in 2..n){
                 result*=index
             }
             result
         }
    }
}
fun main(args: Array<String>) {
    var pf=getMathFanc("cube")
    println(pf(5))
    pf=getMathFanc("cube")
    println(pf(3))//27
    pf=getMathFanc("qqq")
    println(pf(5))//120
}
~~~

## lambda表达式的脱离

作为参数传入的lambad表达式可以脱离函数单独使用

~~~kotlin
//定义一个list类型的变量，并将初始化为空的list
var lambadList=java.util.ArrayList<(Int)->Int>()
//定义一个函数，该函数的形参类型为函数

fun collectFn(fn:(Int)->Int){
    //这是将fn传入的参数（或lambda表达式）添加到lamabdlist集合中
    //意味着fn可以在collectFn中范围内使用
    lambadList.add (fn)
}
fun main(args: Array<String>) {
    //调用上面的函数两次
    collectFn { it*it }
    collectFn { it*it*it }
    println(lambadList.size)//2
    //依次调用lambadList集合元素（每个元素都是lambad表达式）
    for (i in lambadList.indices)
    {
        println(lambadList[i](1+10))//1331
    }
}
~~~

## 调用lambda表达式

~~~kotlin
fun main(args: Array<String>) {
    //定义一个lambda表达式，并将它赋值给square变量
    var square={n:Int->
        n*n}
    //使用square调用Lambda表达式
    println(square(5))//25
    println(square(6))//36
    //定义一个lambda表达式，并在它后面添加圆括号来调用该lambda表达式
    var result={base:Int,exponent:Int->
        var result=1
        for (i in 1..exponent){
            result*=base
        }
        result
    }(4,3)
    println(result)//64
}
~~~

## 匿名函数

~~~~kotlin
fun main(args: Array<String>) {
    var test=fun(x:Int,y:Int):Int{
        return x*y
    }
    println(test(2,3))//6
}
~~~~

