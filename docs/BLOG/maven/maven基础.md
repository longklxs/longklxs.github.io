## Maven项目目录结构

| 目录                          | 目的                           |
| :---------------------------- | ------------------------------ |
| ${basedir}                    | 存放pom.xml和所有的子目录      |
| ${basedir}/src/main/java      | 项目的java源代码               |
| ${basedir}/src/main/resources | 项目的资源，比如说property文件 |
| ${basedir}/src/test/java      | 项目的测试类，比如说JUnit代码  |
| ${basedir}/src/test/resources | 测试使用的资源                 |



## Maven项目的编译和运行

1、编译java文件

```terminal
mvn compile
```

2、执行main方法

```terminal
mvn exec:java -D"exec.mainClass=com.HelloMaven"
```



## Maven项目的setting.xml设置

1. 本地仓库配置

![image-20221212190704921](D:\repository\doc\mysite\docs\BLOG\maven\maven基础\image\image-20221212190704921.png)

2. 镜像配置

![image-20221212191109463](D:\repository\doc\mysite\docs\BLOG\maven\maven基础\image\image-20221212191109463.png)

```xml
<!--阿里巴巴镜像-->
	<mirror>
      <id>aliyunmaven</id>
      <mirrorOf>*</mirrorOf>
      <name>阿里云公共仓库</name>
      <url>https://maven.aliyun.com/repository/public</url>
    </mirror>
```

