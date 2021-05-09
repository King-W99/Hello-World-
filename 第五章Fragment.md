## 安卓学习--六Fragment

Fragment是一种可以嵌入在Acitity的UI片段，它可以让屏幕拥有更加合理的屏幕空间，一般在平板上运用的更加多，它拥有自己的生命周期，感觉就像一个小型的Activity。

## Fragment的简单用法

在一个Activity中添加两个Fragmet，并让这两个Fragment平分屏幕空间

首先新建一个左侧Fragmet布局left_fragment_xml,这里只放置一个按钮

~~~kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:text="button"/>

</LinearLayout>
~~~

新建一个右侧Fragment布局叫right_fragment_xml,一个文本框

~~~Kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="#00ff00">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:textSize="24sp"
        android:text="This is aright Fragment"/>
</LinearLayout>
~~~

然后分别新建LeftFragmet和RightFragment两个类继承Fragment，并且重写onCreateView()方法

~~~kotlin
package com.example.fragmenttest

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment

class LeftFragment:Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        return inflater.inflate(R.layout.left_fragment,container,false)
    }
}
~~~

~~~Kotlin
package com.example.fragmenttest

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment

class LeftFragment:Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        return inflater.inflate(R.layout.right_fragment,container,false)
    }
}
~~~

最后在man.xml<fragment>标签中引入Fragment布局

~~~kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
  <fragment
      android:id="@+id/leftFrag"
      android:name="com.example.fragmenttest.LeftFragment"
      android:layout_width="0dp"
      android:layout_height="match_parent"
      android:layout_weight="1"/>


    <fragment
        android:id="@+id/rightFrag"
        android:name="com.example.fragmenttest.RightFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1"/>




</LinearLayout>
~~~

最后运行

<img src="/Users/king/Library/Application Support/typora-user-images/截屏2021-05-09 下午7.23.16.png" alt="截屏2021-05-09 下午7.23.16" style="zoom:50%;" />

## 动态添加Fragment

意思就是在运行程序时候动态添加Fragment

首先我们新建一个要添加得Fragment叫anther_rigt_fragment.xml

这里只是把颜色改为黄色背景

~~~kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#ffff00">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:textSize="24sp"
        android:text="This is anther right fragment"/>
</LinearLayout>
~~~

下一步也是一样新建一个AotherRightFragment类继承Fragment，重写onCreateView()方法

~~~kotlin
kage com.example.fragmenttest

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment

class AntherRightFrogment :Fragment() {
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        return inflater.inflate(R.layout.anther_right_fragement,container,false)
    }
}
~~~

然后修改man.xml代码，引入FrameLayout布局，把右边的Fragment布局替换

~~~kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
  <fragment
      android:id="@+id/leftFrag"
      android:name="com.example.fragmenttest.LeftFragment"
      android:layout_width="0dp"
      android:layout_height="match_parent"
      android:layout_weight="1"/>

   <FrameLayout
       android:id="@+id/rightFragment"
       android:layout_width="0dp"
       android:layout_height="match_parent"
        android:layout_weight="1"
       />

</LinearLayout>
~~~

最后我们修改ManActivity的代码，为button设置监听器，达到点击BUTTON按钮，更换Fragment

~~~kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val button:Button=findViewById(R.id.button)
        button.setOnClickListener{
            repalce(AntherRightFrogment())
        }
        repalce(RightFragment())
    }
     private fun repalce(fragment:Fragment){
      //就是获取所在fragment的父容器的管理器，
        val fragementManager=supportFragmentManager
      //开启一个事务
        val transaction=fragementManager.beginTransaction()
       //添加和替换Fragment
        transaction.replace(R.id.rightFragment,fragment)
        //返回栈
        transaction.addToBackStack(null)
       //提交事务
        transaction.commit()

    }
~~~

### 在Fragment中实现返回栈

按下back建返回上一个Fragment

~~~~kotlin
        //返回栈
        transaction.addToBackStack(null)
~~~~

### Fragment生命周期

onAttach()：Fragment和Activity相关联时调用

onCreate()：系统创建Fragment时调用

onCreateView()：创建Fragment的布局（视图）调用

onActivityCreated()：确保与Fragment相关联的Activity调用完时调用

首先我们修改RightFragment代码来看效果

~~~kotlin
package com.example.fragmenttest

import android.content.Context
import android.os.Bundle
import android.util.Log
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment

