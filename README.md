## Android
为您的移动应用提供了Android sdk , 开始使用有以下步骤
## 1.集成SDK

<aside>
💡 目前支持Android API 级别 15 及更高级别的应用程序 、模拟器，手动集成

</aside>

### 手动集成

- 点击此下方下载sdk

[RiskAssessment.rar](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b393d6f5-3882-4e09-bf49-930b51e0af6e/RiskAssessment.rar)

- 将`aar`文件添加到您的 libs 文件夹中。
- 在您的应用程序中`build.gradle`，添加以下代码

```java
android {
	........
	sourceSets {
        main {
            jniLibs.srcDirs = ['libs']
        }
   }
}

dependencies {
    implementation files('libs/RiskAssessment.aar')
}
```

- 将项目与 gradle 文件同步
