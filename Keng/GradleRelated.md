gradle 配置国内镜像

flutter也要这样



```
buildscript {
    repositories {
        maven { url 'https://maven.aliyun.com/repository/google/' }
        maven { url 'https://maven.aliyun.com/repository/jcenter/'}
        maven { url 'https://maven.aliyun.com/repository/gradle-plugin' }
        maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.2.3'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }        
}

allprojects {
    repositories {
        maven { url 'https://maven.aliyun.com/repository/google/' }
        maven { url 'https://maven.aliyun.com/repository/jcenter/'}
    }
}
```

https://blog.csdn.net/lj402159806/article/details/78422953

