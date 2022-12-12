## 什么是pom.xml?

pom是项目对象模型（Project object model）的简称，它是Maven项目中的文件，使用XML表示，名称叫做pom.xml。作用类似ant的build.xml文件，功能强大。该文件用于管理：源代码、配置文件、开发者的信息和角色、问题追踪系统、组织信息、项目授权、项目的url、项目的依赖关系等等。事实上、在Maven世界中，project可以什么都没有，甚至没有代码，但是必须包含pom.xml文件。

## 概览

下面是一个POM项目中的pom.xml文件中包含的元素。注意，其中的modelVersion是4.0.0，这是当前仅有的可以被Maven2&3同时支持的POM版本，它是必须的。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
            http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <!-- 基本设置 The Basics -->
    <groupId>...</groupId>
    <artifactId>...</artifactId>
    <version>...</version>
    <packaging>...</packaging>
    <dependencies>...</dependencies>
    <parent>...</parent>
    <dependencyManagement>...</dependencyManagement>
    <modules>...</modules>
    <properties>...</properties>
    
    <!-- 构建过程的设置 Build Settings -->
    <build>...</build>
    <reporting>...</reporting>
    
    <!-- 项目信息设置 More Project Information -->
    <name>...</name>
    <description>...</description>
    <url>...</url>
    <inceptionYear>...</inceptionYear>
    <licenses>...</licenses>
    <organization>...</organization>
    <developers>...</developers>
    <contributors>...</contributors>
    
    <!-- 环境设置 Environment Settings -->
    <issueManagement>...</issueManagement>
    <ciManagement>...</ciManagement>
    <mailingLists>...</mailingLists>
    <scm>...</scm>
    <prerequisites>...</prerequisites>
    <repositories>...</repositories>
    <pluginRepositories>...</pluginRepositories>
    <distributionManagement>...</distributionManagement>
    <profiles>...</profiles>
</project>
```

## 一、基本配置

1. Maven的协作相关属性
   * 一个最简单的pom.xml的定义必须包含modelVersion、groupId、artifactId和version这四个元素，当然这其中的元素也是可以从它的父项目中继承的。在Maven中，使用groupId、artifactId和version组成groupdId:artifactId:version的形式来唯一确定一个项目：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"  
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0  
                      http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!-- 
        含义：组织标识，定义了项目属于哪个组，风向标，坐标，或者说若把本项目打包
        用途：此名称则是本地仓库中的路径，列如：otowa.user.dao，在M2_REPO目录下，将是: otowa/user/dao目录
        命名规范:项目名称，模块，子模块
    -->
    <groupId>otowa.user.dao</groupId>
    <!-- 
        含义：项目名称也可以说你所模块名称，定义当面Maven项目在组中唯一的ID
        用途：例如：user-dao，在M2_REPO目录下，将是：otowa/user/dao/user-dao目录
        命名规范:唯一就好
    -->
    <artifactId>user-dao</artifactId>
    <!-- 
        含义：项目当前的版本号
        用途：例如：0.0.1-SNAPSHOT，在M2_REPO目录下，将是：otowa/user/dao/user-dao/0.0.1-SNAPSHOT目录
    -->
    <version>0.0.1-SNAPSHOT</version>
    <!-- 打包的格式，可以为：pom , jar , maven-plugin , ejb , war , ear , rar , par -->
    <packaging>war</packaging>
    <!-- 元素声明了一个对用户更为友好的项目名称 -->
    <name>maven</name>
</project>
```

2. POM之间的关系，继承、聚合与依赖
   * 我们知道Maven在建立项目的时候是基于Maven项目下的pom.xml进行的，我们项目依赖的信息和一些基本信息都是在这个文件里面定义的。那如果当我们有多个项目要进行，这多个项目有些配置内容是相同的，有些是要彼此关联的，那如果按照传统的做法的话我们就需要在多个项目中都定义这些重复的内容。这无疑是非常耗费时间和不易维护的。好在Maven给我们提供了一个pom的继承和聚合的功能。
   * 对于使用java的人而言，继承这个词大家应该都不陌生。要继承pom就需要有一个父pom，在Maven中定义了超级pom.xml，任何没有申明自己父pom.xml的pom.xml都将默认继承自这个超级pom.xml。
   * 位置：
     * 　在Maven 2.xxx版本中，比如maven-2.0.9-uber.jar，打开此Jar文件后，在包包org.apache.maven.project下会有pom-4.0.0.xml的文件
     * 但是到了3.xxx版本之后在： aven安装目录下的：/lib/maven-model-builder-version.jar中 \org\apache\maven\mode目录中的pom-4.0.0.xml。
     * 先来看一下这个超级pom.xml的定义：

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

