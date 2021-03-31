# 第四章 流程控

## 顺序结构

4.2 分为if分支和when分支

if分支分为三种

~~~~kotlin
if(expression){
  statements..
}
----------------
if(expression){
  statements..
}else{
   statements..
}
---------------
if(expression){
  statements..
}else{
   statements..
}else if{
  statements..
}
~~~~

注意事项：在使用if else条件语句时，应该优先把范围小的放到前面来处理

~~~kotlin
fun main(args:Array<String>){
    var age=45
    if (age>=60){
        println("老年人")
    }
    if (age>40&&age<60){
        println("中年人")
    }
    if (age>20&&!(age>40))
        println("年轻人")
}

~~~

if表达式

~~~kotlin
fun main(args:Array<String>){
    var age=20
    var str=if (age>20) "age大于20" else if (age<20)
        "age小于20的" else "age等于20"
    println(str)
}

~~~

## when分支

~~~kotlin
fun main(args:Array<String>){
        var svore ='A'
    when(svore) {
        'A' -> println("优秀")
        'B' -> println("良好")
        'C' -> println("寄个")
        else -> println("不及格")
    }
}
----------------------------------
//但when分支包含多条语句时，要用{}将这些语言包含起来，形成代码块
fun main(args:Array<String>){
        var svore ='A'
    when(svore) {
        'A' -> {
                println("优秀")
                println("厉害了")
               }
        'B' -> println("良好")
        'C' -> println("寄个")
        else -> {
                 println("不及格")
                 println("下次加机油")
                }
    }
}
-------------------------------------
//when分支可以匹配多个值，
fun main(args:Array<String>){
        var svore ='A'
    when(svore) {
        'A','B'-> {
            println("优秀")
            println("小伙子真不错")
        }
        'C' -> println("寄个")
        else -> println("不及格")
    }
}
-----------------------------------------
//when分支的值而且不要求是常量，可以是表达式
un main(args:Array<String>){
   var score='A'
   var str="EFGH" 
    when(score){
        str[0]-4,str[1]-4->{
            println("优秀")
        }
        str[2]-4,str[3]-4-> println("中")
        else -> println("咋咋")
    }
}
----------------------------
//when对条件表达式没有任何要求
import java.util.*

fun main(args:Array<String>){
    var data=Date()
    when(data){
        Date()->{
            println("流弊")
            println("when分支真变态")
        }
        else -> println("厉害")
    }
}
------------------------------------
//when分支还可以作为表达式来使用

fun main(args:Array<String>){
    var score ='B'
    var str=when (score){
        'A'->{
            println("厉害了")
        }
        'B'-> println("还可以")
        else -> println("贼特么强")
    }
    println(str)
}
--------------------------------------------


~~~

## when分支处理范围

~~~kotlin
fun main(args:Array<String>){
   val age=java.util.Random().nextInt(100)
    println(age)
    var str=when(age){
        in 10..25->"但是年少青删薄"
        in 26..50->"风景依稀似去年"
        in 51..80->"醉听清吟胜管玄"
        else ->"其他"
    }
    println(str)
}

~~~

## when 分支处理类型

~~~kotlin
import java.util.*

fun main(args:Array<String>){
   var inputPrice=26
    println(realPrice(inputPrice))
}
fun realPrice (inputPrece:Any)=when(inputPrece){
    is String->inputPrece.toDouble()
    is Int->inputPrece.toDouble()
    is Double->inputPrece
    else ->0.0
}
~~~

## While 循环

~~~kotlin
import java.util.*

fun main(args:Array<String>){
        var a=0
    while(a<10){
        println(a)
        a++
    }

}
-------------------------
//do  while
fun main(args:Array<String>){
    var a=1
     do {
         println(a)
         a++
     }while (a<5)

}
~~~

## for in 循环

~~~kotlin
fun main(args:Array<String>){
        var max=7
        var result =1
    for (num in 1..max){
        result*=num
        println(result)

    }
   
}
--------------------
//for in的循环计数器相当于val声明的常量，但对循环计数器赋值时会发生错误
fun main(args:Array<String>){
for (i in 1 until 5){
    println("i:${i}")
    i=20//错误
}

}

~~~

## 控制循环条件

一旦在循环体中遇到break时就结束循环

~~~kotlin
fun main(args:Array<String>){
for (i in 1 until 5){
    println("i:${i}")
    i=20
}

}
//i的值为：0
//i的值为：1
//i的值为：2
---------------------------
fun main(args:Array<String>) {
     //外层循环，outer作为标识符
    outer@ for (i in 0 until 5){
        for (j in 0 until 3){
            println("i的值为:${i},j的值为：${j}")
            if (j==1){
              //跳出outer标识的循环
                break@outer
            }
        }
    }

}
i的值为:0,j的值为：0
i的值为:0,j的值为：1

~~~

## continue

~~~kotlin
//跳过本次循环剩下的语句
fun main(args:Array<String>) {
     for (i in 0..3){
         println("i的值为：${i}")
         if (i==1){
             continue
         }
         println("continue下面的语句")
     }

}
continue下面的语句
i的值为：1
i的值为：2
continue下面的语句
i的值为：3
continue下面的语句
~~~

## return 结束整个函数和方法

# Hello-World-
