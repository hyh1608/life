# java

- **arthas** (https://arthas.aliyun.com/doc/)

- 为什么java jvm设计STW

  - > 

![image-20210903222436847](./images/image-20210903222436847.png)



### tools

- x

### JDK JRE JVM

![image-20210904224453513](.\images\image-20210904224453513.png)

### == and equals

- == 对比的是栈中的值，
  - 如果是基本数据类型则对比的是变量值
  - 如果是引用类型则对比的是内存对象的地址
- equals
  - object中默认采用==比较
  - String类中重写equals，只对比值

![image-20210904225525039](.\images\image-20210904225525039.png)

### final

内部类如果要使用外部变量，则外部变量必须要用final修饰

### String StringBuffer StringBuilder

![image-20210905224728575](.\images\image-20210905224728575.png)