<!-- START SNIPPET: superpom -->
<project>
  <modelVersion>4.0.0</modelVersion>

  <repositories>
    <repository>
      <id>central</id>
      <name>Central Repository</name>
      <url>https://repo.maven.apache.org/maven2</url>
      <layout>default</layout>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
    </repository>
  </repositories>

  <pluginRepositories>
    <pluginRepository>
      <id>central</id>
      <name>Central Repository</name>
      <url>https://repo.maven.apache.org/maven2</url>
      <layout>default</layout>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <releases>
        <updatePolicy>never</updatePolicy>
      </releases>
    </pluginRepository>
  </pluginRepositories>

  <build>
    <directory>${project.basedir}/target</directory>
    <outputDirectory>${project.build.directory}/classes</outputDirectory>
    <finalName>${project.artifactId}-${project.version}</finalName>
    <testOutputDirectory>${project.build.directory}/test-classes</testOutputDirectory>
    <sourceDirectory>${project.basedir}/src/main/java</sourceDirectory>
    <scriptSourceDirectory>${project.basedir}/src/main/scripts</scriptSourceDirectory>
    <testSourceDirectory>${project.basedir}/src/test/java</testSourceDirectory>
    <resources>
      <resource>
        <directory>${project.basedir}/src/main/resources</directory>
      </resource>
    </resources>
    <testResources>
      <testResource>
        <directory>${project.basedir}/src/test/resources</directory>
      </testResource>
    </testResources>
    <pluginManagement>
      <!-- NOTE: These plugins will be removed from future versions of the super POM -->
      <!-- They are kept for the moment as they are very unlikely to conflict with lifecycle mappings (MNG-4453) -->
      <plugins>
        <plugin>
          <artifactId>maven-antrun-plugin</artifactId>
          <version>1.3</version>
        </plugin>
        <plugin>
          <artifactId>maven-assembly-plugin</artifactId>
          <version>2.2-beta-5</version>
        </plugin>
        <plugin>
          <artifactId>maven-dependency-plugin</artifactId>
          <version>2.8</version>
        </plugin>
        <plugin>
          <artifactId>maven-release-plugin</artifactId>
          <version>2.3.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>

  <reporting>
    <outputDirectory>${project.build.directory}/site</outputDirectory>
  </reporting>

  <profiles>
    <!-- NOTE: The release profile will be removed from future versions of the super POM -->
    <profile>
      <id>release-profile</id>

      <activation>
        <property>
          <name>performRelease</name>
          <value>true</value>
        </property>
      </activation>

      <build>
        <plugins>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-source-plugin</artifactId>
            <executions>
              <execution>
                <id>attach-sources</id>
                <goals>
                  <goal>jar</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-javadoc-plugin</artifactId>
            <executions>
              <execution>
                <id>attach-javadocs</id>
                <goals>
                  <goal>jar</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-deploy-plugin</artifactId>
            <configuration>
              <updateReleaseInfo>true</updateReleaseInfo>
            </configuration>
          </plugin>
        </plugins>
      </build>
    </profile>
  </profiles>

</project>
<!-- END SNIPPET: superpom -->
```

​					对于一个pom.xml来说有几个元素是必须定义的，一个是project根元素，然后就是它里面的modelVersion、groupId、artifactId和version。由上面的超级pom.xml的内容我们可以看到pom.xml中没有groupId、artifactId和version的定义，所以我们在建立自己的pom.xml的时候就需要定义这三个元素。和java里面的继承类似，子pom.xml会完全继承父pom.xml中所有的元素，而且对于相同的元素，一般子pom.xml中的会覆盖父pom.xml中的元素，但是有几个特殊的元素它们会进行合并而不是覆盖。这些特殊的元素是：

* dependencies
* developers
* contributors
* plugin列表，包括plugin下面的reports
* resources

3. 继承

   * 被继承项目与继承项目是父子目录关系

     现在假设我们有一个项目projectA，它的pom.xml定义如下：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>com.tiantian.mavenTest</groupId>  
  <artifactId>projectA</artifactId>  
  <packaging>jar</packaging>  
  <version>1.0-SNAPSHOT</version>  
</project>  
```



