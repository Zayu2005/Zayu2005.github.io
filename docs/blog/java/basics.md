---
icon: simple/coffeescript
---

# Java 语法基础

:material-calendar: 2026-03-07 · :material-tag: Java, 基础

---

Java 是一门面向对象的编程语言，以「一次编写，到处运行」著称。本文整理 Java 的核心语法要点。

## 数据类型

Java 是强类型语言，数据类型分为**基本类型**和**引用类型**两大类。

### 基本类型

``` java title="8 种基本数据类型"
// 整数类型
byte   b = 127;          // 1 字节
short  s = 32767;        // 2 字节
int    i = 2147483647;   // 4 字节
long   l = 9999999999L;  // 8 字节

// 浮点类型
float  f = 3.14f;        // 4 字节
double d = 3.14159265;   // 8 字节

// 字符类型
char   c = 'A';          // 2 字节 (Unicode)

// 布尔类型
boolean flag = true;
```

!!! tip "注意"

    `long` 字面量需加 `L` 后缀，`float` 字面量需加 `f` 后缀。

### 引用类型

``` java title="常见引用类型"
String name = "Hello Java";
int[] nums = {1, 2, 3, 4, 5};
List<String> list = new ArrayList<>();
```

## 面向对象

Java 面向对象的三大特性：**封装、继承、多态**。

``` java title="类的定义" hl_lines="5-7"
public class Animal {
    private String name;
    private int age;

    // 构造方法
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void speak() {
        System.out.println(name + " makes a sound");
    }
}
```

``` java title="继承与多态"
public class Dog extends Animal {

    public Dog(String name, int age) {
        super(name, age);
    }

    @Override
    public void speak() {
        System.out.println(getName() + " says: Woof!");
    }
}

// 多态
Animal animal = new Dog("Buddy", 3);
animal.speak(); // Buddy says: Woof!
```

## 集合框架

Java 集合框架是日常开发中最常用的部分之一。

``` mermaid
graph TD
    A[Collection] --> B[List]
    A --> C[Set]
    A --> D[Queue]
    B --> E[ArrayList]
    B --> F[LinkedList]
    C --> G[HashSet]
    C --> H[TreeSet]
    D --> I[PriorityQueue]
    J[Map] --> K[HashMap]
    J --> L[TreeMap]
```

### 常用操作

=== "List"

    ``` java
    List<String> list = new ArrayList<>();
    list.add("Java");
    list.add("Python");
    list.add("Go");

    // 遍历
    for (String item : list) {
        System.out.println(item);
    }

    // Stream API (Java 8+)
    list.stream()
        .filter(s -> s.length() > 3)
        .forEach(System.out::println);
    ```

=== "Map"

    ``` java
    Map<String, Integer> map = new HashMap<>();
    map.put("Java", 1995);
    map.put("Python", 1991);
    map.put("Go", 2009);

    // 遍历
    map.forEach((key, value) ->
        System.out.println(key + ": " + value)
    );

    // 获取值（带默认值）
    int year = map.getOrDefault("Rust", 2010);
    ```

=== "Set"

    ``` java
    Set<String> set = new HashSet<>();
    set.add("Java");
    set.add("Python");
    set.add("Java"); // 重复元素不会被添加

    System.out.println(set.size()); // 2
    System.out.println(set.contains("Java")); // true
    ```

## 异常处理

``` java title="try-catch-finally"
public String readFile(String path) {
    try {
        return Files.readString(Path.of(path));
    } catch (FileNotFoundException e) {
        System.err.println("文件不存在: " + path);
        return "";
    } catch (IOException e) {
        System.err.println("读取失败: " + e.getMessage());
        return "";
    } finally {
        System.out.println("操作完成");
    }
}
```

!!! info "Java 7+ try-with-resources"

    ``` java
    try (var reader = new BufferedReader(new FileReader(path))) {
        return reader.readLine();
    }
    // 自动关闭资源，无需 finally
    ```

## 总结

| 概念 | 要点 |
|------|------|
| 数据类型 | 8 种基本类型 + 引用类型，强类型 |
| 面向对象 | 封装、继承、多态 |
| 集合框架 | List / Set / Map，善用 Stream API |
| 异常处理 | try-catch-finally，优先用 try-with-resources |

Java 的生态非常成熟，掌握好基础语法后，可以继续学习 Spring 框架来构建企业级应用。:rocket:
