# Google Java 编程风格指南

## 1 介绍

这份文档是 Google Java 编程风格规范的**完整**定义。当且仅当 Java 源文件符合此文档中的规则时，我们才认为它符合 Google 的 Java 编程风格。

与其它的编程风格指南一样，这些问题不仅涵盖编码格式方面的美学问题，而且还涵盖了其他方面的约定和编码标准。然而，本文档主要侧重于我们普遍遵循的**严格规则**，并避免给出无法明确执行的建议（无论是通过人工还是通过工具）。

### 1.1 术语说明

在本文档中，除非特殊说明：

1. 术语 `class` 表示“普通”类，枚举类，接口或注解（`@interface`）。
2. 术语 `member`（在类中）表示嵌套的类、属性、方法或者构造方法，也就是一个类中的所有顶级内容，初始值和注释除外。
3. 术语 `comment` 只用来表示实现的注释（`implementation comments`），我们不使用文档注释（`documentation comments`）一词，而是用`Javadoc`。

其他术语的说明，将偶尔在文档中的特定地方单独说明。

### 1.2 指南

本文档中的示例代码并**不作为规范**。也就是说，虽然示例代码是遵循 Google 编程风格的，但并不意味着这是表示这些代码的唯一方式。示例中的格式选择不应该被强制定为规则。

## 2 源文件基础

### 2.1 文件名

