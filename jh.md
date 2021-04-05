## Map集合的学习使用

kotlin中使用to来指定Key-value值，to之前是key，之后是value

~~~kotlin
fun main(args: Array<String>) {
    //Map集合申明和使用
    /**
     * 提供了mapOf来创建集合，不可变的可接受0个或者多个Key-value对
     * mutableMapOf()返回可变的MutableMap集合，可接受0个或多个
     * hashMapOf()返回可变的HashMap集合，可接受0个或多个
     * linkedMapOf()....
     *
     */
    var map = mapOf("java" to 86, "kotlin" to 92, "go" to 78)
    println(map)//按添加顺序输出
    //创建可变得集合
    var mutablemap = mutableMapOf("java" to 86, "kotlin" to 92, "go" to 78)
    println(mutablemap)//按添加顺序输出
    println("map返回对象的实际类型:${map.javaClass}")//class java.util.LinkedHashMap
    println("mutablemap返回对象的实际类型为:${mutablemap.javaClass}")//class java.util.LinkedHashMap
    //创建hashmap集合
     var hashmap= hashMapOf("java" to 86,"kotlin" to 92, "go" to 78)
    println(hashmap)//不按添加顺序输入
   //创建linkedmap集合
    var linkedmap= linkedMapOf("java" to 86, "kotlin" to 92, "go" to 78)
    //创建sortedmapof
    var sortedmap= sortedMapOf("java" to 86,"kotlin" to 92, "go" to 78)
    println(sortedmap)//按从小到大顺序排序
}
~~~

## 使用Map方法

~~~kotlin
fun main(args: Array<String>) {
    //使用Map方法
    //创建不可变的集合返回的是Map
    var map= mapOf("java" to 86,"kotlin"  to  92,"go" to 76)
    //判断所有key-value对的key的长度是否到大于4，value到大于80
    println(map.all ({it.key.length>4&&it.value>80}))//输出false
    //判断任意key-value对的key的长度是否到大于4，value到大于80
    println(map.any({it.key.length>4&&it.value>80}))//true
    //由于有contains方法可以用in ！in运算符、
    println("java" in map)//true
    println("c" in map)//false



    //对map所有集合元素过yu,要求key包含li
    val filteredMap=map.filter({"li " in  it.key})
    println(filteredMap)//输出是{}
    //将每个key-value对映射成新值，返回所有新值组成map集合
    val mappedlist=map.map({"<<疯狂${it.key}讲义>>价格为${it.value}"})
    println(mappedlist)//[<<疯狂java讲义>>价格为86, <<疯狂kotlin讲义>>价格为92, <<疯狂go讲义>>价格为76]
    //获取Key最大值
    println(map.maxBy({it.key}))//kotlin=92

    //根据value获取最小值
    println(map.minBy({it.value}))//go=76

    var bmap= mapOf("lua" to 67,"erlang" to 73, "kotlin"  to 92)
    //获取并值
    println(map+bmap)//{java=86, kotlin=92, go=76, lua=67, erlang=73}

    //集合相减，减去公共元素
    println(map-bmap)//{java=86, kotlin=92, go=76}
}
~~~

## 遍历Map

遍历的方法1.通过[]运算通过key值来获取value，2.通过for in来遍历

~~~kotlin
fun main(args: Array<String>) {
    var map= mapOf("java" to 86,"kotlin"  to  92,"go" to 76)
    for (en in map.entries) {
        println("${en.key}->${en.value}")//java->86  kotlin->92   go->76
    }
    //先遍历map中的key，再通过key来获取
        for (key in map.keys){
            println("${key}->${map[key]}")
        }
    //通过for in来遍历
    for ((key,value) in map){
        println("${key}->${value}")
    }
}
~~~

## 可变Map

~~~kotlin
import kotlin.jvm.internal.Intrinsics

fun main(args: Array<String>) {
//创建可变的mutablemap
    var mutable= mutableMapOf("java" to 12,"kotlin" to 13,"go" to "121" )
    //以[]语法放入Key-value对
    mutable["qwqw"]=2342
    //以put方法放入key-value语法对
    mutable.put("qqq",1212121)
    //remove删除key-value对
    mutable.remove("go")
    println(mutable)
    //删除所有key-value对
     mutable.clear()
    println(mutable.size)
}
~~~

