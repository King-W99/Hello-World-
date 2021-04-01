# Kotlin集合

**kotlin集合分为两大类1.可变集合：增，删，改。2.不可变集合只能读取元素**

**声明创建Set集合方法**

**1.setOf():该函数返回不可变的Set集合,该函数可接受0个或多个参数**

**2.mutableSetOf():该函数返回可变的mutableset可变集合，该函数可接受0个或多个参数**

**3.hashSetOf():该函数返回可变的hashSet集合**

**4.linkedSetOf():该函数返回可变的linkedSet集合**

**5.sortedSetOf():可变的**

~~~kotlin
import com.sun.scenario.effect.impl.sw.sse.SSEBlend_SRC_OUTPeer
import java.util.*

fun main(args:Array<String>) {
     //创建不可变的集合
    var set= setOf("java","korlin","go")
    println(set)//集合元素按添加顺序来排序
    //创建不可变的集合
    var noset= mutableSetOf("java","Kotlin","go")
    println(noset)//集合元素按添加顺序来排序
    println("setof返回对象的类型是：${set.javaClass}")//class java.util.LinkedHashSet
    println("mutableSteOf返回对象的实际类型：${noset.javaClass}")//class java.util.LinkedHashSet
    //创建hashSet集合
    var hashSet= hashSetOf("java","Kotlin","go")
    println(hashSet)//不保证元素的顺序
    //创建linkedhashSet集合
    var linkedSedOf=linkedSetOf("java","Kotlin","go")
    println(linkedSedOf)//集合按添加顺序来排序
    //创建chreeSet集合
    var chreeSet= sortedSetOf("java","kotlin","go")//注意大小写统一排序时
    println(chreeSet)//元素由小到大排序
    }

~~~

---

## 使用Set方法

## 总结：返回删除集合前面元素的方法 set.dorp(元素位数)

##          对Set集合元素过滤例如包含li的 set.filter({"li" in it})

##           查找包含li元素，有就返回，无就返null: se.find("li" in it)

##          查找某个元素出现的位置：set.indexOf("go")

##          将每个集合元素折合成新值： set.map({<<""+it+"">>})

##         获取最大值：set.max(), 最小值：set.min()

##          反转：set.reversed

~~~kotlin
    fun main(args:Array<String>) {
        //创建不可变集合
        var set= setOf("java","kotlin","go")
        //判断所有元素的长度是否大于4
        println(set.all({ it.length>4 }))//false
        //判断任意元素的长度是否大于4
        println(set.any({it.length>4}))//true
        //以Lambda表达式的值为Key,集合元素为value，组成Map集合
        val map=set.associateBy ({"<<疯狂"+it+"讲义"})
        println(map)
        //由于有contains方法，所以可以用in，!in运算符
        println("java" in set)//true
        println("go" !in set)//false
        //返回并删除Set集合前面两个元素后的集合
        val dropedList=set.drop(2)
        println(dropedList)//输出go
        //对Set集合元素进行过滤，要求集合元素包含li
        val filteredList=set.filter { "li" in it }
        println(filteredList)//[kotlin]
        //查找Set集合包含li的元素，如果找到就返回该元素，否则返回null
        val foundSre1=set.find { "li" in it }
        println(foundSre1)//kotlin
        //查找Set集合中包含gang的元素，找到就返回该元素，否则返回null
        val foundStr2=set.find { "gang" in it }
        println(foundStr2)//null
        //将Set集合中所有字符串拼接起来
        val foundStr3=set.fold("",{acc,e->acc+e})
        println(foundStr3)
        //查找某个元素出现的位置
        println(set.indexOf("go"))//2
        //将每个集合元素映射成新值，返回所有新值组成的Set集合
        val mappedList=set.map ({"疯狂"+it+"讲义"})
        println(mappedList)//[疯狂java讲义, 疯狂kotlin讲义, 疯狂go讲义]
        //获取最大值
        println(set.max())//kotlin
        //获取最小值
        println(set.min())//go
        //反转集合顺序
        val reverrsedList=set.reversed()//倒转
        println(reverrsedList)
        //计算两个集合的交集
        var bSet= setOf("lua","erlang","kotlin")
        println(set intersect bSet)//[kotlin]
        //计算两个集合的并集
        println(set union bSet)//[java, kotlin, go, lua, erlang]
        //集合相加相当于并集
        println(set+bSet)//[java, kotlin, go, lua, erlang]
        //集合相减，减去他们公共的元素
        println(set-bSet)//[java, go]
    }



~~~

## 遍历Set

~~~kotlin
    fun main(args:Array<String>) {
      var books= setOf("疯狂Androide讲义","疯狂的ios讲义","疯狂的Kotlin讲义")
            for (book in books){
                    println(book)
            }
         //使用forEach来遍历集合
            books.forEach({ println(it)})
         //根据索引来遍历集合
            for (i in books.indices){
                    println(books.elementAt(i))
            }
  }


~~~

## 可变的Set添加元素：add(element :E)

或者addAll(elements:Collection<e>)来添加多个元素

~~~kotlin
fun main(args:Array<String>) {
      //定义一个可变的元素
            var langage= mutableSetOf("swift")
            langage.add("go")
            langage.add("java")
            println(langage.count())
            langage.addAll(setOf("li","aq"))
            println(langage.count())
            println(langage)//[swift, go, java, li, aq]
    }


~~~



## 删除元素

remove()删除指定元素

removeAll()批量删除元素

retainAll()只保留共有的元素

clear()清空集合 

~~~kotlin
    fun main(args:Array<String>) {
      var lange= mutableSetOf("kotlin","oc","php","peal","aaa","bbb")
     //删除php
        lange.remove("php")
     //再次删除oc
        lange.remove("oc")
        println(lange)//[kotlin, peal, aaa, bbb]
        //批量删除多个元素
        lange.removeAll(setOf("aaa","bbb"))
        println(lange)//[kotlin, peal]
        //清空集合
        lange.clear();
        println(lange.count())//0
    }


~~~







## List集合

声明与创建集合

listOf()该函数不返回不可变的List集合,可接受0个或多个参数

lsitOfNotNull():该函数不返回不可变的List集合

mutableListOf(),返回可变的MutableList集合

~~~kotlin
fun main(args:Array<String>) {
        //创建不可变得集合，返回LIst
        var la= listOf("aa","ggg")
        println(la)//集合包含null值
        //创建不可变得集合，返回LIst
        var la2= listOfNotNull("aaaa","awaaa")
        println(la2)//不包含null值
        //创建可变的集合
        var la3= mutableListOf("aaa","vvvvvvv")
        println(la3)
            //创建arraylist集合
        var la4=arrayListOf("adaa","adadfaawf")
        println(la4)
        
    }


~~~

# Hello-World-
