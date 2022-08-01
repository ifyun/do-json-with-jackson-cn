# Maven 依赖

首先将 `jackson-databind` 依赖添加到 `pom.xml` 中：

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.13.3</version>
</dependency>
```

此依赖会将以下库添加到 classpath：

1. `jackson-annotations-2.13.3.jar`
2. `jackson-core-2.13.3.jar`
3. `jackson-databind-2.13.3.jar`

!> 原书使用的版本是 2.9.8
