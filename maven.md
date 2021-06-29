# Maven

## 安装

### Ubuntu

```sh
apt update
apt install maven

mvn version
```



### 配置maven源

切换maven源为aliyun source

```sh
cp /etc/maven/settings.xml /root/.m2/

vim /root/.m2/settings.xml
```

在 `<mirrors> </mirros>`中添加以下内容

```xml
    <mirror>
      <id>aliyun-central</id>
      <mirrorOf>central</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>https://maven.aliyun.com/repository/central</url>
    </mirror>
    <mirror>
      <id>aliyun-public</id>
      <mirrorOf>public</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>https://maven.aliyun.com/repository/public</url>
    </mirror>
    <mirror>
      <id>aliyun-google</id>
      <mirrorOf>google</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>https://maven.aliyun.com/repository/google</url>
    </mirror>

```



## 使用maven编译java/scala项目

使用以下命令生成mvn项目模板，可以基于该模板添加项目代码

```sh
mvn archetype:generate -B \
	-DarchetypeGroupId=net.alchim31.maven \
    -DarchetypeArtifactId=scala-archetype-simple \
    -DarchetypeVersion=1.7 \
    -DgroupId=com.zn \
    -DartifactId=spark-sample \
    -Dversion=1.0-SNAPSHOT
```

其中需要自定义修改有两处：

- `com.zn`为organization也通常作为package，
- `spark-sample`为项目名称

执行成功后的目录结构如下：(当前只有scala目录，需要手动添加java目录)

```sh
spark-sample/
├── pom.xml
└── src
    ├── main
    │   └── scala
    │       └── com
    │           └── zn
    │               └── App.scala
    └── test
        └── scala
            └── samples
                ├── junit.scala
                ├── scalatest.scala
                └── specs.scala
```

添加java目录结构，添加后的目录结构为

```sh
spark-sample/
├── pom.xml
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com
│   │   │       └── zn
│   │   │           └── Hello.java
│   │   └── scala
│   │       └── com
│   │           └── zn
│   │               ├── App.scala
│   └── test
│       └── scala
│           └── samples
│               ├── junit.scala
│               ├── scalatest.scala
│               └── specs.scala

```

修改`pom.xml`文件，

删除`<sourceDirectory>`以及`<testSourceDirectory>`：

```sh
 <build>
    <sourceDirectory>src/main/scala</sourceDirectory>
    <testSourceDirectory>src/test/scala</testSourceDirectory>
 </build>
```

并修改为：添加`<resources>`

```
  <build>
    <resources>
      <resource>
        <directory>src/main/scala</directory>
        <filtering>true</filtering>
      </resource>
      <resource>
        <directory>src/main/java</directory>
        <filtering>true</filtering>
      </resource>
    </resources>
  </build>

```

**注意：`pom.xml`文件中的相关版本信息记得修改为自己的版本。**

`pom.xml`文件配置完成后，就可以在项目根目录下执行

```sh
mvn package
```