源文件名由最顶层的、大小写敏感的类名（只有[一个](https://checkstyle.sourceforge.io/styleguides/google-java-style-20180523/javaguide.html#s3.4.1-one-top-level-class)）来命名，文件扩展名为 `.java`。

### 2.2 文件编码：UTF-8

源文件使用 **UTF-8** 编码。

### 2.3 特殊字符

#### 2.3.1 空格字符

除了换行符外，**ASCII 水平空格字符（`0x20`）**是源码文件中唯一支持的空格字符。这意味着：

1. 所有其他空白字符将都会被转义。
2. Tab 制表符不用于缩进。

#### 2.3.2 特殊转义序列

对于具有[特殊转义序列](http://docs.oracle.com/javase/tutorial/java/data/characters.html)（例如：`\b`, `\t`, `\n`, `\f`, `\r`, `\"`, `\'`, `\\`等）的字符，都采用这种转义字符的方式表示，而不采用对应字符的八进制数（如：`\012`）或 Unicode （如：`\u000a`）来表示。

#### 2.3.3 非 ASCII 字符

对于其余的非 ASCII 字符，就直接使用 Unicode 字符即可（如：`∞`），或者使用等效的 Unicode 转义符（如：`\u221e`）。尽管强烈建议不要使用 Unicode 字符来表示字符串，但选择的核心理由是哪种代码更让人**易于阅读和理解**。

> **提示**：在使用 Unicode 转义符时，或者甚至是有时直接使用 Unicode 字符的时候，建议多添加一些解释性的注释说明。

示例：

| 示例  | 结论  |
| ------------ | ------------ |
| String unitAbbrev = "μs";	 | 最佳：即使没有注释也非常清晰。 |
| String unitAbbrev = "\u03bcs"; // "μs"  | 允许，但没有理由要这样做。  |
| String unitAbbrev = "\u03bcs"; // Greek letter mu, "s"  | 允许，但这样做显得笨拙还容易出错。  |
| String unitAbbrev = "\u03bcs";  | 很糟：读者根本看不出这是什么。  |
| return '\ufeff' + content; // byte order mark  | 很好：对于非打印字符，使用转义，并在必要时写上注释。  |

> **提示**：不要因为担心某些程序可能无法正确处理非 ASCII 字符而降低了代码的可读性。如果真的发生这种情况，程序就会**阻断**，必须对其进行**修复**。

## 3 源文件结构

源文件按**从上到下的顺序**，由以下几部分组成：

1. 许可证 (`License`）或版权信息（`copyright`）（如果有）
2. 包（`package`）语句
3. 包导入（`import`）语句
4. 低级类（`class`）类声明

> **注意**：以上每个部分之间应该只有**一个空行**作为分隔。

### 3.1 许可证或版权信息（如果有）

如果一个文件中包含许可证或版权信息，那么它应该在文件最前面。

### 3.2 package 语句

包（`package`）语句不换行，列限制（4.4 节，[列限制](#)）不适用于 `package` 语句。(即 `package` 语句独立成行)

### 3.3 import 语句

#### 3.3.1 import 不使用通配符

包导入（`import`）语句中**都不使用通配符**），不管是否是静态导入（`static import`）。

#### 3.3.2 import 不换行

包导入（`import`）语句不换行，列限制（4.4 节，[列限制](#)）不适用于 `import` 语句。(即每个 `import` 语句独立成行)

#### 3.3.3 顺序和间距

包导入（`import`）语句顺序如下：

1. 所有静态导入（`static import`）都在一个块中。
2. 所有非静态导入都在一个块中。

如果同时具有静态和非静态导入，则使用**一个空行**将两块分开。每个块的若干导入语句之间不空行。

在每个块中，导入的名称均按 ASCII 顺序排序。（**注意**：这与按 ASCII 排序的 import 语句不同，因为 `.` 在 `;` 之前排序。）

#### 3.3.4 静态内部类不用静态导入

静态内部类中不用静态导入（`static import`）。它们使用普通方式的导入（`import`）语句。

### 3.4 类声明

#### 3.4.1 仅有一个顶级类声明

每个源文件中只能有一个顶级类（`class`）声明。

#### 3.4.2 类成员顺序

类成员的顺序对代码的易读性有很大的影响，但是没有一个统一正确的标准。不同的类可能有不同的排序方式。

最重要的一点，**类中的所有成员都应该以某种逻辑去排序，维护者要能够解释这种排序逻辑**。比如，新的方法不能总是习惯性地添加到类的结尾，因为这样就是按创建的时间顺序而非某种逻辑来排序了。

##### 3.4.2.1 重载：永不分离

当一个类中有多个构造函数或者多个同名方法时，这些方法应该按顺序排列在一起，中间不要插入其它方法（甚至是私有成员）。

## 4 格式

**术语说明**：块状结构（`block-­like construct`）指的是以类、方法或构造函数为主体。需要注意的是，根据第 4.8.3.1 节的[数组初始化](#)中的初始值可被选择性地视为块状结构。

### 4.1 大括号

#### 4.1.1 使用大括号（即使是可选的）

大括号一般用在 `if`, `else`, `for`, `do` 和 `while` 等语句，即使只有一条语句（或者内容是空）的时候，也应该把大括号写上。

#### 4.1.2 非空语句块采用 K&R 风格

对于非空语句块，大括号遵循 Kernighan 和 Ritchie 风格（["Egyptian brackets"](http://www.codinghorror.com/blog/2012/07/new-programming-jargon.html)）：

- 左大括号前不换行
- 左大括号后换行
- 右大括号前换行
- 如果右大括号结束是一个语句块或者方法体、构造函数体或者有命名的类体，则需要换行。当右括号后面接 `else` 或逗号（`,`）时，不换行。

示例：

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

一些例外的情况，将在 4.8.1 节[枚举类](#)中介绍。

#### 4.1.3 空语句块：使代码更简洁

一个空的语句块或者空的构造方法，可以采用 K&R 风格（如 [4.1.2 节](#4.1.2)中所述）。也可以直接闭合（`{}`），中间不需要换行、空格或其他字符。但是，它与其他几个语句块联合组成语句块时，则需要换行。（例如：`if/else` 或者 `try/catch/finally`）。

示例：

```java
// 这是可接受的
void doNothing() {}

// 这也同样是可接受的
void doNothingElse() {
}
```

```java
// 这是不可接受的：多块语句中没有简洁的空语句块.
try {
  doSomething();
} catch (Exception e) {}
```

### 4.2块缩进：2个空格

每当一个新的语句块产生，缩进就增加两个空格。当这个语句块结束时，缩进恢复到上一层级的缩进级别。此缩进要求对整个语句块中的代码和注释都适用。（请参见 4.1.2 节，[非空语句块采用 K&R 风格](#)）

> **注意**：根据实际的编程经验，`2`个空格缩进的代码在当前大屏的计算机上会显得十分拥挤，反而使得代码`臃肿`不够美观。所以，我这里建议使用`4`个空格来缩进，会使得更加美观，而且能侧面督促开发人员减少代码的嵌套层数。

### 4.3 一条语句占一行

每条语句结束都需要换行。

### 4.4 列长度限制：100

Java 代码的列长度限制为 `100` 个字符。“字符”表示任何 Unicode 代码点。除非另有说明，否则超出此限制的任何行都必须进行换行，在 4.5 节会有“[换行](#)”的介绍。

> 每个 Unicode 代码点都算作一个字符，即使其显示宽度更大或更小。例如，如果使用[全角字符](https://en.wikipedia.org/wiki/Halfwidth_and_fullwidth_forms)，则可以选择在此规则严格要求的位置之前换行。

例外情况：

1. 不可能满足列长度限制的行(例如，Javadoc 中的一个长 URL，或是一个长的 JSNI 方法引用)。
2. `package` 和 `import` 语句(见 3.2 节和 3.3 节)。
3. 注释中的命令行，它们可能用于剪切粘贴到 shell 中。

### 4.5 换行

**术语说明**：将原本合法占用一行的代码分成多行，则该操作称之为“换行”（`line­-wrapping`）。

我们并没有全面，确定性的准则来决定在每一种情况下如何断行。通常，可以有几种有效的方式对同一段代码进行换行。

> **注**: 虽然换行的典型原因是避免超出列限制，但实际上，满足列限制的代码也可以由作者自行决定是否换行。

> **提示**：提取方法或局部变量也许可以解决问题，而不需要进行换行。

#### 4.5.1 在哪里换行

换行的主要原则是：**选择在更高级的语法逻辑处换行**。包括：

1. 当在一个非赋值运算的语句换行时，在运算符号之前换行。（这与 Google 的 C++ 规范和 JavaScrip 规范等其他规范不同）。
- 下面`类似运算符`的情况也适用：
  - 点（`.`）操作符
  - 方法引用中双冒号（`::`）操作符
  - 泛型符号（`<T extends Foo & Bar>`）
  - `catch` 块中的管道符号（`catch (FooException | BarException e)`）
2. 当要在赋值运算符语句处换行时，一般在赋值符号之后换行。不过，不管选哪一种方式都是可以接受的。
  - 这也适用于增强 for 循环（`foreach`）语句中的冒号（`:`）。
3. 方法名或构造函数名与仍然左括号（`(`）留在同一行。
4. 逗号（`,`）与其前面的内容留在同一行。
5. 在 `Lambda` 表达式中的箭头旁边永远不要换行，除非当 `Lambda` 表达式的主体由单个无括号的表达式组成时，可能会在箭头之后换行。示例：

```java
MyLambda<String, Long, Object> lambda =
    (String label, Long value, Object obj) -> {
        ...
    };

Predicate<String> predicate = str ->
    longExpressionInvolving(str);
```

> **注**：换行的主要目标是使代码更清晰易读，而不是书写最少行数的代码。

#### 4.5.2 换行缩进：至少 +4 个空格

自动换行时，第一行后的每一行都至少比第一行多缩进 `4` 个空格。

当存在连续自动换行时，缩进可能会多缩进不只 `4` 个空格(语法元素存在多级时)。一般而言，两个连续行要使用相同的缩进，当且仅当它们使用了同级别的语法元素。

第 4.6.3 节[水平对齐](#)中指出，不鼓励使用可变数目的空格来对齐前面行的符号。

### 4.6 空白

#### 4.6.1 垂直空白

以下情况总要使用一个空行分割：

1. 类成员之间需要单个空行隔开：例如：字段，构造函数，方法，内部类，静态初始化块，实例初始化块。
  - **例外情况**：两个连续属性之间的空行是可选的，根据需要使用空行来创建字段间的逻辑分组。
  - **例外情况**：第 [4.8.1 节](#)介绍了枚举常量之间的空白行。
2. 根据本文档其他章节的要求（如第 3 节，[源文件结构](#)和第3.3节，[import 语句](#)）。

一个空行也可以出现在任何提高可读性的地方，例如在语句之间将代码组织成逻辑子部分。在类的第一个成员之前或最后一个成员之后，使用空行(可选)。

不需要使用多个连续的空行（不鼓励）。

#### 4.6.2 水平空白

除了语法、其他规则、词语分隔、注释和 Javadoc 外，水平的 ASCII 空格只在以下情况出现：

1. 所有保留的关键字与紧接它之后同一行的左小括号（`(`）之间需要用空格隔开，例如：`if`, `for` `catch`。
2. 所有保留的关键字与在它之前的右大括号（`}`）之间需要空格隔开，例如：`else`、`catch`。
3. 在左大括号之前都需要空格隔开。
  - **例外情况**：`@SomeAnnotation({a, b})`(不使用空格）。
  - **例外情况**：`String[][] x = {{"foo"}};`（第二个 `{` 之前不使用空格）。
4. 所有的二元和三元运算符的两边，都需要使用空格隔开。以下“类似操作符”的情况也适用：
  - 连接泛型类型的 `&` 符号：`<T extends Foo & Bar>`
  - 处理多个异常 `catch` 中的管道操作符：`catch (FooException | BarException e)`
  - 增强 `for` 循环（`foreach`）中的冒号：`:`
  - `Lambda` 表达式中的箭头符号（`->`）
  例外情况：
  - 方法引用中的双冒号（`::`），写法类似于 `Object::toString`
  - 点号操作符（`.`），写法类似于 `object.toString()`
5. 逗号(`,`)、冒号(`:`)、分号(`;`)和右小括号(`)`)、Lambda箭头符号(`->`)之后，需要空格隔开。
6. 使用`//`双斜线开始一行注释时，双斜线两边都应该用空格隔开。并且可使用多个空格。（可选的，例如：`a = 0; // 赋值为0`）
7. 在声明的变量类型和变量名之间需要用空格隔开：`List<String> list`
8. 初始化一个数组时，大括号内的空格是可选的。
  -  `new int[] {5, 6}` 和 `new int[] { 5, 6 }` 这两种都是可以的
9. 在类型注解和 `[]` 或者 `...` 之后使用空格。

> **注意**：这些规则并不要求或禁止一行的开关或结尾需要额外的空格，只对内部空格做要求。

### 4.6.3 水平对齐：不需要

> **术语说明**：水平对齐，是指通过添加多个空格，使本行的某一符号与上一行的某一符号上下对齐。

这种对齐是被允许的，但是不会做强制要求。
这种对齐是允许的，但**从来不是**谷歌风格所要求的。甚至不需要在已经使用过的地方保持水平对齐。

下面是一个没有对齐的示例，然后使用对齐：
以下是没有水平对齐和做了水平对齐的例子：

```java
private int x; // 这种挺好
private Color color; // 同上

private int   x;     // 允许，但是未来会继续编辑
private Color color; // 可能会使它对不齐
```

> **提示**：水平对齐可以提高代码的可读性，但是增加了未来维护代码的麻烦。考虑到维护时只需要改变一行代码，之前的对齐可以不需要改动。为了对齐，你更有可能改了一行代码，同时需要更改附近的好几行代码，而这几行代码的改动，可能又会引起一些为了保持对齐的代码改动。那原本这行改动，我们称之为**爆炸半径**。这种改动，在最坏的情况下可能会导致大量的无意义的工作，即使在最好的情况下，也会影响版本历史信息，减慢代码 `review` 的速度，引起更多合并代码的冲突。

### 4.7 分组小括号：推荐使用

除非作者和审阅人（`reviewer`）都认为去掉小括号也不会使代码被误解，或是去掉小括号能让代码更易于阅读，否则我们不应该去掉小括号。假设读者能记住整个 Java 运算符优先级表，是不合理的。

### 4.8 特殊结构

#### 4.8.1 枚举类

枚举常量间用逗号隔开，换行是可选的。而且还允许附加的空行（通常只有一个）。以下是一些可行的示例：

```java
private enum Answer {
  YES {
    @Override public String toString() {
      return "yes";
    }
  },

  NO,
  MAYBE
}
```

没有方法和 Javadoc 的枚举类可写成数组初始化的格式：

```java
private enum Suit { CLUBS, HEARTS, SPADES, DIAMONDS }
```

由于枚举类也是一个类，因此所有适用于其它类的格式规则也适用于枚举类。

#### 4.8.2 变量声明

##### 4.8.2.1 每次声明一个变量

每个变量声明（字段或局部变量）只声明一个变量：不使用诸如 `int a, b;` 之类的声明。

**例外情况**: `for` 循环的初始化头中可以使用多个变量声明。

##### 4.8.2.2 需要时才声明

不要在代码块的开头把所有局部变量一次性全部进行声明，而是在第一次需要使用它时才声明，这样能最大程度地减小其作用范围。局部变量在声明时最好就进行初始化，或者声明后尽快进行初始化。

#### 4.8.3 数组

##### 4.8.3.1 数组初始化：可以是块状结构

数组初始化可以写成块状结构，例如以下格式的写法都是允许的（）：

```java
new int[] {           new int[] {
  0, 1, 2, 3            0,
}                       1,
                        2,
new int[] {             3,
  0, 1,               }
  2, 3
}                     new int[]
                          {0, 1, 2, 3}
```

##### 4.8.3.2 非 C 风格的数组声明

中括号是类型的一部分：`String[] args`， 而非 `String args[]`。

#### 4.8.4 switch 语句

**术语说明**：switch 块的大括号内是一个或多个语句组。每个语句组包含一个或多个 `switch` 标签（`case FOO:` 或 `default:`），后面跟着一条或多条语句（对于最后一个语句组，则为零个或多个语句）。

##### 4.8.4.1 缩进

和其他语句块一样，switch 大括号之后缩进两个字符。

每个 switch 标签之后，有一个换行符，并按照大括号相同的处理方式缩进两个字符。在标签结束后，恢复到之前的缩进，类似大括号的结束符。

##### 4.8.4.2 直落（fall through）：注释

在 switch 块内，每个语句组要么通过 `break`、`continue`、`return` 或 `throw 异常`来终止，要么通过一条注释来说明程序将继续执行到下一个语句组，任何能表达这个意思的注释都是可以的（典型的是用 `// fall through`）。在 switch 块的最后一个语句组中不需要此特殊的注释。例如：

```java
switch (input) {
  case 1:
  case 2:
    prepareOneOrTwo();
    // fall through
  case 3:
    handleOneTwoOrThree();
    break;
  default:
    handleLargeNumber(input);
}
```

> **注意**：在 `case 1` 之后仅在语句组的末尾不需要该注释。

每个 switch 语句中，都需要显式声明 `default` 语句，即使没有任何代码。

> **注意**：枚举类型的 switch 语句可以省略`default`语句组，如果它包含覆盖该类型的所有可能值的显式情况。这使得IDE或其他静态分析工具能够在丢失任何情况时发出警告。

**例外**：如果枚举类型的 switch 语句包含包含该类型所有可能值的情况，则可以省略 `default` 语句组。这使得 IDE 或其他静态分析工具能够在丢失任何情况时发出警告。

#### 4.8.5 注解

注解应用到类、方法或者构造方法上时，应紧接 Javadoc 之后。每行只能有一个注解。注解所在行不受列长度限制（4.5 节，[换行](#)），也不需要增加缩进。例如：

```java
@Override
@Nullable
public String getNameIfPresent() { ... }
```

**例外**：如果注解只有一个，并且不带参数。则它可以和类或方法名放在同一行。例如：

```java
@Override public int hashCode() { ... }
```

当注解应用到成员变量时，也是紧接 Javadoc 之后。不同的是，多个注解可以放在同一行。例如：

```java
@Partial @Mock DataLoader loader;
```

对于参数或者局部变量使用注解的情况，没有特定的规范。

#### 4.8.6 注释

本节介绍注释的使用。Javadoc 在第 7 节 [Javadoc](#) 中单独介绍。

任何换行符之前都可以带有任意空格后再跟注释。这样的注释使该行成为非空白行。

##### 4.8.6.1 块注释风格

注释的缩进与它所注释的代码缩进相同。可以采用 `/* ... */` 风格，也可以用 `// ...` 风格。当使用 `/* ... */` 进行多行注释时，每一行都应该以 `*` 开始，并且 `*` 应该上下对齐。

```java
/*
 * This is          // And so           /* Or you can
 * okay.            // is this.          * even do this. */
 */
```

注释不包含在用星号（`*`）或其他字符绘制的框中。

> **提示**：在编写多行注释时，如果希望自动代码格式化的程序在必要时重新进行换行，请使用 `/* ... */` 的风格（段落样式）。大多数格式化程序不会在 `// ...` 风格的注释块中重新换行。

#### 4.8.7 修饰符

类和成员变量的修饰符，按 `Java Lauguage Specification` 中介绍的先后顺序排序。具体是：

```java
public protected private abstract default static final transient volatile synchronized native strictfp
```

#### 4.8.8 数字字面量

长整型（`long`）的数字字面量使用大写的 `L` 作为后缀，不得使用小写（避免与数字 `1` 混淆）。例如：使用 `3000000000L`，而不是 `3000000000l`。

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
