# 使用 ObjectMapper 读写

让我们从基本的读写开始。

`ObjectMapper` 简单的 `readValue` API 是一个很好的切入点。我们可以用它来解析 JSON 内容或将其反序列化为 Java 对象。

此外，在写操作方面，我们可以使用 `writeValue` API 将任何 Java 对象序列化为 JSON 输出。

在本章中，我们将使用下面的 `Car` 类和两个字段作为序列化和反序列化的对象：

```java
public class Car {
    private String color;
    private String type;
    // standard getters setters
}
```

## Java 对象转化为 JSON

让我们看看第一个使用 `ObjectMapper` 的 `writeValue` 方法将 Java 对象序列化为 JSON 的例子：

```java
ObjectMapper objectMapper = new ObjectMapper();
Car car = new Car("yellow", "renault");
objectMapper.writeValue(new File("target/car.json"), car);
```

上述文件的输出将会是：

```json
{"color":"yellow","type":"renault"}
```

`ObjectMapper` 的 `writeValueAsString` 和 `writeValueAsBytes` 方法从 Java 对象生成一个 JSON，
并以字符串或字节数组的形式返回生成的 JSON：

```java
String carAsString = objectMapper.writeValueAsString(car);
```

## JSON 转化为 Java 对象

下面是一个使用 `ObjectMapper` 类将 JSON 字符串转换为 Java 对象的简单示例：

```java
String json = "{ \"color\": \"Black\", \"type\": \"BMW\"}";
Car car = objectMapper.readValue(json, Car.class);
```

## 从 JSON 数组字符串创建 Java List

我们可以使用 `TypeReference` 将数组形式的 JSON 解析为 Java 对象列表：

```java
String jsonCarArray = "[{\"color\": \"Black\", \"type\": \"BMW\"}," +
    "{\"color\": \"Red\", \"type\": \"FIAT\"}]";
List<Car> listCar =
    objectMapper.readValue(jsonCarArray, new TypeReference<List<Car>>(){});
```

## 从 JSON 字符串创建 Java Map

类似地，我们可以将 JSON 解析为 Java Map：

```java
String json = "{\"color\": \"Black\", \"type\": \"BMW\"}";
Map<String, Object> map =
    objectMapper.readValue(json, new TypeReference<Map<String,Object>>(){});
```
