---
title: Spring MVC 使用@ResponseBody注解返回Json

date: 2017-06-25 21:25:30
categories:
tags: [Spring,SpringMVC,序列化]
---
目前我所知道的，使用Spring MVC返回Json数据有三种方式：

> 1. 配置一个JsonView，需要用到jackson的jar包，是Spring2时代的产物；
> 2. 使用json工具将数据序列化成json，然后使用 `HttpServletResponse` 直接输出，常用的工具有：gson、fastjson、jackson等；
> 3. 利用Spring MVC的注解 `@ResponseBody` 注解。

今天主要说说第三种方式，用代码说话：

```java
@ResponseBody
@RequestMapping(value = "/detail", produces = "application/json;charset=UTF-8")
public Computer detail() {
    final Computer computer = new Computer();
    computer.setModel("MacBook Pro");
    computer.setWeight(1.3);
    computer.setProductionDate(new Date(2017, 1, 1));
    computer.setSellingTime(new Date());
    return computer;
}
```

上面这段代码的返回结果如下：

```json
{
    "model":"MacBook Pro",
    "weight":1.3,
    "productionDate":61444022400000,
    "sellingTime":1490257377531
}
```

那么，现在需求来了。为了节省带宽，需要把json中数据的key使用缩写，但又不影响实体类的使用。
如果直接修改实体类中的属性名称的话，会试代码维护变得非常艰难，数据拷贝的时候需要写一大堆Setter和Getter，代码可读性大大下降。
大家都知道，使用Gson等序列化工具时，可以使用 `@SerializedName` 注解，为属性指定序列化时的名称，那么这里使用 `@SerializedName` 注解可以做到吗？答案肯定是不行的。但是jackson包为我们提供了类似的注解@JsonProperty，使用方法和 `@SerializedName` 完全一样，只需要给实体类中所有的属性添加相应的注解。

```java
private class Computer {
    @JsonProperty("m")
    private String model;
    @JsonProperty("w")
    private Double weight;
    @JsonProperty("pd")
    private Date productionDate;
    @JsonProperty("st")
    private Date sellingTime;
    //****** getters and setters *****
}
```

得到的结果如下：

```json
{
    "m":"MacBook Pro",
    "w":1.3,
    "pd":61444022400000,
    "st":1490258203258
}
```

序列化的时候，是将时间转成了毫秒数，这就需要前台或者其他调用者做进一步处理，一般都是在序列化的时候将时间属性格式化后再输出。生产日期 `productionDate` 应该只到日，不应该再有 `00:00:00` 输出了，而售出时间 `sellingTime` 则需要精确到秒。我们可以自行定义序列化和反序列化方式，分别继承抽象类 `JsonSerializer` 和 `JsonDeserializer` ，并实现其中的抽象方法，然后在相应的字段上添加注解 `@JsonSerialize(using = DateSerializer.class)` 指定我们定义的类就OK了。如：

```java
/**
 * 序列化是将时间格式化为 yyyy-MM-dd 形式
 */
public class DateSerializer extends JsonSerializer<Date> {
    @Override
    public void serialize(final Date date,
                          final JsonGenerator jsonGenerator,
                          final SerializerProvider serializerProvider)
            throws IOException, JsonProcessingException {
        jsonGenerator.writeString((new SimpleDateFormat("yyyy-MM-dd")).format(date));
    }
}
/**
 * 序列化是将时间格式化为 yyyy-MM-dd HH:mm:ss 形式
 */
public class DateTimeSerializer extends JsonSerializer<Date> {
    @Override
    public void serialize(final Date date,
                          final JsonGenerator jsonGenerator,
                          final SerializerProvider serializerProvider)
            throws IOException, JsonProcessingException {
        jsonGenerator.writeString((new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"))
            .format(date));
    }
}

```

然后在相应的实体类的属性上添加注解：


```java
private class Computer {
    @JsonProperty("m")
    private String model;
    @JsonProperty("w")
    private Double weight;
    @JsonProperty("pd")
    @JsonSerialize(using = DateSerializer.class
    private Date productionDate;
    @JsonProperty("st")
    @JsonSerialize(using = DateTimeSerializer.class
    private Date sellingTime;
    //****** getters and setters *****
}
```

得到的结果为：

```json
{
    "m":"MacBook Pro",
    "w":1.3,
    "pd":"2017-01-01",
    "st":"2017-03-23 17:52:41"
}
```

当然，jackson提供的不止这些，比如：`@JsonIgnore`、`@JsonPropertyOrder`等，都是非常实用的。
