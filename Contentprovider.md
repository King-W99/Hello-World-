# 安卓学习之八--Contentprovider

## ContentProvider简介

ContentProvider主要用与在不同用于程序之间实现数据共享功能，允许一个程序访问另一个程序的数据，同时还能保护被访问数据的安全性，ContentProvider是Android实现跨程序共享数据的标准方式，在此之前我们还要学习Android运行时权限。

## 运行时权限

Android是在6.0系统中引入的运行时权限的功能，为了刚好的保护用户的安全和隐私，这个权限设计思路，就是用户认可你所申请的程序，就会安装，不认可就拒绝安装。

引入这个功能后，就是用户可以在安装程序时不用一次性授权所有权限，而是在需要的时候对某一项权限单独授权。

### 常用权限分类

**安卓把常用权限分为两种，一类是普通权限,一类是危险权限**

普通权限：就是不会威胁到用户的隐私和安全的权限，对于这部分的授权系统会自动为我们申请。

威胁权限：就是威胁到用户的隐私和安全，比如获取用户的联系人消息和定位功能，这就需要用户手动授权。

下面就列出到Android 10系统为止部分威胁权限

![截屏2021-05-14 下午4.34.41](/Users/king/Library/Application Support/typora-user-images/截屏2021-05-14 下午4.34.41.png)

这个表网上都用，但用到一个权限，去查一下，如果要进行运行时权限的处理，需要到AndroidManifest.xml文件添加一个权限声明就可以了

**注意，表格中每一个危险权限都属于一个权限组，我们进行运行时权限处理时使用的是权限名，原则上同意了某个权限申请，那么同组的其它权限都会自动授权。**

## 在程序运行时申请权限

下面实例就用CALL_PHONE来学习，这个权限是编写拨打电话功能时候声明，因为拨打电话会涉及电话资费问题，所以也被列为危险权限。

首先我们新键一个项目名为Runtimepermission项目，修改antivity_man_xml布局,在这里面增加一个按钮，后面我们通过点击按钮来触发拨打电话逻辑

~~~kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   
    android:layout_width="match_parent"
    android:layout_height="match_parent"
      android:orientation="vertical">

    <Button
        android:id="@+id/makCall"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Make Call"/>
</LinearLayout>
~~~

接着修改ManActivity代码

当然为了稳妥我们把操作放到异常捕获里面

~~~kotlin
package com.example.runtimepermission

import android.content.Intent
import android.net.Uri
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.Button

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val makecall: Button =findViewById(R.id.makCall)
        makecall.setOnClickListener { 
            try {
                //这里构建了一个隐士的Intetn，action指向Intent.ACTION_CALL
                // 这是一个系统内置的打电话动作   
                val  intent=Intent(Intent.ACTION_CALL)
                
                //在data执行协议是tel
                intent.data= Uri.parse("tel:10086")
                startActivity(intent)
            }catch (e:SecurityException){
                e.printStackTrace()
            }
           
        }
    }
}
~~~

接下来在AndroidManifest.xml,申明权限

~~~kotlin
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.runtimepermission">
//申明权限
<uses-permission android:name="android.permission.CALL_PHONE"/>

~~~

这个如果是在安卓6.0之前正常运行，但是在之后就不行了，我们来看日志

![截屏2021-05-14 下午5.39.43](/Users/king/Library/Application Support/typora-user-images/截屏2021-05-14 下午5.39.43.png)

这个 Permission Denial可以看出，适用于权限静止所导致的问题，因为在Android6.0以后所有危险权限必须在运行时进行处理

接下来修改ManAcitity代码

~~~kotlin
package com.example.runtimepermission

