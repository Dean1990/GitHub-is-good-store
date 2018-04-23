### 关于gradle对于android动态打包的一些配置之buildTypes

****

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

#### BuildTypes

构建类型，AndroidStudio的Gradle组件默认提供了“debug”“release”两个默认配置，此处用于配置是否需要混淆、是否可调试等 

当然除上面提到的两种，还允许自定义配置，但这并不是常事。

#### 配置

- buildConfigField

  可以配置debug和release不同的变量，比如服务器地址等等，配置参考[《关于gradle对于android动态打包的一些配置之productFlavor》](http://deanlib.com/index.php/archives/4/)

- minifyEnabled

  一般情况下，AS会自动生成一个 release的配置，其中就有 minifyEnabled false 

  是否开启混淆，debug与release的默认值都为false。

- proguardFiles

  这个配置应该于上面的 minifyEnabled 配合使用，用于指定混淆规则，一般默认生成“proguard-rules.pro”规则配置文件，在工程app目录下。

  ```groovy
  proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
  ```

- zipAlignEnabled

  是否对齐资源，用于优化程序，但需要指定签名信息 signingConfig 配置

- signingConfig
  指定signingConfigs的信息

  ```groovy
  signingConfig signingConfigs.release
  ```

  **Tip:** signingConfigs 与 buildTypes 同级，都是在android{}内配置

  ```groovy
  signingConfigs {
      debug {
          storeFile file("keystore.jks")//项目app目录为根目录
          storePassword "123456"
          keyAlias "key"
          keyPassword "123456"
      }
      release {
          storeFile file("keystore.jks")
          storePassword "123456"
          keyAlias "key"
          keyPassword "123456"
      }
  }
  ```

- shrinkResources

  是否移除无用的资源文件

- debuggable

  是否开启DEBUG，默认情况 debug包为true，release包为false

  还记得在[《关于gradle对于android动态打包的一些配置之productFlavor》](http://deanlib.com/index.php/archives/4/)文章中提到自定义BuildConfig的属性isDebug时，提到系统自动生成DEBUG属性，正是由此配置的。

#### 举个栗子

```groovy
buildTypes {
    debug {
        //配置BuildConfig变量
        buildConfigField "String","SERVICE_URL","\"http://deanlib.com/debugservice/\""
        minifyEnabled false//是否混淆
        zipAlignEnabled false//是否资源对齐
        shrinkResources false//是否移除无用资源文件
        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        signingConfig signingConfigs.debug//签名
		debuggable false//是否开启debug
    }
    release {
        buildConfigField "String","SERVICE_URL","\"http://deanlib.com/service/\""
        minifyEnabled true
        zipAlignEnabled true
        shrinkResources true
        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        signingConfig signingConfigs.release
    }
}
```



----

   2018/4/9.

   *Dean.King*

   **Beijing**