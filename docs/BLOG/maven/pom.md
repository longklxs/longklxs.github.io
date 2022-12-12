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

* systemPath:

  * is used only if the dependency scope is system.Otherwise, the build will fail if this element is set. The path must be absolute, so it is recommended to use a property to specify the machine-specific path (more on `properties` below), such as `${java.home}/lib`. Since it is assumed that system scope dependencies are installed *a priori*, Maven does not check the repositories for the project, but instead checks to ensure that the file exists. If not, Maven fails the build and suggests that you download and install it manually.

* optional:

  * Marks a dependency optional when this project itself is a dependency. For example, imagine a project `A` that depends upon project `B` to compile a portion of code that may not be used at runtime, then we may have no need for project `B` for all project. So if project `X` adds project `A` as its own dependency, then Maven does not need to install project `B` at all. Symbolically, if `=>` represents a required dependency, and `-->` represents optional, although `A=>B` may be the case when building A `X=>A-->B` would be the case when building `X`.

    In the shortest terms, `optional` lets other projects know that, when you use this project, you do not require this dependency in order to work correctly.


### Dependency Version Requirement Specificaiton

Dependencies'versioon elements define version requirements, which are used to compute dependency version. Soft requirements can be replaced by different versions of the same artifact found elsewhere in the dependency graph. Hard requirements mandate a particular version or versions and override soft requirements. if there are no versions of a dependency that satisfy all the hard requirements for that artifact, the build fails.

Version requirements have the following syntax:

* 1.0：Soft requirement for 1.0. Use 1.0 if no other version appears earlier in the dependency tree.
* [1.0] ：Hard requirement for 1.0. Use 1.0 and only 1.0.
* (,1.0)：Hard requirement for any version <= 1.0.
* [1.2,1.3]：Hard requirement for any version between 1.2 and 1.3 inclusive.

* [1.0,2.0)：1.0<=x<2.0;Hard requirement for any version less than or equal to 1.5
* [1.5,)：Hard requirement for any version greater than or equal to 1.5.
* (,1.0],[1.2,): Hard requirement for any version less than or equal to 1.0 than or greater than or equal to 1.2, but not 1.1. Multiple requirements are separated by commas.

* (,1.1),(1.1,): Hard requirement for any version except 1.1; for example because 1.1 has a critical vulnerability.

Maven picks the highest version of each project that satisfies all the hard requirements of the dependencies on he project. if no version satisfies all the hard requirements, the build fails.

### Version order specification:

* semver规则

### Version Order Testing:

...

### Exclusions

* Exclusions tell Maven not to include the specified project that is a dependency of this dependency(in other words, its transitive dependency). For example, the maven-embedder requires maven-core, and we do not wish to use it or its dependencies, then we would add it as an exclusion.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <dependencies>
    <dependency>
      <groupId>org.apache.maven</groupId>
      <artifactId>maven-embedder</artifactId>
      <version>2.0</version>
      <exclusions>
        <exclusion>
          <groupId>org.apache.maven</groupId>
          <artifactId>maven-core</artifactId>
        </exclusion>
      </exclusions>
    </dependency>
    ...
  </dependencies>
  ...
</project>
```

* It is also sometimes useful to clip a dependency's transitive dependencies. A dependency may have incorrectly specified scopes, or dependencies that conflict with other dependencies in your project. Using wildcard excludes makes it easy to exclude all a dependency's transitive dependencies. In the case below you may be working with the maven-embedder and you want to manage the dependencies you use yourself, so you clip all the transitive dependencies:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <dependencies>
    <dependency>
      <groupId>org.apache.maven</groupId>
      <artifactId>maven-embedder</artifactId>
      <version>3.1.0</version>
      <exclusions>
        <exclusion>
          <groupId>*</groupId>
          <artifactId>*</artifactId>
        </exclusion>
      </exclusions>
    </dependency>
    ...
  </dependencies>
  ...
</project>
```

* exclusions:Exclusions contain one or more exclusion elements, each containing a groupId and artifactId denoting a dependency to exclude. Unlike optional, which may or may not be installed and used, exclusions actively remove themselves from the dependency tree.

### Inheritance

* One powerful addition that Maven brings to build management is the concept of project inheritance. Although in build systems such as Ant inheritance can be simulated, Maven makes project inheritance explicit in the project object model.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>org.codehaus.mojo</groupId>
  <artifactId>my-parent</artifactId>
  <version>2.0</version>
  <packaging>pom</packaging>
</project>
```

* The `packaging` type required to be `pom` for *parent* and *aggregation* (multi-module) projects. These types define the goals bound to a set of lifecycle stages. For example, if packaging is `jar`, then the `package` phase will execute the `jar:jar` goal. Now we may add values to the parent POM, which will be inherited by its children. Most elements from the parent POM are inherited by its children, including:

  			* groupId

  * version
  * description
  * url
  * inceptionYear
  * organization
  * licenses
  * developers
  * contributors
  * mailingLists
  * scm
  * issueMangement
  * ciManagement
  * properties
  * dependencyManagement
  * dependencies
  * repositories
  * pluginRepositories
  * build
    * plugin
    * executions with matching ids
    * plugin configuration
    * etc.
  * reporting
  * profiles

* Notable elements which are not inherited include:

  * artifactId
  * name
  * prerequisites

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <parent>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>my-parent</artifactId>
    <version>2.0</version>
    <relativePath>../my-parent</relativePath>
  </parent>
 
  <artifactId>my-project</artifactId>
</project>
```



