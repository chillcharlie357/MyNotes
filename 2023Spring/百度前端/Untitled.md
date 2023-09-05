[🚀 Why you need ViewModels and why you don't](https://www.composables.com/tutorials/viewmodels-in-jetpack-compose)


# gradle更换阿里云镜像

[公共代理库](https://help.aliyun.com/document_detail/102512.html?spm=a2c40.aliyun_maven_repo.0.0.36183054Rfi7Xz)

[如何在Maven私有仓库设置公共代理库\_云效-阿里云帮助中心](https://help.aliyun.com/document_detail/153736.html?spm=a2c4g.150040.0.i3)

## `.kts`

找到类似文件：`build.gradle.kts` `settings.gradle.kts`

```kotlin
buildscript {  
    repositories {  
        maven("https://maven.aliyun.com/repository/public/")  
        maven("https://maven.aliyun.com/repository/spring/")  
        mavenLocal()  
    }  
}
```

```kotlin
pluginManagement {  
  repositories {  
    maven("https://maven.aliyun.com/repository/public/")  
    maven("https://maven.aliyun.com/repository/spring/")  
    mavenLocal()  
    gradlePluginPortal()  
    google()  
    mavenCentral()  
  }  
}
```
## `.gradle`

找到类似文件：`build.gradle`、`setting.gradle`

```groovy
pluginManagement {  
    repositories {  
        maven {  
            url 'https://maven.aliyun.com/repository/public/'  
        }  
        maven {  
            url 'https://maven.aliyun.com/repository/spring/'  
        }  
        mavenLocal()  
        google()  
        mavenCentral()  
        gradlePluginPortal()  
    }}  
dependencyResolutionManagement {  
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)  
    repositories {  
        maven {  
            url 'https://maven.aliyun.com/repository/public/'  
        }  
        maven {  
            url 'https://maven.aliyun.com/repository/spring/'  
        }  
        mavenLocal()  
        google()  
        mavenCentral()  
    }}
```

# 天气api(gen by AI)

[天气api](https://poe.com/s/qohVLzcGUb5Chy31PCkU)


[按钮上显示天气信息](https://poe.com/s/PugcvBygRNWN9f8EFGTL)


[Conversation with Assistant on Poe](https://poe.com/s/2JVdqXFFHe6magFS0GRv)


# 自定义返回导航

[提供自定义返回导航  |  Android 开发者  |  Android Developers](https://developer.android.com/guide/navigation/custom-back?hl=zh-cn)


