DataTag 语法与使用教程

本教程将带你从零开始学习 DataTag —— 一个 Java 编写的类似 JSON 的数据格式与工具库。
教程按照 从简单到复杂 的顺序编排，每一章都配有语法说明和示例，适合所有水平的开发者。

---

📑 目录

基础篇

1. 初识 DataTag
2. 基本数据类型
   · 2.1 整数类型
   · 2.2 浮点数类型
   · 2.3 布尔类型
   · 2.4 字符与字符串
   · 2.5 空值
3. 容器数据类型
   · 3.1 复合结构 (Composite)
   · 3.2 列表 (List)

进阶篇

1. 引用数据类型
   · 4.1 标识符
   · 4.2 日期
   · 4.3 颜色
   · 4.4 文件与网址
   · 4.5 资源与类路径
   · 4.6 枚举
   · 4.7 Java 序列化

高级篇

1. 高级语法
   · 5.1 方法调用
   · 5.2 类型转换
   · 5.3 引用 (Ref)

实战篇

1. Java 中使用 DataTag
   · 6.1 创建与操作 Composite
   · 6.2 创建与操作 List
   · 6.3 解析文本与二进制
   · 6.4 自定义方法扩展
   · 6.5 常用 API 速查

---

1. 初识 DataTag

DataTag 是一种类似 JSON 的数据格式，但更加强大：

· 支持 更多数据类型（如颜色、日期、文件路径等）
· 支持 引用、类型转换、方法调用 等高级语法
· 提供 Java 库方便读写和操作
· 所有数据都可以为 null（使用 null 或 none 关键字）
· 支持二进制序列化，读写效率高
· 内置常量池机制，重复数据自动优化

最小示例：

```
{
    Name: "张三",
    Age: 25,
    Address: null
}
```

注释语法（只有多行注释）：

```
/* 这是注释
   可以跨多行 */
{
    Name: "李四"  /* 行内注释 */
}
```

---

2. 基本数据类型

基本数据类型是最基础的构成单元，所有数据都可以用 null 或 none 表示空值。

2.1 整数类型

支持多种进制和大小后缀。

类型 后缀（不区分大小写） 示例 说明
字节 (Byte) b / B 100b, -0x1Fb, 0b1010B 范围：-128 ~ 127
短整数 (Short) s / S 2000s, -0o377S 范围：-32768 ~ 32767
整数 (Integer) 无 12345, -0xABCD 范围：-2^31 ~ 2^31-1
长整数 (Long) l / L 987654321L, -0b1111L 范围：-2^63 ~ 2^63-1
大整数 (BigInteger) i / I 999999999i, -0xFFFFI 任意精度整数

进制前缀：

· 十六进制：0x 或 0X（如 0xFF）
· 八进制：0o 或 0O（如 0o377）
· 二进制：0b 或 0B（如 0b1010）

示例：

```
{
    ByteValue: 127b,
    ShortValue: -32768S,
    IntValue: 100000,
    HexInt: 0xABCDEF,
    BinaryInt: 0b101010,
    LongValue: 9007199254740991L,
    BigIntValue: 12345678901234567890i,
    NullInt: null
}
```

2.2 浮点数类型

支持小数、科学计数法和百分比，以及十六进制浮点。

类型 后缀（不区分大小写） 示例 说明
单精度浮点 (Float) f / F 3.14f, -0x1.8p1F, 50%f 32位 IEEE 754
双精度浮点 (Double) 可选 d/D 或 % 3.14159, -2.5e-3, 75%, 0x1.2p3 64位 IEEE 754
大浮点数 (BigDecimal) d / D 或 % 123.456d, 99.99%D 任意精度小数

百分比说明：添加 % 后缀表示除以100，如 50% 等同于 0.5

示例：

```
{
    Pi: 3.1415926,
    Scientific: 1.23e-4,
    Percent: 50%,           /* 0.5 */
    HexFloat: 0x1.8p1,      /* 3.0 */
    FloatValue: 3.14f,
    DoubleValue: 2.71828d,
    BigDecimalValue: 123.456789d,
    NullDouble: none
}
```

2.3 布尔类型

仅有两个值：true 和 false。

```
{
    Flag1: true,
    Flag2: false,
    Flag3: null
}
```

2.4 字符与字符串

字符 (Character)：

· 单引号括起，只能一个字符
· 支持转义：\b \f \n \r \t \' \" \\
· 不支持 Unicode 转义（如 \uFFFF）

示例：

