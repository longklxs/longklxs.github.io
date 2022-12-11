

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

1. 上面定义的pom是maven允许的最低限度。它们共同标志着maven在仓库中的位置。
   
   1. groupId：这在组织或项目中通常是唯一的。点标记的groupId不必对应项目包含的包结构。但是，这是一个值得遵循的好习惯。当存储在存储库中时，组的作用很像java打包结构在操作系统的作用。例如：org.codehaus.mojo组位于目录$M2_REPO/org/codehaus/mojo。
   
   2. artifactId：artifactId通常是项目的名称。和groupId一样，区分不同的包，例如：my-project住在$M2_REPO/org/codehaus/mojo/my-project。
   
   3. version：groupId：artifactId表示单个项目，但他们无法描述项目的版本，my-project 1.0版本位于目录结构中$M2_REPO/org/codehaus/mojo/my-project/1.0。
   
   上面给出的三个元素指向一个项目的特定版本。