然后我们有另一个项目projectB，而且projectB是跟projectA的pom.xml文件处于同一个目录下，这时候如果projectB需要继承自projectA的话我们可以这样定义projectB的pom.xml文件。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">  
  <parent>  
    <groupId>com.tiantian.mavenTest</groupId>  
    <artifactId>projectA</artifactId>  
    <version>1.0-SNAPSHOT</version>  
  </parent>  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>com.tiantian.mavenTest</groupId>  
  <artifactId>projectB</artifactId>  
  <packaging>jar</packaging>  
  <version>1.0-SNAPSHOT</version>  
</project> 
```

由projectB的pom.xml文件的定义我们可以知道，当需要继承指定的一个Maven项目时，我们需要在自己的pom.xml中定义一个parent元素，在这个元素中指明需要继承项目的groupId、artifactId和version

* 被继承项目与继承项目的目录结构不是父子关系

  * 当被继承项目与继承项目的目录结构不是父子关系的时候，我们再利用上面的配置是不能实现Maven项目的继承关系的，这个时候我们就需要在子项目的pom.xml文件定义中的parent元素下再加上一个relativePath元素的定义，用以描述父项目的pom.xml文件相对于子项目的pom.xml文件的位置。

  * 假设我们现在还是有上面两个项目，projectA和projectB，projectB还是继承自projectA，但是现在projectB不在projectA的子目			录中，而是与projectA处于同一目录中。这个时候projectA和projectB的目录结构如下：

  * ------projectA

    　　------pom.xml

    ------projectB

    　　------pom.xml

  * 　这个时候我们可以看出projectA的pom.xml相对于projectB的pom.xml的位置是“../projectA/pom.xml”，所以这个时候projectB的pom.xml的定义应该如下所示：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">  
  <parent>  
    <groupId>com.tiantian.mavenTest</groupId>  
    <artifactId>projectA</artifactId>  
    <version>1.0-SNAPSHOT</version>  
    <relativePath>../projectA/pom.xml</relativePath>  
  </parent>  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>com.tiantian.mavenTest</groupId>  
  <artifactId>projectB</artifactId>  
  <packaging>jar</packaging>  
  <version>1.0-SNAPSHOT</version>  
</project>  
```



* 聚合

  * 对于聚合这个概念搞java的人应该都不会陌生。先来说说我对聚合和被聚合的理解，比如说如果projectA聚合到projectB，那么我们就可以说projectA是projectB的子模块， projectB是被聚合项目，也可以类似于继承那样称为父项目。对于聚合而言，这个主体应该是被聚合的项目。所以，我们需要在被聚合的项目中定义它的子模块，而不是像继承那样在子项目中定义父项目。具体做法是：

    * 修改被聚合项目的pom.xml中的packaging元素的值为pom
    * 在被聚合项目的pom.xml中的modules元素下指定它的子模块项目

  * 对于聚合而言，当我们在被聚合的项目上使用Maven命令时，实际上这些命令都会在它的子模块项目上使用。这就是Maven中聚合的一个非常重要的作用。假设这样一种情况，你同时需要打包或者编译projectA、projectB、projectC和projectD，按照正常的逻辑我们一个一个项目去使用mvn compile或mvn package进行编译和打包，对于使用Maven而言，你还是这样使用的话是非常麻烦的。因为Maven给我们提供了聚合的功能。我们只需要再定义一个超级项目，然后在超级项目的pom.xml中定义这个几个项目都是聚合到这个超级项目的。之后我们只需要对这个超级项目进行mvn compile，它就会把那些子模块项目都进行编译。

  * 被聚合项目和子模块项目在目录结构上是父子关系

    * 还拿上面定义的projectA和projectB来举例子，现在假设我们需要把projectB聚合到projectA中。projectA和projectB的目录结构如下所示

    * ------projectA

      　　　　------projectB

      　　　　　　-----pom.xml

      　　　　------pom.xml

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>com.tiantian.mavenTest</groupId>  
  <artifactId>projectA</artifactId>  
  <version>1.0-SNAPSHOT</version>  
  <packaging>pom</packaging>  
  <modules>  
       <module>projectB</module>  
  </modules>  