```
{
    CharA: 'A',
    CharNewLine: '\n',
    CharQuote: '\'',
    CharBackslash: '\\'
}
```

字符串 (String)：

· 双引号括起
· 支持相同转义字符
· 不支持 Unicode 转义

示例：

```
{
    String: "Hello, World!",
    Escaped: "Line1\nLine2\tTabbed",
    Quote: "She said, \"Hello!\"",
    NullString: null
}
```

多行字符串 (MoreLinesString)：

· 三个双引号 """ 括起
· 可包含换行
· 同样支持转义

示例：

```
{
    MultiLine: """
        line 1
        line 2
        "quoted text"
    """
}
```

单词字符串 (WordString)：

· 无需引号
· 规则：以小写字母、数字、下划线、美元符号开头，后可跟字母数字下划线美元符号
· 或以 # 开头后跟字母数字下划线美元符号

示例：

```
{
    Word: hello_world,
    WordWithDigit: var123,
    SharpWord: #tagName,
    DollarWord: $value
}
```

2.5 空值

使用 null 或 none 表示任何数据类型的空值。

```
{
    Empty: null,
    AlsoEmpty: none,
    Person: {
        Name: "王五",
        Age: null,      /* 年龄未知 */
        Address: none   /* 地址不存在 */
    }
}
```

---

3. 容器数据类型

容器可以嵌套任意数据类型，包括其他容器。

3.1 复合结构 (Composite)

类似 JSON 对象，键值对集合。

键的规则：

· 必须由大写字母开头
· 后可跟字母（大小写）、数字、下划线、美元符号
· 如果键只有一个字符，那必须是单个大写字母
· 正则：[A-Z][a-zA-Z0-9_$]+ 或 [A-Z]

语法：

```
{
    键1: 值1,
    键2: 值2,
    ...
}
```

示例：

```
{
    Name: "李四",
    Age: 30,
    Address: {
        City: "北京",
        Zip: 100000,
        Street: null
    },
    Scores: [95, 87, 92],
    NullField: null
}
```

底层实现：LinkedHashMap<Key, Value>，保持插入顺序。

3.2 列表 (List)

有序集合，元素可以是任意类型。

语法：

```
[ 值1, 值2, 值3, ... ]
```

示例：

```
[
    1,
    2.5,
    "three",
    [4, 5, 6],
    {
        Key: "value",
        NestedList: [7, 8, 9]
    },
    null
]
```

底层实现：ArrayList<Element>。

---

4. 引用数据类型

这些类型提供了对现实概念的映射，方便直接使用。

4.1 标识符 (Identifier)

格式：命名空间/名字，用于表示某个唯一标识。

语法：[a-zA-Z0-9_$]+/[a-zA-Z0-9_$]+

示例：

```
{
    UserId: system/xfherun,
    ResourceId: com.example/MyResource,
    NullId: null
}
```

4.2 日期 (Date)

支持三种格式：

格式 语法 示例
完整日期时间 yyyy[/MM[/dd]] HH[:mm[:ss[.SSS]]] 2025/03/15 14:30:00.123
仅年份 y数字 y2025
仅小时 h数字 h17（下午5点）

示例：

```
{
    FullDate: 2025/03/15 14:30:00.123,
    SimpleDate: 2025/3/15 14:30,
    YearOnly: y2025,
    HourOnly: h17,
    NullDate: null
}
```

4.3 颜色 (Color)

支持五种颜色表示法：

类型 语法 示例
RGB rgb(r, g, b) rgb(255, 0, 0)
RGBA rgba(r, g, b, a) rgba(0, 255, 0, 128)
Hex6 color(6位十六进制) color(FF00FF)
Hex8 color(8位十六进制) color(80FF00FF)
颜色名 color(颜色名) color(red)

示例：

```
{
    Red: rgb(255, 0, 0),
    GreenWithAlpha: rgba(0, 255, 0, 128),
    Purple: color(FF00FF),
    SemiTransparent: color(80FF00FF),
    Blue: color(blue),
    NullColor: null
}
```

4.4 文件与网址

文件 (File)：

· 语法：file 路径;
· 路径中的 \ 和 ; 需要转义：\\ 表示 \，\; 表示 ;

网址 (Url)：

· 语法：url 地址;
· 同样需要转义 \ 和 ;

示例：

```
{
    ConfigFile: file C:\\config.conf;,
    LogFile: file /var/log/app.log;,
    Website: url https://example.com;,
    ApiUrl: url https://api.example.com/v1;,
    NullFile: null
}
```

4.5 资源与类路径

Java类路径 (JavaClassPath)：

· 语法：包名.类名.class
· 规则：[a-zA-Z0-9_$]+(.[a-zA-Z0-9_$]+)*.class

资源 (Resource)：

· 语法：JavaClassPath >>> 资源路径;
· 前面的类用于加载资源（通过 getResourceAsStream）
· 如果类不存在，使用内部默认类加载
· 资源路径中的 \ 和 ; 需要转义

示例：

```
{
    ClassPath: com.example.Test.class,
    Resource: com.example.Test.class >>> /config.properties;,
    ResourceWithEscape: com.example.Test.class >>> path\\ with\\; semicolon;,
    NullResource: null
}
```

4.6 枚举 (Enum)

格式：枚举类别.枚举名

· 枚举类别：符合键规则（大写字母开头）
· 枚举名：全大写，可包含数字、下划线、美元符号

示例：

```
{
    Status: StatusCode.SUCCESS,
    Error: ErrorType.NOT_FOUND,
    Level: LogLevel.INFO,
    NullEnum: null
}
```

4.7 Java 序列化 (Serialization)

以十六进制形式存储序列化数据，一般由工具生成，不手动编写。

语法：serial = 十六进制数据;

示例：

```
{
    SerialData: serial = CAFEBABE0000002E0012...;
}
```

---

5. 高级语法

5.1 方法调用

调用 Java 中注册的方法，支持带命名空间的方法名。

语法：

· 方法命名空间：WordString (:: WordString)*
· 方法名：WordString
· 调用：命名空间::方法名(参数1, 参数2, ...)

示例：

```
{
    /* 简单调用 */
    Sum: math::add(10, 20),
    
    /* 带命名空间的调用 */
    Result: com.example.utils::StringUtils::format("Hello %s", "World"),
    
    /* 传递复杂参数 */
    Data: test::process([1, 2, 3], {Name: "test"}, true),
    
    /* 嵌套调用 */
    Nested: math::add( math::multiply(2, 3), math::add(4, 5) )
}
```

5.2 类型转换

将一种类型显式转换为另一种类型，格式：<目标类型> 数据。

所有可用的类型标识符：

类型标识符 说明 目标Java类型
<i8> 字节 ByteType
<i16> 短整数 ShortType
<i32> 整数 IntegerType
<i64> 长整数 LongType
<f32> 单精度浮点数 FloatType
<f64> 双精度浮点数 DoubleType
<i> 大整数 BigIntegerType
<f> 大浮点数 BigDecimalType
<bool> 布尔 BooleanType
<char> 字符 CharacterType
<str> 字符串 StringType
<identifier> 标识符 IdentifierType
<date> 日期 DateType
<color> 颜色 ColorType
<file> 文件 FileType
<url> 网址 UrlType
<resource> 资源 ResourceType
<classpath> Java类路径 JavaClassPathType
<enum> 枚举 EnumType
<serial> 序列化 SerializationType
<nullable> 空类型 强制转为null
<auto> 自动提升 将普通Java对象提升为对应的DataTag类型
<reference> 引用类型 ReferenceType
<basic> 基础类型基类 BasicType
<container> 容器类型基类 ContainerType
<number> 数字类型基类 NumberType
<composite> 复合类型 Composite
<list> 列表类型 List

转换规则：

· 基本类型转换调用对应的 parseXxx 方法
· 容器类型转换会尝试解析字符串内容
· <nullable> 强制将任何值转为 null
· <auto> 将普通Java对象（如字符串、数字等）提升为对应的DataTag包装类型，不做类型转换，仅做包装

示例：

```
{
    /* 字符串转数字 */
    FromString: <i32> "123",
    FromHex: <i32> "0xFF",
    
    /* 数字转字符串 */
    FromInt: <str> 456,
    
    /* 浮点转换 */
    ToFloat: <f32> "3.14",
    ToDouble: <f64> 100,
    
    /* 强制转为null */
    ToNull: <nullable> "anything",  // 结果为 null
    
    /* 自动提升为DataTag类型 */
    Auto1: <auto> "123",      // 提升为 DataTag，而不是转为整数
    Auto2: <auto> 3.14,       // 提升为 DataTag
    Auto3: <auto> true,       // 提升为 DataTag
    
    /* 字符串转复合 */
    ToComposite: <composite> "{Name:\"张三\", Age:20}",
    
    /* 类型转换后的引用 */
    CastThenRef: <composite> "{Data: [1,2,3]}",
    RefElement: ref @this::CastThenRef::Data->0  // 得到 1
}
```

<auto> 的详细说明：
<auto> 不会改变数据的语义，只是将Java层面的原始值（如字符串"123"）包装成对应的DataTag类型。它不会尝试解析或转换内容，仅仅是为数据添加类型包装，使其能够在DataTag系统中使用。

5.3 引用 (Ref)

可以在数据内部引用其他部分的值，实现复用和数据关联。

基本语法：ref @this::路径

路径规则：

· 访问复合字段：字段名
· 访问嵌套字段：父字段->子字段
· 访问列表元素：列表字段->索引
· 访问方法结果：方法字段::内部字段
· 必须从根（@this）开始，不支持相对路径
· 不能引用向下的未定义属性

5.3.1 基础引用

```
{
    Name: "张三",
    Age: 23,
    
    /* 引用当前复合中的字段 */
    SameName: ref @this::Name,
    SameAge: ref @this::Age
}
```

结果：

```
{
    Name: "张三",
    Age: 23,
    SameName: "张三",
    SameAge: 23
}
```

5.3.2 嵌套引用

```
{
    Nesting: {
        FieldString: "FieldString",
        Nesting2: {
            FieldString2: "FieldString2"
        }
    },
    Ref1: ref @this::Nesting->FieldString,
    Ref2: ref @this::Nesting->Nesting2->FieldString2
}
```

结果：

```
Ref1: "FieldString"
Ref2: "FieldString2"
```

5.3.3 多层嵌套

```
{
    Nesting: {
        FieldString: "Level1",
        Nesting2: {
            FieldString2: "Level2",
            Nesting3: {
                FieldString: "Level3",
                Nesting4: {
                    FieldString2: "Level4"
                }
            }
        }
    },
    Ref1: ref @this::Nesting->FieldString,
    Ref2: ref @this::Nesting->Nesting2->FieldString2,
    Ref3: ref @this::Nesting->Nesting2->Nesting3->FieldString,
    Ref4: ref @this::Nesting->Nesting2->Nesting3->Nesting4->FieldString2
}
```

5.3.4 引用列表元素

```
{
    List: [10, 20, 30, 40, 50],
    Ref0: ref @this::List->0,
    Ref1: ref @this::List->1,
    RefLast: ref @this::List->4
}
```

结果：

```
Ref0: 10
Ref1: 20
RefLast: 50
```

5.3.5 引用方法调用结果

假设方法 test::returnComposite() 返回：

```
{
    Name: "xfherun",
    Age: 19
}
```

```
{
    InvokeMethod: test::returnComposite(),
    Name: ref @this::InvokeMethod::Name,
    Age: ref @this::InvokeMethod::Age
}
```

结果：

```
Name: "xfherun"
Age: 19
```

5.3.6 方法返回嵌套结构

假设方法返回：

```
{
    Object: {
        Name: "xfherun1",
        Age: 24
    }
}
```

```
{
    InvokeMethod: test::returnComposite(),
    Name: ref @this::InvokeMethod::Object->Name,
    Age: ref @this::InvokeMethod::Object->Age
}
```

5.3.7 方法返回列表

假设方法返回：[a, b, c, d, e]

```
{
    InvokeMethod: test::returnComposite(),
    First: ref @this::InvokeMethod->0,
    Second: ref @this::InvokeMethod->1
}
```

5.3.8 复杂嵌套结构

假设方法返回：

```
{
    Name: "李四",
    Object: {
        Address: "Internal",
        Array: [
            "My name's xfherun",
            23,
            {
                Name: "xfherun",
                Age: 19
            }
        ]
    }
}
```

```
{
    InvokeMethod: test::returnComposite(),
    RefName: ref @this::InvokeMethod->Name,
    RefAddr: ref @this::InvokeMethod->Object->Address,
    RefStr: ref @this::InvokeMethod->Object->Array->0,
    RefNum: ref @this::InvokeMethod->Object->Array->1,
    RefObjName: ref @this::InvokeMethod->Object->Array->2->Name,
    RefObjAge: ref @this::InvokeMethod->Object->Array->2->Age
}
```

5.3.9 引用方法参数

```
{
    Accept: test::process(
        [1, r, 5],
        "字符串",
        true,
        {Name: "zhangsan"}
    ),
    RefList: ref @this::Accept->0,
    RefString: ref @this::Accept->1,
    RefBool: ref @this::Accept->2,
    RefListElement: ref @this::Accept->0->0,
    RefName: ref @this::Accept->3->Name
}
```

5.3.10 引用类型转换后的数据

```
{
    Test: "{Name:\"Hello\"}",
    CastTo: <composite> ref @this::Test,
    
    /* 获取原始数据（转换前） */
    Original: ref @this::CastTo->0,
    
    /* 获取转换后的字段 */
    Name: ref @this::CastTo::Name
}
```

结果：

```
Original: "{Name:\"Hello\"}"
Name: "Hello"
```

重要：

· 如果转换后的类型不是容器类型，则不能通过 :: 访问内部字段
· ->0 用于获取转换前的原始数据
· ::Name 用于访问转换后复合中的字段

---

6. Java 中使用 DataTag

DataTag 提供了完整的 Java API 来创建、操作和解析数据。

6.1 创建与操作 Composite

```java
import com.xfherun.datatag.datatags.*;

// 创建复合对象
Composite composite = new Composite();

// 添加各种类型的数据
composite.put(Key.of("Integer"), new IntegerType(123));
composite.put(Key.of("String"), new StringType("Hello DataTag"));
composite.put(Key.of("Boolean"), new BooleanType(true));
composite.put(Key.of("Double"), new DoubleType(3.14159));
composite.put(Key.of("Null"), null);

// 添加复合嵌套
Composite nested = new Composite();
nested.put(Key.of("Name"), new StringType("张三"));
nested.put(Key.of("Age"), new IntegerType(25));
composite.put(Key.of("Person"), nested);

// 添加列表
List list = new List();
list.add(new IntegerType(1));
list.add(new IntegerType(2));
list.add(new IntegerType(3));
composite.put(Key.of("Numbers"), list);

// 获取数据
Integer intVal = composite.getInteger(Key.of("Integer"));
String strVal = composite.getString(Key.of("String"));
Composite person = composite.getComposite(Key.of("Person"));
String name = person.getString(Key.of("Name"));

// 转为代码文本
String codeText = composite.toDataTagString();      // 带缩进和换行
String codeLine = composite.toDataTagLineString();  // 单行紧凑格式

// 写入二进制文件
try (FileOutputStream fos = new FileOutputStream("data.dt")) {
    composite.write(fos);
    System.out.println("二进制数据已写入");
} catch (IOException e) {
    e.printStackTrace();
}
```

6.2 创建与操作 List

```java
import com.xfherun.datatag.datatags.*;

// 创建列表
List list = new List();

// 添加元素
list.add(new IntegerType(100));
list.add(new StringType("two"));
list.add(new BooleanType(false));
list.add(new DoubleType(2.71828));
list.add(null);

// 添加嵌套列表
List nestedList = new List();
nestedList.add(new StringType("a"));
nestedList.add(new StringType("b"));
nestedList.add(new StringType("c"));
list.add(nestedList);

// 添加复合
Composite composite = new Composite();
composite.put(Key.of("Key"), new StringType("value"));
list.add(composite);

// 获取元素
DataTag element = list.get(0);
IntegerType intElement = list.get(0, IntegerType.class);
String strElement = list.getString(1);  // 索引1

// 转为文本
String text = list.toDataTagString();
String line = list.toDataTagLineString();

// 写入二进制
try (FileOutputStream fos = new FileOutputStream("list.dt")) {
    list.write(fos);
}
```

6.3 解析文本与二进制

6.3.1 从文本解析

```java
// 解析字符串
String source = "{Name: \"李华\", Age: 20, Scores: [95, 87, 92]}";
Composite composite = Composite.parse(source);

// 获取数据
String name = composite.getString(Key.of("Name"));
int age = composite.getInteger(Key.of("Age")).intValue();
List scores = composite.getList(Key.of("Scores"));
int firstScore = ((IntegerType) scores.get(0)).intValue();

// 解析文件
Composite fromFile = Composite.parse(new File("config.dt"));

// 解析URL
Composite fromUrl = Composite.parse(new URL("https://example.com/data.dt"));

// 解析输入流
try (InputStream is = new FileInputStream("data.dt")) {
    Composite fromStream = Composite.parse(is);
}

// 带方法管理器解析（用于支持方法调用）
DataTagMethodManager manager = new DataTagMethodManager();
manager.addMethod(Namespace.of("math"), new DataTagClass(MathMethods.class));
Composite withMethods = Composite.parse(
    "{Result: math::add(5, 3)}",
    new DataTagMethodManager[]{manager}
);
```

6.3.2 从二进制读取

```java
// 读取二进制复合
try (FileInputStream fis = new FileInputStream("data.dt")) {
    Composite composite = Composite.read(fis);
}

// 读取二进制列表
try (FileInputStream fis = new FileInputStream("list.dt")) {
    List list = List.read(fis);
}
```

6.3.3 常量池与二进制分析

```java
Composite composite = Composite.parse("{Name: \"张三\", Age: 20, Name2: \"张三\"}");

// 获取常量池（重复的"张三"会被优化）
TreeMap<Integer, DataTag> constPool = composite.getConstPool();
System.out.println("常量池大小: " + constPool.size());

// 打印常量池内容
System.out.println(composite.printConstPool());

// 查看二进制数据（ASCII形式）
System.out.println(composite.viewBinaryData("ascii"));

// 查看十六进制形式
System.out.println(composite.viewBinaryData());
```

6.4 自定义方法扩展

你可以将自己的 Java 方法注册到 DataTag 中，供语法调用。

6.4.1 创建方法类

```java
import com.xfherun.datatag.datatags.*;
import com.xfherun.datatag.parser.methods.*;
import com.xfherun.datatag.parser.compiler.*;

/**
 * 数学方法示例
 */
public class MathMethods {
    
    /**
     * 加法
     */
    @DataTagMethodAnnotation(
        name = "add",
        parameters = {DataTag.class, DataTag.class},
        returnType = IntegerType.class
    )
    public static IntegerType add(DataTagCompiler context, DataTag a, DataTag b) {
        // 使用 intValue() 方法获取数值
        int x = 0, y = 0;
        
        if (a instanceof IntegerType) {
            x = ((IntegerType) a).intValue();
        } else if (a instanceof LongType) {
            x = (int) ((LongType) a).longValue();
        } else if (a instanceof DoubleType) {
            x = (int) ((DoubleType) a).doubleValue();
        }
        
        if (b instanceof IntegerType) {
            y = ((IntegerType) b).intValue();
        }
        
        return new IntegerType(x + y);
    }
    
    /**
     * 乘法（支持大整数）
     */
    @DataTagMethodAnnotation(
        name = "multiply",
        parameters = {DataTag.class, DataTag.class},
        returnType = BigIntegerType.class
    )
    public static BigIntegerType multiply(DataTagCompiler context, DataTag a, DataTag b) {
        // 使用 bigIntegerValue() 获取大整数
        java.math.BigInteger x = ((NumberBasicType<?>) a).bigIntegerValue();
        java.math.BigInteger y = ((NumberBasicType<?>) b).bigIntegerValue();
        
        return new BigIntegerType(x.multiply(y));
    }
    
    /**
     * 除法（支持高精度）
     */
    @DataTagMethodAnnotation(
        name = "divide",
        parameters = {DataTag.class, DataTag.class},
        returnType = BigDecimalType.class
    )
    public static BigDecimalType divide(DataTagCompiler context, DataTag a, DataTag b) {
        // 使用 bigDecimalValue() 获取高精度小数
        java.math.BigDecimal x = ((NumberBasicType<?>) a).bigDecimalValue();
        java.math.BigDecimal y = ((NumberBasicType<?>) b).bigDecimalValue();
        
        return new BigDecimalType(x.divide(y, java.math.RoundingMode.HALF_UP));
    }
}
```

6.4.2 字符串处理示例

```java
public class StringMethods {
    
    @DataTagMethodAnnotation(
        name = "concat",
        parameters = {DataTag.class, DataTag.class},
        returnType = StringType.class
    )
    public static StringType concat(DataTagCompiler context, DataTag a, DataTag b) {
        String s1 = a instanceof StringType ? ((StringType) a).getData() : a.toString();
        String s2 = b instanceof StringType ? ((StringType) b).getData() : b.toString();
        return new StringType(s1 + s2);
    }
    
    @DataTagMethodAnnotation(
        name = "substring",
        parameters = {DataTag.class, DataTag.class, DataTag.class},
        returnType = StringType.class
    )
    public static StringType substring(DataTagCompiler context, 
                                        DataTag str, 
                                        DataTag start, 
                                        DataTag end) {
        String s = ((StringType) str).getData();
        int begin = ((IntegerType) start).intValue();
        int finish = ((IntegerType) end).intValue();
        return new StringType(s.substring(begin, finish));
    }
}
```

6.4.3 注册并使用方法

```java
// 创建方法管理器
DataTagMethodManager manager = new DataTagMethodManager();

// 添加方法类（可以添加多个）
manager.addMethod(Namespace.of("math"), new DataTagClass(MathMethods.class));
manager.addMethod(Namespace.of("string"), new DataTagClass(StringMethods.class));

// 解析时传入管理器
Composite composite = Composite.parse(
    """
    {
        AddResult: math::add(15, 27),
        MultiplyResult: math::multiply(123456789, 987654321),
        DivideResult: math::divide(10, 3),
        ConcatResult: string::concat("Hello", " World"),
        Substring: string::substring("DataTag", 0, 4)
    }
    """,
    new DataTagMethodManager[]{manager}
);

// 获取结果
int addResult = composite.getInteger(Key.of("AddResult")).intValue();           // 42
java.math.BigInteger mulResult = composite.getBigInteger(Key.of("MultiplyResult"));
java.math.BigDecimal divResult = composite.getBigDecimal(Key.of("DivideResult"));
String concatResult = composite.getString(Key.of("ConcatResult")); // "Hello World"
```

6.4.4 方法定义注意事项

1. 方法修饰符：必须是 public static
2. 第一个参数：必须是 DataTagCompiler context
3. 后续参数：必须是 DataTag 的实现类
4. 返回值：必须是 DataTag 的实现类，不能是 void
5. 注解要求：
   · name：必须与方法名一致
   · parameters：第二个参数开始的类型字节码数组
   · returnType：返回值类型的字节码

6.5 常用 API 速查

6.5.1 NumberBasicType<N extends Number> 通用方法

所有数字类型（IntegerType、LongType、DoubleType、FloatType、BigIntegerType、BigDecimalType）都实现自接口 BasicType，类的设计：

```java
package com.xfherun.datatag.datatags;

import java.math.BigDecimal;
import java.math.BigInteger;
import java.util.Objects;

/**
 * 数字类型
 * project: DataTag
 *
 * @author xfherun 何润
 */
public abstract class NumberBasicType<N extends Number> implements BasicType
{
    /**
     * 数字
     */
    protected volatile String number;

    /**
     * 默认的空参构造 -> class: NumberBasicType
     *
     * @author xfherun 何润
     */
    public NumberBasicType()
    {
    }

    public NumberBasicType(N num)
    {
        set(num);
    }

    public NumberBasicType(String string)
    {
        set(string);
    }

    /**
     * 设置数字
     *
     * @param number 数字
     * @author xfherun 何润
     */
    public abstract void set(N number);

    /**
     * 设置数字
     *
     * @param number 字符串数字
     * @author xfherun 何润
     */
    public abstract void set(String number);

    /**
     * 获取字节
     *
     * @return 返回字节
     * @author xfherun 何润
     */
    public abstract byte byteValue();

    /**
     * 获取短整数
     *
     * @return 返回短整数
     * @author xfherun 何润
     */
    public abstract short shortValue();

    /**
     * 获取整数
     *
     * @return 返回整数
     * @author xfherun 何润
     */
    public abstract int intValue();

    /**
     * 获取长整数
     *
     * @return 返回长整数
     * @author xfherun 何润
     */
    public abstract long longValue();

    /**
     * 获取单精度浮点数
     *
     * @return 返回单精度浮点数
     * @author xfherun 何润
     */
    public abstract float floatValue();

    /**
     * 获取双精度浮点数
     *
     * @return 返回双精度浮点数
     * @author xfherun 何润
     */
    public abstract double doubleValue();

    /**
     * 获取大整数
     *
     * @return 返回大整数
     * @author xfherun 何润
     */
    public abstract BigInteger bigIntegerValue();

    /**
     * 获取大浮点数
     *
     * @return 返回大浮点数
     * @author xfherun 何润
     */
    public abstract BigDecimal bigDecimalValue();

    @Override
    public boolean equals(Object o)
    {
        if (this == o) return true;
        if (!(o instanceof NumberBasicType<?> that)) return false;
        return Objects.equals(number, that.number);
    }

    @Override
    public int hashCode()
    {
        return Objects.hash(number);
    }

    @Override
    public String toString()
    {
        return String.valueOf(number);
    }
}

```

6.5.2 Key 类

```java
package com.xfherun.datatag.datatags;

import java.io.Serializable;
import java.util.Objects;
import java.util.concurrent.ThreadLocalRandom;

/**
 * 键
 * project: DataTag
 *
 * @author xfherun 何润
 */
public class Key implements Serializable
{
    /**
     * 键的语法校验正则表达式
     */
    public static final String KEY_REGEX = "([A-Z][a-zA-Z\\d_$]+)|([A-Z])";

    /**
     * 键
     */
    private final String key;

    /**
     * 创建键
     *
     * @param key 键
     * @author xfherun 何润
     */
    protected Key(String key)
    {
        if (key == null) throw new NullPointerException("'key' == null!");
        if (key.trim().isEmpty()) throw new IllegalArgumentException("'key' is empty!");
        if (!key.matches(KEY_REGEX))
            throw new IllegalArgumentException("Invalid key: '" + key + "'! Regex expression: '" + KEY_REGEX + "'!");
        this.key = key.trim();
    }

    /**
     * 获取 key 的值
     *
     * @return key 的值
     * @author xfherun 何润
     */
    public String getKey()
    {
        return key;
    }

    @Override
    public boolean equals(Object o)
    {
        if (this == o) return true;
        if (!(o instanceof Key that)) return false;
        return key.equals(that.key);
    }

    @Override
    public int hashCode()
    {
        return Objects.hash(key);
    }

    @Override
    public String toString()
    {
        return key;
    }

    /**
     * 创建键
     *
     * @param key 键
     * @return 返回Key
     * @author xfherun 何润
     */
    public static Key of(String key)
    {
        return new Key(key);
    }

    /**
     * 创建键
     *
     * @param key 键
     * @return 返回Key
     * @author xfherun 何润
     */
    public static Key greedy(String key)
    {
        if (key != null && key.matches(KEY_REGEX))
            return Key.of(key);
        if (key == null || key.isEmpty())
        {
            StringBuffer sb = new StringBuffer();
            int count = ThreadLocalRandom.current().nextInt(1, 33);
            char[] tables = {
                    (char) ThreadLocalRandom.current().nextInt('A', 'Z' + 1),
                    (char) ThreadLocalRandom.current().nextInt('a', 'z' + 1),
                    (char) ThreadLocalRandom.current().nextInt('0', '9' + 1),
                    '_',
                    '$'
            };
            L:
            for (int i = 0; i < count; i++)
            {
                char chr = tables[ThreadLocalRandom.current().nextInt(0, tables.length)];
                while (true)
                {
                    if (i == 0)
                    {
                        if (chr >= 'A' && chr <= 'Z')
                        {
                            sb.append(chr);
                            continue L;
                        }
                        chr = tables[ThreadLocalRandom.current().nextInt(0, tables.length)];
                    }
                    else break;
                }
                sb.append(chr);
            }
            return new Key(sb.toString());
        }
        if (key.length() == 1)
        {
            char c = key.charAt(0);
            if (c >= 'A' && c <= 'Z') return new Key(key);
            key = key.toUpperCase();
            if (c >= '0' && c <= '9') return new Key("Number" + key);
            else if (c == '_' || c == '$') return new Key("Other" + key);
            return new Key(key);
        }
        StringBuffer sb = new StringBuffer();
        for (int i = 0; i < key.length(); i++)
        {
            if (i == 0)
            {
                String s = String.valueOf(key.charAt(i)).toUpperCase();
                if (s.charAt(0) >= 'A' && s.charAt(0) <= 'Z')
                    sb.append(s);
                else
                {
                    if (s.charAt(0) >= '0' && s.charAt(0) <= '9')
                        sb.append("Number").append(s);
                    else if (s.charAt(0) == '_' || s.charAt(0) == '$')
                        sb.append("Other").append(s);
                }
            }
            else sb.append(key.charAt(i));
        }
        return new Key(sb.toString());
    }
}

```

6.5.3 常量池相关

```java
// 获取常量池
TreeMap<Integer, DataTag> constPool = composite.getConstPool();

// 打印常量池内容
String poolInfo = composite.printConstPool();

// 查看二进制数据
String binaryView = composite.viewBinaryData("ascii");  // "ascii" 或 "utf-8"
```

---

🎉 结束语

至此，你已经掌握了 DataTag 的全部语法和 Java 使用方法。从简单的数据类型到高级的引用与方法调用，DataTag 为你提供了一种灵活、强大的数据表达方式。

主要特性回顾：

· ✅ 丰富的数据类型（基本类型、容器类型、引用类型）
· ✅ 支持引用，避免数据重复
· ✅ 类型转换系统
· ✅ 方法调用扩展
· ✅ 高效的二进制序列化
· ✅ 常量池优化

如果在使用过程中遇到问题，可以借助 IDE 的代码提示或私信我或查阅相关源码。祝你编码愉快！