import android.content.Intent
import android.content.pm.PackageManager
import android.net.Uri
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.Button
import android.widget.Toast
import androidx.core.app.ActivityCompat
import androidx.core.content.ContextCompat
import java.util.jar.Manifest

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val makecall: Button = findViewById(R.id.makCall)
        makecall.setOnClickListener {
            //第一步先判断用户是否已经给我们授权了
            if (ContextCompat.checkSelfPermission(
                    this,
                    //第二个参数是具体的权限名这里使用返回值和PackageManager.PERMISSION_GRANTED做比较
                    //相等就已经授权，反之没有
                    android.Manifest.permission.CALL_PHONE
                ) != PackageManager.PERMISSION_GRANTED
            ) {
                //如果没有授权就调用这个方法授权，第一个参数Activity实例，
                //第二个String数组，把申请的权限名放进去
                //第三个参数，请求码，只要是唯一值就可以
                ActivityCompat.requestPermissions(
                    this,
                    arrayOf(android.Manifest.permission.CALL_COMPANION_APP), 1
                )
            } else {
                call()
            }
        }
    }
  //调用完requestPermissions()方法后，不管结果如何都会回调onRequestPermissionsResult()方法中
  //授权结果封装到 grantResults参数里
  //我们只要判断一下授权结果，如果同意就call，不同意就弹出提示消息
    override fun onRequestPermissionsResult(
        requestCode: Int,
        permissions: Array<out String>,
        grantResults: IntArray
    ) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults)
      when(requestCode){
          1->{
              if (grantResults.isNotEmpty()&&grantResults[0]== PackageManager.PERMISSION_GRANTED){
                  return call()
              }else{
                  Toast.makeText(this,"You denied the permission",Toast.LENGTH_SHORT).show()
              }
          }
      }
    }

    private fun call(){
        try {
            //这里构建了一个隐士的Intetn，action指向Intent.ACTION_CALL
            // 这是一个系统内置的打电话动作
            val  intent=Intent(Intent.ACTION_CALL)

            //在data执行协议是tel
            intent.data= Uri.parse("tel:10086")
            startActivity(intent)
        }catch (e:SecurityException) {
            e.printStackTrace()
        }
    }
}
~~~

最后运行程序，点击“Make call"按钮，用于用户没有授权拨打电话权限，第一次运行就会弹出权限申请的对话框，我们可以同意或拒绝。

## ContentResolver基本用法

对于一个应用程序，想要访问 ContentResolver共享数据，就一定借助ContentResolver类，可以通过context 的get方法获取

### 读取系统联系人

首先在模拟器兴建两个联系人

代码如下

~~~~kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
   >

   <ListView
       android:id="@+id/contactsView"
       android:layout_width="match_parent"
       android:layout_height="match_parent">

   </ListView>

</LinearLayout>
~~~~

~~~kotlin
package com.example.cantactstest

import android.content.pm.PackageManager
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.provider.ContactsContract
import android.widget.ArrayAdapter
import android.widget.ListView
import android.widget.Toast
import androidx.core.app.ActivityCompat
import androidx.core.content.ContextCompat
import java.util.jar.Manifest

class MainActivity : AppCompatActivity() {
    private val contactsList=ArrayList<String>()
    private lateinit var  adapter:ArrayAdapter<String>

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        adapter= ArrayAdapter(this,android.R.layout.simple_list_item_1,contactsList)
        //获取布局id
        val contacetView:ListView=findViewById(R.id.contactsView)
        contacetView.adapter=adapter
        //这里判断是否用户已经同意权限
       if(ContextCompat.checkSelfPermission(this,android.Manifest.permission.READ_CONTACTS)!=
               PackageManager.PERMISSION_GRANTED){
           ActivityCompat.requestPermissions(this, arrayOf(android.Manifest.permission.READ_CONTACTS),1)
       }else{
        readContacts()
       }

    }
    //回调，处理返回的结果
    override fun onRequestPermissionsResult(
        requestCode: Int,
        permissions: Array<out String>,
        grantResults: IntArray
    ) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults)
        when(requestCode){
            1 ->{
                if (grantResults.isNotEmpty()&&grantResults[0]!=PackageManager.PERMISSION_GRANTED)
                    readContacts()
            }else ->{
            Toast.makeText(this, "You denide thr permission", Toast.LENGTH_SHORT).show()

        }
        }
    }

    private  fun readContacts(){
        //查询联系人消息
        contentResolver.query(ContactsContract.CommonDataKinds.Phone.CONTENT_URI,null,null,null,
        null)?.apply {
            while(moveToNext()){
                //获取联系人的姓名
                val displayName=getString(getColumnIndex(ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME))
                //获取联系人手机号
               val number=getString(getColumnIndex(ContactsContract.CommonDataKinds.Phone.NUMBER))
               contactsList.add("$displayName\n$number")

            }
            adapter.notifyDataSetChanged()
            close()
        }
    }


}





~~~

