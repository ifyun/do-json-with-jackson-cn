# 解组具有额外/未知字段的 JSON

JSON 输入有各种形状和大小，大多数情况下，我们需要将其映射到有一定数量字段的预定义 Java 对象。我们的目标是**简单地忽略任何不能映射到现有
Java 字段的 JSON 属性。**

例如，假设我们需要将 JSON 解组为以下 Java 实体：

```java
public class MyDto {
    private String stringValue;
    private int intValue;
    private boolean booleanValue;

    // standard constructor, getters and setters
}
```

## UnrecognizedPropertyException

试图向这个简单的 Java 实体解组具有未知属性的 JSON 将导致 `com.fasterxml.jackson.databind.exc.UnrecognizedPropertyException`：

```java
@Test(expected = UnrecognizedPropertyException.class)
public void givenJsonHasUnknownValues_whenDeserializing_thenException()
  throws JsonParseException, JsonMappingException, IOException {
    String jsonAsString =
        "{\"stringValue\": \"a\"," +
        "\"intValue\": 1," +
        "\"booleanValue\": true," +
        "\"stringValue2\": \"something\"}";
    
    ObjectMapper mapper = new ObjectMapper();
    
    MyDto readValue = mapper.readValue(jsonAsString, MyDto.class);
    
    assertNotNull(readValue);
}
```

这将失败并出现以下异常：

```text
com.fasterxml.jackson.databind.exc.UnrecognizedPropertyException:
Unrecognized field "stringValue2" (class org.baeldung.jackson.ignore.MyDto),
not marked as ignorable (3 known properties: "stringValue", "booleanValue", "intValue"])
```

## 通过 ObjectMapper 处理未知字段

我们现在可以配置完整的 `ObjectMapper` 来忽略 JSON 中的属性：

```java
ObjectMapper mapper = new ObjectMapper()
    .configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false)
```

然后，我们应该能够将这种 JSON 读取到一个预定义的 Java 实体中：

```java
@Test
public void givenJsonHasUnknownValuesButJacksonIsIgnoringUnknowns_whenDeserializing_thenCorrect()
  throws JsonParseException, JsonMappingException, IOException {
    String jsonAsString =
        "{\"stringValue\": \"a\"," +
        "\"intValue\": 1," +
        "\"booleanValue\": true," +
        "\"stringValue2\": \"something\"}";

    ObjectMapper mapper = new ObjectMapper()
        .configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);

    MyDto readValue = mapper.readValue(jsonAsString, MyDto.class);
 
    assertNotNull(readValue);
    assertThat(readValue.getStringValue(), equalTo("a"));
    assertThat(readValue.isBooleanValue(), equalTo(true));
    assertThat(readValue.getIntValue(), equalTo(1));
}
```

!> 如果是 Spring Boot 项目，可以设置 `spring.jackson.deserialization.fail-on-unknown-properties=false` 全局忽略未知属性。

## 处理类上的未知字段

**我们也可以把一个单一的类标记为接受未知字段**，而不是整个 `ObjectMapper`：

```java
@JsonIgnoreProperties(ignoreUnknown = true)
public class MyDtoIgnoreUnknown { ... }
```
