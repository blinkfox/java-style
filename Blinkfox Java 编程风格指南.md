# Blinkfox Java 编程风格指南

- [新增]新增了类成员的排列顺序约定。（**新增原因**：相比 Google 风格指南文档中“模糊而又正确”的说法，采用了更为“确定性、可明确实施的”约定。）
- [新增]不使用行尾注释，建议在代码行或逻辑行之上做单行注释即可。（**新增原因**：不容易首先阅读到，且行尾注释会占列长度，且容易导致超过列长度限制，后期维护注释麻烦。）
- [修改]如果空语句块，也需要遵守 `K&R` 风格，且增加了语句为空的理由注释；（**修改原因**：更加明确的规范，利于大家写出“更加一致”的代码，且有助于后来人理解为什么要保持语句块是空的。）
- [修改]缩进空格数改为了 `4` 个空格；（**修改原因**：根据我实际的编程经验，`2` 个空格的代码缩进，在当前大屏幕流行的时代，会显得十分拥挤（挤成一坨），反而使得代码`臃肿`不够美观。所以，我这里建议使用`4`个空格来缩进，会使得更加美观、协调，而且能侧面督促开发人员减少代码的嵌套层数。）
- [修改]列长度限制改为了 `120` 个字符；（**修改原因**：Java 相比其他编程语言而言，类和方法的命名都要“长”很多，这样所占的列数就更多，如果列限制仅设置为 `100` 的话，代码被折行的几率会更大。且在当前大屏幕流行的时代，屏幕右边会空出一大部分空间并没有被代码真正的填充，简直就是浪费，而且使得阅读代码时，视线会重复“跳行”。）
- [修改]枚举实例之间需要换行，且附加文档注释；（**修改原因**：枚举本质上也是类，所以，类中的很多规则也适用于枚举。由于枚举实例都是 `public static final` 的，所以，也必然需要添加 Javadoc 注释，所以，实例与实例之间需要使用换行来分割。）
- [修改]注解应用到类、属性、构造方法、方法上时，应紧接 Javadoc 之后，且独立成行。如果注解应用在参数或者局部变量上时，建议与他们写到一行。（**修改原因**：易于识别和阅读。）

## 1 前言

