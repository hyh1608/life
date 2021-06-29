# spark

## env







# 项目

## 项目环境

### 目录结构

```sh
aliyun-spark-sample/
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
│   │               ├── SimpleApp.scala
│   │               └── WordCount.scala
│   └── test
│       └── scala
│           └── samples
│               ├── junit.scala
│               ├── scalatest.scala
│               └── specs.scala

```

`pom.xml`文件参考示例：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.zn</groupId>
  <artifactId>aliyun-spark-sample</artifactId>
  <version>1.0-SNAPSHOT</version>
  <name>${project.artifactId}</name>
  <description>My wonderfull scala app</description>
  <inceptionYear>2018</inceptionYear>
  <licenses>
    <license>
      <name>My License</name>
      <url>http://....</url>
      <distribution>repo</distribution>
    </license>
  </licenses>

  <properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <encoding>UTF-8</encoding>
    <scala.version>2.12.10</scala.version>
    <scala.compat.version>2.12</scala.compat.version>
    <spark.version>3.1.1</spark.version>
    <spec2.version>4.2.0</spec2.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.scala-lang</groupId>
      <artifactId>scala-library</artifactId>
      <version>${scala.version}</version>
    </dependency>
    <dependency>
      <groupId>org.apache.spark</groupId>
      <artifactId>spark-hive_${scala.compat.version}</artifactId>
      <version>${spark.version}</version>
    </dependency>
    <dependency>
      <groupId>org.apache.spark</groupId>
      <artifactId>spark-sql_${scala.compat.version}</artifactId>
      <version>${spark.version}</version>
    </dependency>
    <dependency>
      <groupId>org.apache.spark</groupId>
      <artifactId>spark-core_${scala.compat.version}</artifactId>
      <version>${spark.version}</version>
    </dependency>
    <!-- Test -->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.scalatest</groupId>
      <artifactId>scalatest_${scala.compat.version}</artifactId>
      <version>3.0.5</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.specs2</groupId>
      <artifactId>specs2-core_${scala.compat.version}</artifactId>
      <version>${spec2.version}</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.specs2</groupId>
      <artifactId>specs2-junit_${scala.compat.version}</artifactId>
      <version>${spec2.version}</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

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
    <plugins>
      <plugin>
        <!-- see http://davidb.github.com/scala-maven-plugin -->
        <groupId>net.alchim31.maven</groupId>
        <artifactId>scala-maven-plugin</artifactId>
        <version>3.3.2</version>
        <executions>
          <execution>
            <goals>
              <goal>compile</goal>
              <goal>testCompile</goal>
            </goals>
            <configuration>
              <args>
                <arg>-dependencyfile</arg>
                <arg>${project.build.directory}/.scala_dependencies</arg>
              </args>
            </configuration>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>2.21.0</version>
        <configuration>
          <!-- Tests will be run with scalatest-maven-plugin instead -->
          <skipTests>true</skipTests>
        </configuration>
      </plugin>
      <plugin>
        <groupId>org.scalatest</groupId>
        <artifactId>scalatest-maven-plugin</artifactId>
        <version>2.0.0</version>
        <configuration>
          <reportsDirectory>${project.build.directory}/surefire-reports</reportsDirectory>
          <junitxml>.</junitxml>
          <filereports>TestSuiteReport.txt</filereports>
          <!-- Comma separated list of JUnit test class names to execute -->
          <jUnitClasses>samples.AppTest</jUnitClasses>
        </configuration>
        <executions>
          <execution>
            <id>test</id>
            <goals>
              <goal>test</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
</project>
```

### 编译命令

```sh
mvn package
```

### 执行测试代码

- Java执行代码（Hello.java）

```sh
spark-submit --class "com.zn.Hello" --master local[1] ./target/aliyun-spark-sample-1.0-SNAPSHOT.jar
```

- Scala执行代码 (WordCount.scala)

```sh
spark-submit --class "com.zn.WordCount" --master local[1] target/scala-2.12/simple-project_2.12-1.0.jar
```





## WordCount(Java/Scala)

**Scala**: worldCount.scala  - RDD

```scala
package com.zn

import org.apache.spark.SparkConf
import org.apache.spark.SparkContext
// import org.apache.spark.sql.SparkSession

object WordCount {
    def main(args: Array[String]): Unit = {
        // val logFile = "./data/test.txt"
        val logFile = if (args.length > 0) args(0)
        val conf = new SparkConf().setMaster("local[1]").setAppName("wc")
        val sc = new SparkContext(conf)
        val logData = sc.textFile(logFile)     
        // val spark = SparkSession.builder
        //     .master("local[1]")
        //     .appName("wc")
        //     .getOrCreate()
        // val logData = spark.read.textFile(logFile).cache()
        val words = logData.flatMap { line => line.split(" ")}
        val pairs = words.map{ word => (word, 1)}
        val results = pairs.reduceByKey(_+_).map(tuple => (tuple._2, tuple._1))
        val sorted = results.sortByKey(false).map(tuple => (tuple._2, tuple._1))
        sorted.foreach { x => println(x) }
        sc.stop()
    }
}
```

```sh
mvn package

spark-submit --class "com.zn.WordCount" --master local[1] target/aliyun-spark-sample-1.0-SNAPSHOT.jar ./pom.xml
```



**java**: JavaWordCount.java

```java
```

