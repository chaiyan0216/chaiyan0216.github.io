---
title: Guava 字符串处理
tags: guava
---

[Guava](<https://github.com/google/guava/wiki>)，包含了若干被Google的Java项目广泛依赖的核心库，例如：集合 [collections] 、缓存 [caching] 、原生类型支持 [primitives support] 、并发库 [concurrency libraries] 、通用注解 [common annotations] 、字符串处理 [string processing] 、I/O 等等。所有这些工具每天都在被Google的工程师应用在产品服务中。

#### 一、连接器[Joiner]

连接器通过分隔符把字符串或者map连接起来，并可以替换或者跳过null。

| 方法                                                | 描述                                          |
| --------------------------------------------------- | --------------------------------------------- |
| Joiner.**on**(char separator)                       | 生成Joiner，连接字符为separator               |
| Joiner.**on**(String separator)                     | 生成Joiner，连接字符串为separator             |
| **skipNulls**()                                     | 跳过null值                                    |
| **useForNull**(String nullText)                     | 用nullText代替null值                          |
| **withKeyValueSeparator**(char keyValueSeparator)   | 生成MapJoiner，k-v连接字符keyValueSeparator   |
| **withKeyValueSeparator**(String keyValueSeparator) | 生成MapJoiner，k-v连接字符串keyValueSeparator |
| **join**(Iterable<?> parts)                         |                                               |
| **join**(Iterator<?> parts)                         |                                               |
| **join**(Object[] parts)                            |                                               |


```java
@Test
public void joinerTest() {
    List<String> list = Lists.newArrayList("China", "Japan", null, "American");
    Map<String, String> map = new HashMap<>();
    map.put("CHN", "China");map.put("JP", "Japan");
    map.put("USA", "American");map.put(null, "Null");

    Joiner joiner = Joiner.on(";").skipNulls();
    String s = joiner.join(list);
    assertEquals("China;Japan;American", s);
    Joiner joiner1 = Joiner.on(";").useForNull("N/A");
    String s1 = joiner1.join(list);
    assertEquals("China;Japan;N/A;American", s1);

    Joiner joiner2 = Joiner.on("; ").useForNull("N/A");
    Joiner.MapJoiner mapJoiner = joiner2.withKeyValueSeparator(" -> ");
    String s2 = mapJoiner.join(map);
    log.info("MapJoiner join {}", s2);
    //[INFO] MapJoiner join {N/A -> Null;USA -> American;JP -> Japan;CHN -> China}
}
```

#### 二、拆分器[Splitter]

JDK内建字符串拆分工具有一些古怪特性。比如，String.split丢弃了字符尾部的空白字符。

``` java
String[] ss = ",a,,b,".split(",");
log.info(Arrays.toString(ss));
//[INFO] [, a, , b]
```

使用拆分器可以避免以上奇怪问题，拆分器通过分隔符对字符串进行切分。

| 方法                                                 | 描述                                       |
| ---------------------------------------------------- | ------------------------------------------ |
| Splitter.**on**(char separator)                      | 生成Splitter，分隔字符为separator          |
| Splitter.**on**(CharMatcher separatorMatcher)        | 生成Splitter，分隔符为guava的CharMatcher   |
| Splitter.**on**(String separator)                    | 生成Splitter，分隔字符串为separator        |
| Splitter.**on**(Pattern separatorPattern)            | 生成Splitter，分隔符为正则表达式           |
| Splitter.**onPattern**(String separatorPattern)      | 生成Splitter，分隔符为正则表达式字符串     |
| Splitter.**fixedLength**(int length)                 | 生成Splitter，使用固定切分长度             |
| **limit**(int limit)                                 | 限制切分数量                               |
| **omitEmptyStrings**()                               | 剔除空字符串                               |
| **trimResults**()                                    | 剔除首尾空格                               |
| **trimResults**(CharMatcher trimmer)                 | 剔除首尾CharMatcher字符                    |
| **withKeyValueSeparator**(char separator)            | 生成MapSplitter，k-v切分字符separator      |
| **withKeyValueSeparator**(String separator)          | 生成MapSplitter，k-v切分字符串separator    |
| **withKeyValueSeparator**(Splitter keyValueSplitter) | 生成MapSplitter，k-v拆分器keyValueSplitter |

```java
@Test
public void splitterTest() {
    String s = ";China; Japan;;American ; France ;";
    List<String> lists = Splitter.on(';').splitToList(s);
    log.info(lists.toString());
    //[INFO] [, China,  Japan, , American ,  France , ]

    lists = Splitter.on(";").trimResults().splitToList(s);
    log.info(lists.toString());
    //[INFO] [, China, Japan, , American, France, ]

    lists = Splitter.on(";").omitEmptyStrings().splitToList(s);
    log.info(lists.toString());
    //[INFO] [China,  Japan, American ,  France ]

    lists = Splitter.on(";").omitEmptyStrings().trimResults().splitToList(s);
    log.info(lists.toString());
    //[INFO] [China, Japan, American, France]
}
```

#### 三、字符匹配器[CharMatcher]

一个CharMatcher代表一类字符。CharMatcher之间可以通过逻辑运算实现交、并、非。

| 方法                                                         | 描述                                                        |
| ------------------------------------------------------------ | ----------------------------------------------------------- |
| CharMatcher.**any**()                                        | 匹配所有字符的匹配器                                        |
| CharMatcher.**anyOf**(CharSequence sequence)                 | 匹配sequence中所有字符的匹配器                              |
| CharMatcher.**ascii**()                                      | ascii码的匹配器                                             |
| CharMatcher.**breakingWhitespace**()                         | 可换行的空白字符的匹配器(                                   |
| CharMatcher.**forPredicate**(Predicate<? super Character> predicate) | 基于lambda表达式的匹配器，Predicate用于分配lambda表达式     |
| CharMatcher.**inRange**(char startInclusive, char endInclusive) | [startInclusive, endInclusive]间所有字符的匹配器            |
| CharMatcher.**is**(char match)                               | 单字符匹配器                                                |
| CharMatcher.**isNot**(char match)                            | 除此单字符的匹配器                                          |
| CharMatcher.**javaIsoControl**()                             | Java控制字符匹配器，\t \n \r                                |
| CharMatcher.**none**()                                       | 无字符的匹配器，匹配不到任何字符                            |
| CharMatcher.**noneOf**(CharSequence sequence)                | 除sequence中字符外，其他字符都匹配的匹配器                  |
| CharMatcher.**whitespace**()                                 | 空格匹配器                                                  |
| **and**(CharMatcher other)                                   | 并                                                          |
| **or**(CharMatcher other)                                    | 交                                                          |
| **negate**()                                                 | 非                                                          |
| **matchesAnyOf**(CharSequence sequence)                      | 匹配器匹配到sequence中任一字符                              |
| **matchesAllOf**(CharSequence sequence)                      | 匹配器匹配到sequence中所有字符                              |
| **matchesNoneOf**(CharSequence sequence)                     | 匹配器未匹配到sequence中任一字符                            |
| **indexIn**(CharSequence sequence)                           | 第一个匹配到的index，无匹配则-1                             |
| **indexIn**(CharSequence sequence, int start)                | 从start开始，第一个匹配到的index，无匹配则-1                |
| **lastIndexIn**(CharSequence sequence)                       | 最后一个匹配到的index，无匹配则-1                           |
| **countIn**(CharSequence sequence)                           | 匹配器从sequence中匹配到的字符数量                          |
| **removeFrom**(CharSequence sequence)                        | 从sequence中去除匹配器中的字符                              |
| **retainFrom**(CharSequence sequence)                        | 从sequence中保留匹配器中的字符                              |
| **replaceFrom**(CharSequence sequence, char replacement)     | 用replacement代替匹配到的字符                               |
| **replaceFrom**(CharSequence sequence, CharSequence replacement) | 用replacement代替匹配到的字符                               |
| **trimFrom**(CharSequence sequence)                          | 删除首尾匹配到的字符                                        |
| **trimLeadingFrom**(CharSequence sequence)                   | 删除首部匹配到的字符                                        |
| **trimTrailingFrom**(CharSequence sequence)                  | 删除尾部匹配到的字符                                        |
| **collapseFrom**(CharSequence sequence, char replacement)    | 连续匹配到的字符，用replacement代替                         |
| **trimAndCollapseFrom**(CharSequence sequence,  char replacement) | 删除首尾匹配到的字符，中间连续匹配到的字符用replacement代替 |

```java
@Test
public void charMatcherTest() {
    String input = "";
    String result = "";

    input = "H*el.lo,}12";
    CharMatcher matcher0 = CharMatcher.forPredicate(Character::isLetter);
    CharMatcher matcher1 = CharMatcher.forPredicate(Character::isLowerCase);
    result = matcher0.and(matcher1).retainFrom(input);
    assertEquals("ello", result);

    input = "H*el.lo,}12";
    CharMatcher matcher = CharMatcher.anyOf("Hel");
    result = matcher.retainFrom(input);
    assertEquals("Hell", result);
    result = matcher.removeFrom(input);
    assertEquals("*.o,}12", result);

    input = "       hel    lo      ";
    result = CharMatcher.is(' ').collapseFrom(input, '-');
    assertEquals("-hel-lo-", result);
    result = CharMatcher.is(' ').trimAndCollapseFrom(input, '-');
    assertEquals("hel-lo", result);
}
```

#### 四、字符集[Charsets]

Charsets针对所有Java平台都要保证支持的六种字符集提供了常量引用。尝试使用这些常量，而不是通过名称获取字符集实例。

| 方法                    | 描述 |
| ----------------------- | ---- |
| Charsets.**ISO_8859_1** |      |
| Charsets.**US_ASCII**   |      |
| Charsets.**UTF_8**      |      |
| Charsets.**UTF_16**     |      |
| Charsets.**UTF_16BE**   |      |
| Charsets.**UTF_16LE**   |      |

#### 五、格式器[CaseFormat]

格式器对字符串进行大小写格式化。

| 方法                                 | 描述                           |
| ------------------------------------ | ------------------------------ |
| LOWER_CAMEL                          | 小驼峰，lowerCamel，方法名     |
| UPPER_CAMEL                          | 大驼峰，LowerCamel，类名       |
| LOWER_HYPHEN                         | 小写+连字符，lower-hyphen      |
| LOWER_UNDERSCORE                     | 小写+下划线，C++变量名         |
| UPPER_UNDERSCORE                     | 大写+下划线，常量名            |
| to(CaseFormat format, String str)    | 将str从当前格式专为目标格式    |
| converterTo(CaseFormat targetFormat) | 返回当前格式到目标格式的转换器 |

```java
@Test
public void caseFormatTest() {
    String input = "";
    String result = "";

    Converter<String, String> camelConverter = CaseFormat.LOWER_CAMEL.converterTo(CaseFormat.UPPER_UNDERSCORE);
    input = "lowerCamel";
    result = camelConverter.convert(input);
    assertEquals("LOWER_CAMEL", result);

    input = "UPPER_UNDERSCORE";
    result = CaseFormat.UPPER_UNDERSCORE.to(CaseFormat.UPPER_CAMEL, input);
    assertEquals("UpperUnderscore", result);
}
```