这份文档是 Blinkfox 根据 [Google Java 编程风格](https://checkstyle.sourceforge.io/styleguides/google-java-style-20180523/javaguide.html#s3.3.3-import-ordering-and-spacing)规范而根据自身使用情况而修改的一份 Java 编程风格指南。本文档相对于 Google 的 Java 编程风格文档而言，修改了部分规范，增加了一些更加确定性的规则说明，这样能最大程度上的让代码风格统一、易读。且本文档的语言描述会更加的精简，文档最后也会罗列一些具体的新增或修改点。

## 2 源文件基础

- 源文件名必须与顶级（最外层的）类的名称相同，且使用大驼峰（`UpperCamelCase`）的方式来命名，源文件扩展名为 `.java`。
- 源文件使用 **`UTF-8`** 编码。
- 源文件中只能使用**换行符**和**空格符**（ASCII 字符：`0x20`）作为空白符，**禁止使用 `Tab` 制表符**来用于缩进。
- 对于具有[特殊转义序列](http://docs.oracle.com/javase/tutorial/java/data/characters.html)（例如：`\b`, `\t`, `\n`, `\f`, `\r`, `\"`, `\'`, `\\`等）的字符，都采用这种转义字符的方式表示，而不采用对应字符的八进制数（如：`\012`）或 Unicode （如：`\u000a`）来表示。
- 对于其余的非 ASCII 字符，就直接使用 Unicode 字符即可（如：`∞`）

## 3 源文件结构

### 3.1 结构组成

- 源文件按**从上到下的顺序**，由以下几部分组成，且每个部分之间应该**只有一个空行**作为分隔。：
  1. 许可证 (`License`）或版权信息（`copyright`）（该组成部分是可选的）
  2. 包声明（`package`）语句
  3. 包导入（`import`）语句
  4. 类（`class`）声明
- 如果源文件中需要包含许可证或版权信息，那么应该把它放在文件最前面。
- 包声明（`package`）语句和包导入（`import`）语句不受制于列长度限制，都是独立成行。
- 包导入（`import`）语句中**都不使用通配符**），不管是否是静态导入（`static import`）。
- 包导入（`import`）语句按从上到下的顺序分为**静态导入**（`static import`）块和**非静态导入**块两部分，如果同时具有静态和非静态导入，则**使用一个空行将两块分开。每个块的若干导入语句之间不空行，且导入的顺序按名称的 ASCII 顺序排序**。
- 静态内部类中不使用静态导入（`static import`）。
- 每个源文件中只能有一个顶级类（`class`）声明。

### 3.2 类成员顺序

- 类成员的顺序对代码的易读性有很大的影响，这里归纳总结了一些类成员从上到下的建议排列顺序：
  1. 具有 `static final` 关键字修饰的属性；
  2. 具有 `static` 关键字修饰的属性；
  3. 普通字段属性从上到下再按 `public`、`protected`、`default`、`private` 的访问级别顺序排列；
  4. 同访问级别的属性之间再按引用对象类型和原始类型（或包装）类型的顺序排列；
  5. `static {}` 初始化块；
  6. 一个或多个构造方法，从上到下按参数由少到多的顺序排列；
  7. 其他方法；
- **重载方法必须按顺序排列在一起**，中间不要插入其它方法或私有成员。

## 4 格式

### 4.1 语句块的格式

- 在 `if`, `else`, `for`, `do` 和 `while` 等语句中，即使只有一条语句或者内容为空时，也需要写上大括号。
- **对于语句块，大括号都遵循 `K&R` 风格**（Kernighan 和 Ritchie 风格）：
  - 左大括号前不换行
  - 左大括号后换行
  - 右大括号前换行
  - 如果右大括号结束是一个语句块或者方法体、构造函数体或者有命名的类体，则需要换行。当右括号后面接 `else` 或逗号（`,`）时，不换行。示例：

```java
return () -> {
    while (condition()) {
        method();
    }
};

return new MyClass() {
    @Override public void method() {
        if (condition()) {
            try {
                something();
            } catch (ProblemException e) {
                recover();
            }
        } else if (otherCondition()) {
            somethingElse();
        } else {
            lastThing();
        }
    }
};
```

- **对于空语句块，也需要遵循 `K&R` 风格**，如果是**无参构造方法**，内容可以保持为空，或者使用 `super();` 填充内容，或者默认不写该构造方法。如果是其他类型的空方法或空语句块，需要在空语句块中使用注释说明为什么是空的。示例：

```java
// 这是不可以的
void doNothing() {}

// 这是不可以的
void doNothingElse() {
}

// 这是可以的
void doNothingElse() {
    // 这里需要写明内容为空的现实原因和真正意图.
}
```

### 4.2 缩进机制

- **使用 `4` 个空格作为基础尺度来缩进代码**。即每产生一个新的语句块，缩进就增加 `4` 个空格。当这个语句块结束时，缩进恢复到上一层级的缩进级别。此缩进要求对整个语句块中的代码和注释都适用。
- **每条语句结束都需要换行，换行缩进需 `+8` 个空格**。一条语句自动换行时，第一行后的每一行都比上一行多缩进 `8` 个空格。如果两个连续行中使用了同级别的语法元素（如：`.` 操作符），则他们要使用相同的缩进。

代码示例如下：

```java
// 下面的示例演示了一条语句换行时的缩进为 8 个空格.
private void assertMethod(int[] array1, int[] array2, boolean expected) {
    Assertions.assertEquals(expected,
        new LongLongLongTestClassName().callLongLongLongTestMethodName(array1, array2));
}
```

### 4.3 列长度限制

- 代码的**列长度限制为 `120` 个字符**，该“字符”表示任何 Unicode 代码点。但也有一些例外情况：
  - Javadoc 文档注释中的一个长 URL；
  - 如前面所述的 `package` 和 `import` 语句；
  - 注释中的命令行代码，便于剪切粘贴到 shell 中；

### 4.4 换行机制

- 换行有助于使代码更清晰、易读，换行的主要原则是：**选择在更高级的语法逻辑处换行**。包括下面的一些情形：
  - 当在一个非赋值运算的语句换行时，在运算符号之前换行，如：在字符串拼接的 `+` 号前换行。下面的情况也类似：
    - 在点（`.`）操作符之前换行
    - 在方法引用中双冒号（`::`）操作符之前换行
    - 在与或逻辑运算符（`&&`、`||`）之前换行
    - 在 `catch` 块中的管道符号之前换行（`catch (FooException | BarException e)`）
  - 当要在赋值运算符语句处换行时，一般在赋值符号之后换行（即 `=` 号之后换行）。
  - 在增强 for 循环（`foreach`）语句中的冒号（`:`）之后换行。
  - 方法名或构造函数名与仍然左括号（`(`）留在同一行。
  - 逗号（`,`）与其前面的内容留在同一行。
  - 在 `Lambda` 表达式中的箭头旁边永远不要换行，除非当 `Lambda` 表达式的主体由单个无括号的表达式组成时，可能会在箭头之后换行。示例：

```java
MyLambda<String, Long, Object> lambda =
        (String label, Long value, Object obj) -> {
            ...
        };

Predicate<String> predicate = str ->
        longExpressionInvolving(str);
```

- **类成员之间需要单个空行隔开**：例如：字段，构造函数，方法，内部类，静态初始化块，实例初始化块。在类的第一个成员之前或最后一个成员之后，使用空行。两个连续属性之间的空行是可选的，根据需要使用空行来创建字段间的逻辑分组。但是，如果是业务属性字段，强烈建议对其进行文档注释，且这些业务属性字段之间也需要空行。
- **一个空行也可以出现在任何提高可读性的地方**，例如，在一个方法或代码块中，不同的子逻辑之间用空行隔开。
- 不要使用多个连续的空行，**最多只能空一行**。

### 4.5 空格机制

除了语法、其他规则、词语分隔、注释和 Javadoc 外，水平的 ASCII 空格只在以下情况出现：

- 所有保留的关键字与紧接它之后同一行的左小括号（`(`）之间需要用空格隔开，例如：`if`, `for` `catch`。
- 所有保留的关键字与在它之前的右大括号（`}`）之间需要空格隔开，例如：`else`、`catch`。
- 在左大括号之前都需要空格隔开。
  - **例外情况**：`@SomeAnnotation({a, b})`(不使用空格）。
  - **例外情况**：`String[][] x = {{"foo"}};`（第二个 `{` 之前不使用空格）。
- 所有的二元和三元运算符的两边，都需要使用空格隔开。以下“类似操作符”的情况也适用：
  - 连接泛型类型的 `&` 符号：`<T extends Foo & Bar>`
  - 处理多个异常 `catch` 中的管道操作符：`catch (FooException | BarException e)`
  - 增强 `for` 循环（`foreach`）中的冒号：`:`
  - `Lambda` 表达式中的箭头符号（`->`）
  例外情况：
  - 方法引用中的双冒号（`::`），写法类似于 `Object::toString`
  - 点号操作符（`.`），写法类似于 `object.toString()`
- 逗号(`,`)、冒号(`:`)、分号(`;`)和右小括号(`)`)、Lambda箭头符号(`->`)之后，需要空格隔开。
- 使用`//`双斜线开始一行注释时，双斜线两边都应该用空格隔开。并且可使用多个空格。（可选的，例如：`a = 0; // 赋值为0`）
- 在声明的变量类型和变量名之间需要用空格隔开：`List<String> list`
- 初始化一个数组时，大括号内的空格是可选的。
  - `new int[] {5, 6}` 和 `new int[] { 5, 6 }` 这两种都是可以的
- 在类型注解和 `[]` 或者 `...` 之后使用空格。

示例如下：

```java
/**
 * 这是一个用来演示代码格风格和格式的测试示例类.
 *
 * <p>注意：你不需要去管代码逻辑，着重看代码块、缩进、每一个空行以及空格等的使用.</p>
 *
 * @author intelij idea on 2019-09-25.
 * @author blinkfox on 2020-03-18.
 * @since v1.2.0
 */
@Slf4j
@Annotation(param1 = "value1", param2 = "value2")
@SuppressWarnings( {"ALL"})
public class Foo<T extends Bar & Abba, U> {

    /**
     * 这是一个用于 xxxx 目的的数组.
     */
    public static final int[] X = new int[] {1, 3, 5, 6, 7, 87, 1213, 2};

    private int[] empty = new int[] {};

    /**
     * 这是一个用于测试的测试示例方法，你不需要去管代码逻辑.
     *
     * @param x 变量 x
     * @param y 变量 y
     * @author intelij idea on 2019-09-25.
     * @since v1.2.0
     */
    public void foo(int x, int y) {
        Runnable run1 = () -> {
            System.out.println("Hello World!");
            System.out.println("Test Lambda.");
        };
        Runnable run2 = this::bar;

        // 遇到晦涩难懂的代码，你需要写明注释，为什么要这么写.
        for (int i = 0; i < x; i++) {
            y += (y ^ 0x123) << 2;
        }

        // 这是这下面这一块儿处理逻辑的注释.
        try (MyResource r1 = getResource();
                FileInputStream r2 = new FileInputStream(r1);
                BufferedOutputStream r3 = new BufferedOutputStream(r2)) {
            if (0 < x && x < 10) {
                while (x != y) {
                    x = f(x * 3 + 5);
                }
            } else {
                // 一些重要的业务逻辑处理，你也需要写明注释.
                synchronized (this) {
                    switch (e.getCode()) {
                        //...
                    }
                }
            }
        } catch (MyException | MyException2 e) {
            log.error("This is your error msg.", e);
        } catch (YourException expected) {
            // 如果异常被命名为 'expected' 或者 'ignore'，则意味着可以不处理，但需用注释写明原因.
        } finally {
            int[] arr = (int[]) g(y);
            x = y >= 0 ? arr[y] : -1;
            Map<String, String> sMap = new HashMap<String, String>();
            Bar.<String, Integer>mess(null);
        }

        // 这是一个新的子逻辑代码块，也需要写明注释.
        while (true) {
            if (empty.length == 0) {
                break;
            }
        }
    }

}

```

### 4.6 不使用水平对齐

- **尽量不使用水平对齐**。水平对齐是指通过添加多个空格，使本行的某一符号与上一行的某一符号上下对齐。以下是没有水平对齐和做了水平对齐的例子：

```java
// 不使用水平对齐.
private int x; // 这种挺好
private Color color; // 同上

// 使用了水平对齐.
private int   x;     // 不推荐，未来变量名修改了，会继续编辑，增加了维护难度.
private Color color; // 可能会使它对不齐
```

> **提示**：水平对齐虽然可以提高代码的可读性，但是增加了未来维护代码的麻烦。考虑到维护时只需要改变一行代码，之前的对齐可以不需要改动。为了对齐，你更有可能改了一行代码，同时需要更改附近的好几行代码，而这几行代码的改动，可能又会引起一些为了保持对齐的代码改动。那原本这行改动，我们称之为**爆炸半径**。这种改动，在最坏的情况下可能会导致大量的无意义的工作，即使在最好的情况下，也会影响版本历史信息，减慢代码 `review` 的速度，引起更多合并代码的冲突。

### 4.7 其他格式

- **推荐使用分组小括号**。除非作者和审阅人（`reviewer`）都认为去掉小括号也不会使代码被误解，或是去掉小括号能让代码更易于阅读，否则我们不应该去掉小括号。
- **枚举常量间用逗号隔开，多个枚举实例之间需要空行**。由于每个枚举实例默认都是 `public static final` 的，所以也需要附加文档注释，且需要换行，并添加额外的空行。示例如下：

```java
public enum MyEnum {

    /**
     * 枚举实例 1 的注释.
     */
    INSTANCE1,

    /**
     * 枚举实例 2 的注释.
     */
    INSTANCE2,

    /**
     * 枚举实例 3 的注释.
     */
    INSTANCE3

    // 其他方法...
}
```

- **每个变量声明（字段或局部变量）只声明一个变量**，不使用诸如 `int a, b;` 之类的声明。
  - **例外情况**: `for` 循环的初始化头中可以使用多个变量声明。如：`for (int i = 0, len = arr.length; i < len; i++) {`
- **局部变量在需要使用它的地方再声明**。不要在代码块的开头把所有局部变量一次性全部进行声明（不同于 C 语言），而是在第一次需要使用它时才声明，这样能最大程度地减小其作用范围。局部变量在声明时最好就进行初始化，或者声明后尽快进行初始化。
- 数组的类型不同于 C 语言，中括号是类型的一部分：`String[] args`， 而非 `String args[]`。
- switch 大括号之后缩进 `4` 个字符。每个 switch `case` 语句之后，有一个换行符，并按照大括号相同的处理方式缩进 `4` 个字符。在某个 `case` 结束后，恢复到之前的缩进，类似大括号的结束符。
- 每个 switch 语句中，都需要显式声明 `default` 语句，即使没有任何代码。
- 注解应用到类、属性、构造方法、方法上时，应紧接 Javadoc 之后，且独立成行。如果注解应用在参数或者局部变量上时，建议与他们写到一行。示例：

```java
// 推荐写法. 多个注解独立成行。
@Override
@Nullable
public String getNameIfPresent() { ... }

// 不推荐. (注解和成员在同一行了)
@Override public int hashCode() { ... }

// 不推荐. (多个注解放在了同一行了)
@Partial @Mock
private DataLoader loader;

// 推荐写法，注解修饰参数或局部变量时，放在同一行.
public ModelAndView getUserInfo(
        @RequestParam("userId") String userId,
        @RequestParam("userName") String userName) {
    // this is content.
}
```

- 长整型（`long`）的数字字面量使用大写的 `L` 作为后缀，不得使用小写（避免与数字 `1` 混淆）。例如：使用 `3000000000L`，而不是 `3000000000l`。
- **不使用行尾注释**，建议在代码行或逻辑行之上做单行注释即可。
- 类和成员变量的修饰符，按 `Java Lauguage Specification` 中介绍的先后顺序排序。具体是：

```java
public protected private abstract default static final transient volatile synchronized native strictfp
```

## 5 命名

### 5.1 对所有标识符都通用的规则

标识符只能使用 ASCII 字母和数字，并且在以下少数情况下才使用下划线。因此每个有效的标识符名称都能使用正则表达式 `\w+` 来匹配。

在 Google 编程风格中不使用特殊的前缀或后缀，如`name_`, `mName`, `s_name` 或 `kName`。

### 5.2 标识符类型的规则

#### 5.2.1 包名

包名全部小写，连续的单词只是简单地连接起来，不使用下划线。例如：使用 `com.example.deepspace`，而不是 `com.example.deepSpace` 或者 `com.example.deep_space`。

#### 5.2.2 类名

类名都以大驼峰（[UpperCamelCase](#)）风格编写。

类名通常是名词或名词短语。例如：`Character` 或者 `ImmutableList`。接口名称也可以是名词或名词短语（例如：`List`），但有时可能是形容词或形容词短语（例如：`Readable`）。

注解的命名还没有特定的规则，甚至没有完善的约定。

测试类的命名以它要测试的类的名称开始，以 `Test` 结束。例如：`HashTest` 或 `HashIntegrationTest`。

#### 5.2.3 方法名

方法名都以小驼峰（[lowerCamelCase](#)）风格编写。

方法名通常是动词或动词短语。例如：`sendMessage` 或者 `stop`。

下划线可能出现在 JUnit 测试方法名称中，用以分隔名称的逻辑组成，每个组成部分均用 [lowerCamelCase](#) 编写。一个典型的模式是：`<MethodUnderTest>_<state>`，例如：`Pop_emptyStack`。并不存在唯一正确的方式来命名测试方法。

#### 5.2.4 常量名

常量名的命名模式为 `CONSTANT_CASE`，字母全部大写，使用下划线来分隔单词。那到底什么算是一个常量呢？

每个常量都是一个静态 `final` 字段，其内容是不可变的，且没有可检测的副作用。这包括原始类型、字符串、不可变类型和不可变类型的不可变集合。如果任何一个实例的观测状态是可变的，则它肯定不会是一个常量。不可变对象不一定是常量。例如：

```java
// 下面是常量.
static final int NUMBER = 5;
static final ImmutableList<String> NAMES = ImmutableList.of("Ed", "Ann");
static final ImmutableMap<String, Integer> AGES = ImmutableMap.of("Ed", 35, "Ann", 32);
static final Joiner COMMA_JOINER = Joiner.on(','); // because Joiner is immutable
static final SomeMutableType[] EMPTY_ARRAY = {};
enum SomeEnum { ENUM_CONSTANT }

// 下面的情况不是常量.
static String nonFinal = "non-final";
final String nonStatic = "non-static";
static final Set<String> mutableCollection = new HashSet<String>();
static final ImmutableSet<SomeMutableType> mutableElements = ImmutableSet.of(mutable);
static final ImmutableMap<String, SomeMutableType> mutableValues =
    ImmutableMap.of("Ed", mutableInstance, "Ann", mutableInstance2);
static final Logger logger = Logger.getLogger(MyClass.getName());
static final String[] nonEmptyArray = {"these", "can", "change"};
```

这些常量的名字通常是名词或名词短语。

#### 5.2.5 非常量字段名

非常量字段名以小驼峰（`lowerCamelCase`）风格命名。

这些名字通常是名词或名词短语。例如：`computedValues` 或者 `index`。

#### 5.2.6 参数名

参数名以小驼峰（`lowerCamelCase`）风格命名。

在公共方法中应该避免用单个字符来对参数进行命名。

#### 5.2.7 局部变量名

局部变量名以小驼峰（`lowerCamelCase`）风格命名。

即使局部变量是 `final` 和不可改变的，也不应该把它示为常量，当然也就不能用常量的规则去命名它。

#### 5.2.8 泛型名

泛型可以用以下两种风格之一进行命名：

- 单个的大写字母，后面可以视具体情况跟一个数字（如：`E`, `T`, `X`, `T2`）。
- 以类命名方式(5.2.2 节，[类名](#))，后面加个大写的 `T`（如：`RequestT`, `FooBarT`）。

### 5.3 驼峰命名法（CamelCase）的定义

**驼峰式命名法**分大驼峰式命名法（`UpperCamelCase`）和小驼峰式命名法（`lowerCamelCase`）。有时，我们有多种合理的方式将一个英语词组转换成驼峰形式，如缩略语或不寻常的结构，例如：`IPv6` 或 `iOS`。Google 风格指定了以下的（几乎）确定性的转换方案。

名字从散文形式（`prose form`）开始:

1. 把短语转换为纯 ASCII 码，并且移除任何单引号。例如：`Müller’s algorithm` 将变成 `Muellers algorithm`。
2. 把这个结果切分成单词，在空格或其它标点符号（通常是连字符）处分割开。
  - **推荐**：如果某个单词已经有了常用的驼峰表示形式，按它的组成将它分割开(如`AdWords`将分割成`ad words`)。需要注意的是 `iOS` 并不是一个真正的驼峰表示形式，因此该推荐对它并不适用。
3. 现在将所有字母都小写(包括缩写)，然后将单词的第一个字母大写：
  - 每个单词的第一个字母都大写，来得到大驼峰式命名。
  - 除了第一个单词，每个单词的第一个字母都大写，来得到小驼峰式命名。
4. 最后将所有的单词连接起来得到一个标识符。

请注意，几乎完全忽略了原始单词的大小写。示例：

| 散文形式  | 正确  | 不正确  |
| ------------ | ------------ | ------------ |
| "XML HTTP request"  | XmlHttpRequest  | XMLHTTPRequest  |
| "new customer ID"  | newCustomerId  | newCustomerID  |
| "inner stopwatch"  | innerStopwatch  | innerStopWatch  |
| "supports IPv6 on iOS?"  | supportsIpv6OnIos  | supportsIPv6OnIOS  |
| "YouTube importer"  | YouTubeImporter YoutubeImporter*  | 无   |

加 `*` 号处表示可以接受，但不推荐。

> **注意**：在英语中，某些带有连字符的单词形式不唯一。例如：`nonempty` 和 `non-empty` 都是正确的，因此方法名`checkNonempty`和`checkNonEmpty`也都是正确的。

## 6 编程实践

### 6.1 总是使用 @Override 注解

只要是合法的方法，就要加上 `@Override` 注解，这包括重写超类方法的类方法，实现接口方法的类方法以及重新指定父类接口方法的接口方法。

**例外**：如果父方法为 `@Deprecated` 时，可以省略 `@Override`。

### 6.2 不能忽视捕获的异常

除了下面的例子，对捕获的异常不做任何响应是极少的。(典型的响应方式是打印日志，或者如果它被认为是不可能的，则把它当作一个 `AssertionError` 重新抛出。)

如果它确实是不需要在 `catch` 块中做任何响应，需要做注释加以说明。

```java
return handleTextResponse(response);
try {
  int i = Integer.parseInt(response);
  return handleNumericResponse(i);
} catch (NumberFormatException ok) {
  // 它不是一个数字，不过没关系，继续后续操作
}
return handleTextResponse(response);
```

**例外**：在测试中，如果一个捕获的异常被命名为 `expected`，则它可以被不加注释地忽略。下面是一种非常常见的情形，用以确保所测试的方法会抛出一个期望中的异常， 因此在这里就没有必要加注释。

```java
try {
    emptyStack.pop();
    fail();
} catch (NoSuchElementException expected) {
}
```

### 6.3 使用类来调用静态成员

使用类名调用静态的类成员，而不是具体某个对象或表达式。

```java
Foo aFoo = ...;
Foo.aStaticMethod(); // good
aFoo.aStaticMethod(); // bad
somethingThatYieldsAFoo().aStaticMethod(); // very bad
```

### 6.4 禁用 Finalizers

极少会去重写 `Object.finalize`。

> **提示**：不要这么做。如果你非要使用它，请先仔细阅读和理解 [Effective Java](http://books.google.com/books?isbn=8131726592) 第 7 条：“Avoid Finalizers”，然后不要使用它。

## 7 Javadoc

### 7.1 格式

#### 7.1.1 一般形式

Javadoc 块的基本格式如下所示：

```java
/**
 * Multiple lines of Javadoc text are written here,
 * wrapped normally...
 */
public int method(String p1) { ... }
```

或者是以下单行形式：

```java
/** 一小段的 Javadoc. */
```

基本格式总是可以接受的。当整个 Javadoc 块可以放在一行时(包括注释标签)，就可以使用单行形式。请注意，这仅适用于没有 `@return` 等块标记的情况。

#### 7.1.2 段落

是只包含最左侧星号（`*`）的空行会出现在段落之间和 Javadoc 注解标记（`@XXX`）之前（如果有的话）。除了第一个段落，每个段落第一个单词前都有标签`<p>`，并且它和第一个单词间没有空格。

#### 7.1.3 Javadoc 标记

标准的 Javadoc 注解标记按以下顺序出现：`@param`, `@return`, `@throws`, `@deprecated`, 前面这 4 种标记如果出现，描述都不能为空。当描述无法在一行中容纳，连续行需要至少在 `@` 符号的位置处缩进 `4` 个空格。

#### 7.2 摘要片段

每个 Javadoc 都以一个简短的**摘要片段**开始。这个片段是非常重要的，它是文本在某些上下文（如类和方法索引）中出现的唯一部分。

这只是一个小片段，可以是一个名词短语或动词短语，但不是一个完整的句子。它不会以 `A {@code Foo} is a...` 或者 `This method returns...` 开头, 它也不会是一个完整的祈使句，如 `Save the record.`。但是，由于开头大写且被加了标点符号，它看起来就像是个完整的句子。

> **提示**：一个常见的错误是把简单的 Javadoc 写成 `/** @return the customer ID */`，这是不正确的。它应该写成 `/** Returns the customer ID. */`。

### 7.3 在哪里使用 Javadoc

至少在每个 `public` 类及它的每个 `public` 和 `protected` 成员处使用 Javadoc，下面会提到一些例外情况：

额外的 Javadoc 内容也可能存在，如第 7.3.4 节[非必需的Javadoc](#)。

#### 7.3.1 例外：不言自明的方法

对于简单明显的方法如 `getFoo`，Javadoc 是可选的。这种情况下除了写 “`Returns the foo`”，确实也没有什么值得写了。

> **重要提示**：如果有一些相关信息是需要读者了解的，那么以上的例外不应作为忽视这些信息的理由。例如，对于 `getCanonicalName` 方法，就不应该忽视文档说明，因为读者很可能不知道词语 “`canonical name`” 指的是什么。

#### 7.3.2 例外：重写

如果一个方法重写了超类中的方法，那么 Javadoc 并非必需的。

#### 7.3.3 可选的 Javadoc

对于包外不可见的类和方法，如有需要，也是要使用 Javadoc 的。如果一个注释是用来定义一个类，方法，字段的整体目的或行为，那么这个注释应该写成 Javadoc，这样更统一、更友好。

#### 7.3.4 不需要 Javadoc 的情况

其他类和成员种根据实际是否需要来写 Javadoc。

每当将注释用于定义类或成员的总体目的或行为时，该注释就会改为使用 Javadoc 来编写（使用`/**`）。

非必需的 Javadoc 并不要求严格遵循第 7.1.2、7.1.3 和 7.2 节的格式化规则，当然是推荐遵循的。
