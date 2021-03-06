### 关于gradle对于android动态打包的一些配置之productFlavor

****

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

#### 神秘的gradle

自从用上 Android Studio，第一次接触到 gradle ，就感觉这东西很神秘，开始的时候为了开发 android ，简单了解过它，但看得似懂非懂。其实说句实话，本人一直对弱类型语言无感，总感觉少点什么，约束，还是什么？

#### 开始讲故事

故事的开头是这样的

做为半个外包公司，公司为了什么什么的，所以开发了一套“万能”系统，一套系统卖给了N个客户，然后就需要一套配置，接口地址啊，平台ID 啊什么的，到 APP 端还需要，APP 名称，图标，包名...，所以每次打包就痛苦不堪，要这改一下，那改一下，很麻烦，而且经常出错，打错安装包。

这几天正在看关于 gradle 的中文文档，为什么会看“无感”语言的文档呢，说来话长，最近入手了 kpw ，然后能看的免费书就被限制住了，在一个网站上看到这文档，就随手推到了设备上。整体内容是比较乱的，但还是硬着头皮看了下来，就看到了今天要用到的多版本打包，想想这个功能以前是用过的，就是多渠道批量打包，但不知道的是原来 productFlavors 这个对象还可以设置包名等信息。

#### productFlavor

文档说 productFlavor 这个对象类型和 defaultConfig 是一样的，也就是说 defaultConfig 可以配置的参数，它也支持。defaultConfig 可以设置什么？ applicationId 啊，各种 Version 啊等等。

#### 关于包名
这个 applicationId 就是包名，简单说一下 applicationId 与 manifests 中的 package 的区别，在 eclipse 中因为没有 applicationId（估计那时候也没有想到这种解耦的方式），package 担当了 APP 唯一标识和 java 包名的责任，但在 AS 中，package 只用作 java 包名，applicationId 才是 APP 的唯一标识，一个 APP 中二者可以不一致，并且还要注意一点，ContextWrapper（及其子类，如Activity等） 的 getPackageName() 方法返回的是 applicationId ，而不是 package ，即使方法名叫 getPackageName ，然而并不是那样。

#### 具体操作

- 动态配置资源字段值（ strings , styles , colors 等文本格式）

  ```groovy
   productFlavors{
    official{
      resValue "string","app_name","ABC" //动态配置 strings.xml中的 app_name 字段值，这样定义后，需要删除 strings.xml 中对 app_name 的声明定义，二者不能同时存在，可以放心删除 strings.xml 中的声明，不会报找不到R资源的错误（gradle 果然神秘）
    }
  }
  ```

 - 动态配置包名

   ```groovy
   productFlavors{
     official{
       applicationId "com.deanlib.aaa"
     }
   }
   ```

   说一点要注意的地方，包名的修改可能会影响到一些第三方控件的使用，比如第三方的分享，支付等功能，如果应用中没有集成这些功能，修改包名不会有任何副作用。解决上面提到的问题，可以针对不同的包名申请不同的第三方的 key ，以保持 key 与包名对应，这样就引出另一个问题，动态配置 key ，及下面要提到的动态配置变量。

- 动态配置代码中的变量或者资源图片等不能用资源字段值配置的资源

  ```groovy
  productFlavors{
    official{
      manifestPlaceholders = [
        app_icon:"@mipmap/icon",//配置APP图标
        launch_img:"@mipmap/launch",//配置启动图
        server_url:"http://deanlib.com/"//配置接口服务器
      ]
    }
  }
  ```

  manifestPlaceholders 顾名思义是与 manifest 文件有关的，这些动态配置都可以在 manifest 文件中使用或者需要以 manifest 做为桥梁配置在其中，下面对以上三个典型配置的处理作说明。

  - 配置APP图标

    配置APP图标自然是 manifest 文件中，所以直接设置变量即可。

    ```xml
    <application
    	android:icon="${app_icon}"
    	android:roundIcon="${app_icon}"/>

    ```

  - 配置启动图

    由于不能将一张图片像 resValue 方式那样直接设置数据，图片还是需要存放在mipmap或drawable中，通过使用路径对应图片的方式，以达到动态配置效果。

    以 manifest 文件做为桥梁，定义<meta-data>标签

    ```xml
    <meta-data
    	android:name="LAUNCH_IMG"
    	android:resource="${launch_img}"/>
    ```

    <meta-data>标签定义的位置可以在<application>标签，或者<activity>标签，或者<service>标签，再或者<receiver>标签中，定义在不同标签只是会影响在代码中取值所使用的API方法。

    下面介绍代码中的取值，以<meta-data>标签定义在<application>标签中为例

    ```java
    ImageView image = (ImageView)findViewById(R.id.image);
    int launch = getPackageManager().getApplicationInfo(getPackageName(), PackageManager.GET_META_DATA).metaData.getInt("LAUNCH_IMG");
    image.setImageResource(launch);
    ```

  - 配置接口服务器

    同配置启动图的方式定义<meta-data>标签

    ```xml
    <meta-data
    	android:name="SERVER_URL"
    	android:value="${server_url}"/>
    ```

    然后通过代码取值

    ```java
    Constants.serverUrl = getPackageManager().getApplicationInfo(getPackageName(), PackageManager.GET_META_DATA).metaData.getString("SERVER_URL");
    ```

    这里要说一下配置启动图与配置接口服务器有什么区别，为什么要分别演示？区别是有的，正是因为小小的区别栽过跟头，所以才分别写了出来。

    如果需要配置的是一个数值，可以以字符串形式，或者数值形式声明定义

    ```xml
    manifestPlaceholders = [
          num:123
    ]
    ```

    或者

    ```xml
    manifestPlaceholders = [
          num:"123"
    ]
    ```

    manifest 中

    ```xml
    <meta-data
    	android:name="NUM"
    	android:value="${num}"/>
    ```

    取值时使用 getInt() 都是可以拿到值的

    ```java
    int num = getPackageManager().getApplicationInfo(getPackageName(), PackageManager.GET_META_DATA).metaData.getInt("NUM");
    ```

    虽然资源也是对应在R文件中的数值类型，用上述逻辑取值却取不到任何值，因为在 manifestPlaceholders 中声明定义是 “@mipmap/launch” 这样的形式，所以定义<meta-data>标签时，需要注意一点，使用 android:resource 而不是 android:value 。

    ```xml
    <meta-data
    	android:name="LAUNCH_IMG"
    	android:resource="${launch_img}"/>
    ```

    取值使用getInt()方法，因为资源本来就是数值类型（在 android 中）。

    **Tip:** 动态配置代码中的变量还可以采用 BuildConfig 的方式，及我们使用 buildConfigField 配置时，BuildConfig 就会生成我们配置的字段（自动）

    ```groovy
    buildConfigField "boolean","isDebug","true"
    ```

    参数分别是 类型，名称，值，全部都是字符串类型，比如上面声明一个布尔类型的 isDebug，值为 true，或者声明一个数值类型：

    ```groovy
    buildConfigField "int","count","1"
    ```

    或者声明一个字符串类型，注意看：

    ```groovy
    buildConfigField "String","name","\"Dean\""
    ```

    看到不同的地方了吗？字符串声明的双引号是不能省略的。

    buildConfigField 可以配置于 productFlavors 中也可以配置于 buildTypes 中，

    ```groovy
    buildTypes {
        release {
            buildConfigField "boolean","isDebug","false"
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
        debug{
            buildConfigField "boolean","isDebug","true"
        }
    }
    ```

    或者

    ```groovy
    productFlavors{
       official{
           buildConfigField "boolean","isDebug","false"
       }
    }
    ```

    代码中使用

    ```java
    public static final boolean DEBUG = BuildConfig.isDebug;
    ```

    其实吧，这字段声明的一点意义都没有[流汗]

    如果点进去看看BuildConfig这个类的话，系统已经自动生成 DEBUG 字段

    ```java
    public static final boolean DEBUG = Boolean.parseBoolean("true");
    ```

    默认情况下，debug 包 DEBUG = true ，release 包 DEBUG = false。