</project> 
```





* 由上面的定义我们可以看到被聚合的项目的packaging类型应该为pom，而且一个项目可以有多个子模块项目。对于聚合这种情况，我们使用子模块项目的artifactId来作为module的值，表示子模块项目相对于被聚合项目的地址，在上面的示例中就表示子模块projectB是处在被聚合项目的子目录下，即与被聚合项目的pom.xml处于同一目录。这里使用的module值是子模块projectB对应的目录名projectB，而不是子模块对应的artifactId。这个时候当我们对projectA进行mvn package命令时，实际上Maven也会对projectB进行打包。

* 被聚合项目与子模块项目在目录结构上不是父子关系

  * 那么当被聚合项目与子模块项目在目录结构上不是父子关系的时候，我们应该怎么来进行聚合呢？还是像继承那样使用relativePath元素吗？答案是非也，具体做法是在module元素中指定以相对路径的方式指定子模块。我们来看下面一个例子

  * 继续使用上面的projectA和projectB，还是需要把projectB聚合到projectA，但是projectA和projectB的目录结构不再是父子关系，而是如下所示的这种关系

  * ------projectA

    　　　　------pom.xml

    ------projectB

    　　　　------pom.xml

  * 这个时候projectA的pom.xml文件就应该这样定义：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
   
  <groupId>com.tiantian.mavenTest</groupId>  
  <artifactId>projectA</artifactId>  
  <version>1.0-SNAPSHOT</version>  
  <packaging>pom</packaging>  
  <modules>  
       <module>../projectB</module>  
  </modules>  
</project>
```



* 注意看module的值是“../projectB”，我们知道“..”是代表当前目录的上层目录，所以它表示子模块projectB是被聚合项目projectA的pom.xml文件所在目录（即projectA）的上层目录下面的子目录，即与projectA处于同一目录层次。注意，这里的projectB对应的是projectB这个项目的目录名称，而不是它的artifactId

* 聚合与继承同时进行

  * 假设有这样一种情况，有两个项目，projectA和projectB，现在我们需要projectB继承projectA，同时需要把projectB聚合到projectA。然后projectA和projectB的目录结构如下

  * ------projectA

    ​       ------pom.xml

    ------projectB

    ​       ------pom.xml

  * 那么这个时候按照上面说的那样，projectA的pom.xml中需要定义它的packaging为pom，需要定义它的modules，所以projectA的pom.xml应该这样定义：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>com.tiantian.mavenTest</groupId>  
  <artifactId>projectA</artifactId>  
  <version>1.0-SNAPSHOT</version>  
  <packaging>pom</packaging>  
  <modules>  
       <module>../projectB</module>  
  </modules>  
</project> 
```



* 而projectB是继承自projectA的，所以我们需要在projectB的pom.xml文件中新增一个parent元素，用以定义它继承的项目信息。所以projectB的pom.xml文件的内容应该这样定义：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <parent>  
       <groupId>com.tiantian.mavenTest</groupId>  
       <artifactId>projectA</artifactId>  
       <version>1.0-SNAPSHOT</version>  
       <relativePath>../projectA/pom.xml</relativePath>  
  </parent>  
  <groupId>com.tiantian.mavenTest</groupId>  
  <artifactId>projectB</artifactId>  
  <version>1.0-SNAPSHOT</version>  
  <packaging>jar</packaging>  
</project>
```

 

5. 依赖关系：依赖关系列表（dependency list）是POM的重要部分
   * 项目之间的依赖是通过pom.xml文件里面的dependencies元素下面的dependency元素进行的。一个dependency元素定义一个依赖关系。在dependency元素中我们主要通过依赖项目的groupId、artifactId和version来定义所依赖的项目。
   * 先来看一个简单的项目依赖的示例吧，假设我现在有一个项目projectA，然后它里面有对junit的依赖，那么它的pom.xml就类似以下这个样子：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>com.tiantian.mavenTest</groupId>  
  <artifactId>projectB</artifactId>  
  <version>1.0-SNAPSHOT</version>  
  <packaging>jar</packaging>  
   
  <dependencies>  
    <dependency>  
      <groupId>junit</groupId>  
      <artifactId>junit</artifactId>  
      <version>3.8.1</version>  
      <scope>test</scope>  
      <optional>true</optional>  
    </dependency>  
  </dependencies>  
