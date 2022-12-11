## pom's basic element

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <!-- The Basics -->
  <groupId>...</groupId>
  <artifactId>...</artifactId>
  <version>...</version>
  <packaging>...</packaging>
  <dependencies>...</dependencies>
  <parent>...</parent>
  <dependencyManagement>...</dependencyManagement>
  <modules>...</modules>
  <properties>...</properties>

  <!-- Build Settings -->
  <build>...</build>
  <reporting>...</reporting>

  <!-- More Project Information -->
  <name>...</name>
  <description>...</description>
  <url>...</url>
  <inceptionYear>...</inceptionYear>
  <licenses>...</licenses>
  <organization>...</organization>
  <developers>...</developers>
  <contributors>...</contributors>

  <!-- Environment Settings -->
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

## maven coordinates(maven的坐标)

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>org.codehaus.mojo</groupId>
  <artifactId>my-project</artifactId>
  <version>1.0</version>
</project>
```

* 上面定义的pom是maven允许的最低限度。它们共同标志着maven在仓库中的位置。
  
  * groupId：这在组织或项目中通常是唯一的。点标记的groupId不必对应项目包含的包结构。但是，这是一个值得遵循的好习惯。当存储在存储库中时，组的作用很像java打包结构在操作系统的作用。例如：org.codehaus.mojo组位于目录$M2_REPO/org/codehaus/mojo。
  
  * artifactId：artifactId通常是项目的名称。和groupId一样，区分不同的包，例如：my-project住在$M2_REPO/org/codehaus/mojo/my-project。
  
  * version：groupId：artifactId表示单个项目，但他们无法描述项目的版本，my-project 1.0版本位于目录结构中$M2_REPO/org/codehaus/mojo/my-project/1.0。

上面给出的三个元素指向一个项目的特定版本。

## packaging(打包)

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <packaging>war</packaging>
  ...
</project>

```

* 当没有声明packaging时,Maven假定打包是默认的:jar

## POM Relationships

### dependencies

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <type>jar</type>
      <scope>test</scope>
      <optional>true</optional>
    </dependency>
    ...
  </dependencies>
  ...
</project>
```

* groupId、artifactId、version：

  * 这三个一体表示项目的坐标

* classifier：
  * The classifier distinguishes artifacts that were build from the same POM but differ in content. It is some optional and arbitrary string that -if present-is appended to the artifact name just after the version number.
* type：
  * 对应于选择的依赖类型。这默认为jar。虽然通常表示依赖文件名的扩展名，但情况并非总是如此。
* scope:
  * This element refers to the classpath of the task at hand(comiling and runtime, testing, etc) as well as how to limit the transitivity of a dependency. There are five scopes available:
    * compile -this is the default scope, used if none is specified.Compile dependencies are available in all classpaths. Furthermore, those dependencies are propagated to dependent projects.
    * provided -this is much like compile, but indicates you expect the JDK or a container to provide it at runtime. It is only available on the compilation and test classpath, and is not transitive.
    * runtime -this scope indicates that the dependency is not required for compilation, but is for execution. It is in the runtime and test classpaths, but not the compile classpath.
    * test -this scope indicates that the dependency is not required for normal use of the application, and is only available for the test compilation and execution phases. It is not transitive.
    * system - this scope is similar to provided except that you have to provide the JAR which contains it explicitly. The artifact is always available and is not looked up in a repository.