class RightFragment :Fragment(){
    companion object{
        const val TAG="RightFragment"
    }
    //当Fragment和Activity建立关联时调用
    override fun onAttach(context: Context) {
        super.onAttach(context)
        Log.d(TAG,"onAttach")
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        Log.d(TAG,"onCteate")
    }
    //为Fragment创建视图获加载布局时调用
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        Log.d(TAG,"onCreateView")
        return  inflater.inflate(R.layout.right_fragment,container,false)
    }
    //确保和Fragment相关联的Activity已经创建完毕时候调用
    override fun onActivityCreated(savedInstanceState: Bundle?) {
        super.onActivityCreated(savedInstanceState)

        Log.d(TAG,"onActivityCreated")
    }

    override fun onStart() {
        super.onStart()
        Log.d(TAG,"onStart")
    }

    override fun onResume() {
        super.onResume()
        Log.d(TAG,"onResume")
    }

    override fun onPause() {
        super.onPause()
        Log.d(TAG,"onPause")
    }

    override fun onStop() {
        super.onStop()
        Log.d(TAG,"onStop")
    }
     //当与Fragment先关联的视图移除时候调用
    override fun onDestroyView() {
        super.onDestroyView()
        Log.d(TAG,"onDestroyView")
    }

    override fun onDestroy() {
        super.onDestroy()
        Log.d(TAG,"onDestroy")
    }
   //当Fragment与Activity解除关联时候调用
    override fun onDetach() {
        super.onDetach()
        Log.d(TAG,"onDetach")
    }

}
~~~

首次加载RightFragment

<img src="/Users/king/Library/Application Support/typora-user-images/截屏2021-05-09 下午7.36.41.png" alt="截屏2021-05-09 下午7.36.41" style="zoom:50%;" />

按下Button按钮

![截屏2021-05-09 下午7.37.53](/Users/king/Library/Application Support/typora-user-images/截屏2021-05-09 下午7.37.53.png)

按下Back键返回

<img src="/Users/king/Library/Application Support/typora-user-images/截屏2021-05-09 下午7.38.47.png" alt="截屏2021-05-09 下午7.38.47" style="zoom:50%;" />

再次按下Back键

<img src="/Users/king/Library/Application Support/typora-user-images/截屏2021-05-09 下午7.39.51.png" alt="截屏2021-05-09 下午7.39.51" style="zoom:50%;" />

### 使用限定符

这个作用比较大例如一般平板使用双页符，因为屏幕比较大任性，但是我们手机就不同了屏幕空间小，

只能显示一页，那么怎么才能让运行程序的程序自动判断到底用那页了，这时候就使用我们的限定符(qualifier)实现。

首先我们修改activity.main.xml代码，只留下我们左边的Fragment，就是带一个button按钮的，把它就用做我们手机的单页显示。

​    修改  **android:layout_width="match_parent**让它宽度和父布局一样占满空间 

~~~kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
  <fragment
      android:id="@+id/leftFrag"
      android:name="com.example.fragmenttest.LeftFragment"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:layout_weight="1"/>



</LinearLayout>
~~~

接着在res目录下新建一个layout-large文件夹，然后在这个文件夹下添加一个新的activity_main.xml布局，代码就是前面双页布局，这里只修改屏幕占比，这里large就是一个限定符，认为是large的设备就加载这个个layout-large文件夹布局。

~~~kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
     android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment
        android:id="@+id/leftFrag"
        android:name="com.example.fragmenttest.LeftFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1"/>
    <fragment
        android:id="@+id/rightFrage"
        android:name="com.example.fragmenttest.RightFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="3"/>

</LinearLayout>
~~~

最后运行代码在平板上

<img src="/Users/king/Library/Application Support/typora-user-images/截屏2021-05-09 下午8.32.58.png" alt="截屏2021-05-09 下午8.32.58" style="zoom:50%;" />

在手机上

<img src="/Users/king/Library/Application Support/typora-user-images/截屏2021-05-09 下午8.34.54.png" alt="截屏2021-05-09 下午8.34.54" style="zoom:50%;" />

### 最小宽度限定符

large解决了单双页的问题，那么这个large到底多大了，这里我们映入了最小宽度限定符，它允许我们为屏幕指定一个最小值以dp为单位，超过这个最小值，则加载一个布局，那么小于则执行另外一个。

首先我们在res目录下兴建一个layout_sw600文件夹，再次兴建activity_main.xml布局

这里的600dp就是一个临界点，代码都一样就是把两个布局加载

运行的结果，当然看你运行的屏幕了，但宽度大于600dp就加载layout_sw600目录下的activity_main.xml布局，小与600dp就加载默认的，就是layout目录下activity_main.xml布局。