</project>
```



groupId，artifactId,version:描述了依赖的项目唯一标志

type:对应于依赖项目的packaging类型，默认是jar

scope:用于限制相应的依赖范围、传播范围。scope的主要取值范围如下（还有一个是在Maven2.0.9以后版本才支持的import，关于import作用域将在后文《dependency介绍》中做介绍）：

* **test:**在测试范围有效，它在执行命令test的时候才执行，并且它不会传播给其他模块进行引入，比如 junit,dbunit 等测试框架
* **compile(default 默认):**这是它的默认值，这种类型很容易让人产生误解，以为只有在编译的时候才是需要的，其实这种类型表示所有的情况都是有用的，包括编译和运行时。而且这种类型的依赖性是可以传递的
* **runtime:**在程序运行的时候依赖，在编译的时候不依赖。
* **provided:**这个跟compile很类似，但是它表示你期望这个依赖项目在运行时由JDK或者容器来提供。这种类型表示该依赖只有在测试和编译的情况下才有效，在运行时将由JDK或者容器提供。这种类型的依赖性是不可传递的。**比如 javaee：**
  * eclipse开发web环境中是没有javaee必须要手动添加。
  * myeclipse新建web项目会有JavaEE(servlet-api.jar,jsp-api.jar...)web容器依赖的jar包，一般都是做开发的时候才使用。但是myeclipse不会把这些 jar包发布的，lib下你是找不到javaee引入的jar包,因为myeclipse发布项目的时候会忽略它。为什么？因为tomcat容器bin/lib已经存在了这个jar包了。
* **system：**这种类型跟provided类似，唯一不同的就是这种类型的依赖我们要自己提供jar包，这需要与另一个元素systemPath来结合使用。systemPath将指向我们系统上的jar包的路径，而且必须是给定的绝对路径。
  * **systemPath：**上面已经说过了这个元素是在scope的值为system的时候用于指定依赖的jar包在系统上的位置的，而且是绝对路径。该元素必须在依赖的 jar包的scope为system时才能使用，否则Maven将报错。
  * **optional：**当该项目本身作为其他项目的一个依赖时标记该依赖为可选项。假设现在projectA有一个依赖性projectB，我们把projectB这个依赖项设为optional，这表示projectB在projectA的运行时不一定会用到。这个时候如果我们有另一个项目projectC，它依赖于projectA，那么这个时候因为projectB对于projectA是可选的，所以Maven在建立projectC的时候就不会安装projectB，这个时候如果projectC确实需要使用到projectB，那么它就可以定义自己对projectB的依赖。当一个依赖是可选的时候，我们把optional元素的值设为true，否则就不设置optional元素。
  * **exclusions：**考虑这样一种情况，我们的projectA依赖于projectB，然后projectB又依赖于projectC，但是在projectA里面我们不需要projectB依赖的projectC，那么这个时候我们就可以在依赖projectB的时候使用exclusions元素下面的exclusion排除projectC。这个时候我们可以这样定义projectA对projectB的依赖：

```xml
<dependencies>  
     <dependency>  
            <groupId>com.tiantian.mavenTest</groupId>  
            <artifactId>projectB</artifactId>  
            <version>1.0-SNAPSHOT</version>  
            <exclusions>  
                   <exclusion>  
                          <groupId>com.tiantian.mavenTest</groupId>  
                          <artifactId>projectC</artifactId>  
                   </exclusion>  
            </exclusions>  
     </dependency>  
</dependencies>
```



* 属性

  * 在pom.xml文件中我们可以使用${propertyName}的形式引用属性。是值的占位符，类似EL，类似ant的属性，比如${X}，可用于pom文件任何赋值的位置。有以下分类

    * **env.propertyName：**这种形式表示引用的是环境变量，比如我们需要引用当前系统的环境变量PATH的时候，就可以使用${env.PATH}。

    * **project.propertyName：**这种形式表示引用的是当前这个pom.xml中project根元素下面的子元素的值。比如我们需要引用当前project下面的version的时候，就可以使用${project.version}。

    * **settings.propertyName：**这种形式引用的是Maven本地配置文件settings.xml或本地Maven安装目录下的settings.xml文件根元素settings下的元素。比如我们需要引用settings下的本地仓库localRepository元素的值时，我们可以用${settings.localRepository}。

    * **Java System Properties：**java的系统属性，所有在java中使用java.lang.System.getProperties()能够获取到的属性都可以在pom.xml中引用，比如${java.home}。

    * **自定义：**pom.xml中properties元素下面的子元素作为属性。假如在pom.xml中有如下一段代码<properties><hello.world>helloWorld</hello.world></properties>，那么我们就可以使用${hello.world}引用到对应的helloWorld。
