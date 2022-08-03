# Jackson MixIn 注解

接下来让我们看看如何使用 Jackson MixIn 注解。

让我们使用 MixIn 注解，例如忽略 `User` 类型的属性：

```java
public class Item {
    public int id;
    public String itemName;
    public User owner;
}

@JsonIgnoreType
public class MyMixInForIgnoreType {
}
```

有时候，我们使用了第三方的实体类，这种情况下我们无法将注解添加到第三方的实体类上，那么 MixIn 就发挥作用了。让我们看看如何使用：

```java
@Test
public void whenSerializingUsingMixInAnnotation_thenCorrect()
  throws JsonProcessingException {
    Item item = new Item(1, "book", null);
    
    String result = new ObjectMapper().writeValueAsString(item);
    assertThat(result, containsString("owner"));
    
    ObjectMapper mapper = new ObjectMapper();
    mapper.addMixIn(User.class, MyMixInForIgnoreType.class);
    
    result = mapper.writeValueAsString(item);
    assertThat(result, not(containsString("owner")));
}
```

!> 示例中仅仅使用了 `@JsonIgnoreType`，你也可以使用多个注解进行组合，或者与上一个章节中的知识点结合使用。
