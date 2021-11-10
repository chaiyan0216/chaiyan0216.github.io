---
title: 深浅拷贝
typora-root-url: ../../..
---

### 一、什么是深浅拷贝

- 引用复制：复制出来的引用和原引用指向同一实例变量。
- 浅拷贝：拷贝出来的引用和原引用不指向同一实例变量，拷贝出来的引用指向的实例变量的内部引用和原引用指向的实例变量的内部引用指向同一实例变量。
- 深拷贝：对原对象的完全拷贝，包括原对象内部的引用类型。



公共代码：

```java
@Data
@AllArgsConstructor
class Person {
    private String name;
    private Address address;
}

@Data
@AllArgsConstructor
class Address {
    private String city;
    private String road;
}
```



#### 1 引用复制

![image-copy](/images/java-copy.png)

person1、person2指向的内存地址一致：

Person1: com.cy.test.Person@41629346

Person2: com.cy.test.Person@41629346

```java
public class Copy {
    public static void main(String[] args) throws CloneNotSupportedException {
        Address address = new Address("上海", "淮海路");
      
        Person person1 = new Person("人一", address);
        Person person2 = person1;
      
        System.out.println(person1);
        System.out.println(person2);
    }
}
```



#### 2 浅拷贝

![image-copy1](/images/java-copy1.png)

person1、person2的地址相同：

Person(name=人一, address=Address(city=上海, road=**南京路**))

Person(name=人二, address=Address(city=上海, road=**南京路**))

```java
public class ShallowCopy {
    public static void main(String[] args) throws CloneNotSupportedException {
        Address address = new Address("上海", "淮海路");

        Person person1 = new Person("人一", address);
        Person person2 = (Person) person1.clone();
        person2.setName("人二");
        person2.getAddress().setRoad("南京路");
      
        System.out.println(person1);
        System.out.println(person2);
    }
}

class Person implements Cloneable {
    private String name;
    private Address address;

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```



#### 3 深拷贝

![image-copy2](/images/java-copy2.png)

person1、person2的地址不同：

Person(name=人一, address=Address(city=上海, road=**淮海路**))

Person(name=人二, address=Address(city=上海, road=**南京路**))

```java
public class DeepCopy {
    public static void main(String[] args) throws CloneNotSupportedException {
        Address address = new Address("上海", "淮海路");

        Person person1 = new Person("人一", address);
        Person person2 = (Person) person1.clone();
        person2.setName("人二");
        person2.getAddress().setRoad("南京路");
      
        System.out.println(person1);
        System.out.println(person2);
    }
}

class Person implements Cloneable {
    private String name;
    private Address address;

    @Override
    protected Object clone() throws CloneNotSupportedException {
        Person person = new Person(name, new Address(address.getCity(), address.getRoad()));
        return person;
    }
}
```





### 二、深拷贝实现

#### 1 Cloneable接口

实现 **Cloneable** 接口让类可以被拷贝。默认 **clone** 方法里的实现是 **super.clone()**，是浅拷贝。

重写 clone 方法，在方法体内生成新的内部实例对象，就可以实现深拷贝。但这种方法遇到多层引用时，会比较繁琐，每个类都需要实现 Cloneable 接口，并在 clone 方法体内生成新的内部引用。

#### 2 序列化

实例对象先序列化再反序列化就可以避免对相同实例对象的引用，需要实现序列化接口。可以直接使用 Apache Commons Lang 的 **SerializationUtils** 进行 clone。

```java
public class DeepCopy {
    public static void main(String[] args) throws CloneNotSupportedException {
        Address address = new Address("上海", "淮海路");

        Person person1 = new Person("人一", address);
        Person person2 = SerializationUtils.clone(person1);
        person2.setName("人二");
        person2.getAddress().setRoad("南京路");

        System.out.println(person1);
        System.out.println(person2);
    }
}

@Data
@AllArgsConstructor
class Person implements Serializable {
    private String name;
    private Address address;
}

@Data
@AllArgsConstructor
class Address implements Serializable {
    private String city;
    private String road;
}
```

#### 3 Json

实例对象转为 json，再从 json 转回对象也可以避免对相同实例对象的引用，而且不用实现任何接口。

```java
public class DeepCopy {
    public static void main(String[] args) throws CloneNotSupportedException {
        Address address = new Address("上海", "淮海路");

        Person person1 = new Person("人一", address);
        Gson gson = new Gson();
        Person person2 = gson.fromJson(gson.toJson(person1), Person.class);
        person2.setName("人二");
        person2.getAddress().setRoad("南京路");

        System.out.println(person1);
        System.out.println(person2);
    }
}

@Data
@AllArgsConstructor
class Person {
    private String name;
    private Address address;
}

@Data
@AllArgsConstructor
class Address {
    private String city;
    private String road;
}
```