#### 一个完整的例子

1. app 的 build.gradle 

   ```groovy
   apply plugin: 'com.android.application'

   android{
     compileSdkVersion 25
     buildToolsVersion '26.0.2'
     
     defaultConfig{
       applicationId "com.deanlib.aaa"
       minSdkVersion 15
       targetSdkVersion 19
       versionCode 1
       versionName "1.0"
     }
     
     productFlavors{
       //官方版本
       official{
         applicationId "com.deanlib.aaa.official"
         resValue "string","app_name","aaa官方版"
         manifestPlaceholders = [
           app_icon:"@mipmap/icon",//配置APP图标
           launch_img:"@mipmap/launch",//配置启动图
           server_url:"http://aaa.deanlib.com/official/"//配置接口服务器
         ]
       }
       //免费版本
       free{
         applicationId "com.deanlib.aaa.free"
         resValue "string","app_name","aaa免费版"
         manifestPlaceholders = [
           app_icon:"@mipmap/icon_free",//配置APP图标
           launch_img:"@mipmap/launch_free",//配置启动图
           server_url:"http://aaa.deanlib.com/free/"//配置接口服务器
         ]
       }
     }
   }
   ```

   ​

2. manifest 文件

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <manifest xmlns:android="http://schemas.android.com/apk/res/android"
       xmlns:tools="http://schemas.android.com/tools"
       package="com.deanlib.aaa">
     	<application
           android:name=".app.App"
           android:allowBackup="true"
           android:label="@string/app_name"
           android:icon="${app_icon}"
           android:roundIcon="${app_icon}"
           android:supportsRtl="true"
           android:hardwareAccelerated="true"
           android:theme="@style/AppTheme.NoAppBar">
   		<meta-data
   			android:name="SERVER_URL"
   			android:value="${server_url}"/>
         	<meta-data
   			android:name="LAUNCH_IMG"
   			android:resource="${launch_img}"/>
       </application>
   </manifest>
   ```

   ​

3. 代码动态取值

   ```java
   public class App extends Application {
     @Override
       public void onCreate() {
           super.onCreate();
         	OotbConfig.init(this,true);
         	try{
               //动态设置接口地址
               Constants.server = getPackageManager().getApplicationInfo(getPackageName(), PackageManager.GET_META_DATA).metaData.getString("SERVER_URL");
           }catch (Exception e){
               e.printStackTrace();
           }
         	OotbConfig.setRequestServer(Constants.server,null,new UserResultCode("200"),new DefaultLoadingDialog());
       }
   }
   ```

   ```java
   public class LaunchActivity extends BaseActivity {
      @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
         	ImageView image = (ImageView)findViewById(R.id.image);
           try {
   			int launch = getPackageManager().getApplicationInfo(getPackageName(), PackageManager.GET_META_DATA).metaData.getInt("LAUNCH_IMG");
   			image.setImageResource(launch);
           } catch (Exception e) {
               e.printStackTrace();
           }
       }
   }
   ```

   最后说明一个操作，当在开发时，使用AS的 Run app 时，AS会编译运行配置中的一个版本，至于是哪个版本，默认情况下，AS2.0和AS3.0也不尽相同，所以一般情况都要手动指定，AS的设置位置在界面的左下侧边栏的 Build Variants 中，对于要运行的App 选择相应版本。

   ----

   2017/12/21.

   *Dean.King*

   **Beijing**


