---
markmap:
  autoFit: false
  colorFreezeLevel: 4
  initialExpandLevel: 3
  maxWidth: 500
  spacingHorizontal: 90
  spacingVertical: 10
---

# Java 知识图谱

## 基础

### JVM、JRE 和 JDK 的关系是什么

#### JDK

- 功能齐全的 Java SDK
- JRE + javac + javadoc 等
- 创建和编译程序

#### JRE

- Java Runtime Environment
- 运行 Java 程序所需全部内容的集合
- JVM + Java 类库 + Java 命令等

#### JVM

- Java Virtual Machine
- Java 虚拟机

### 字节码

#### 什么是字节码

- 十六进制组成
- JVM 以两个十六进制值组成
- javac 命令编译源代码为字节码

#### 为什么使用字节码

- 无论什么平台（JVM 为操作系统做了定制）都可以编译出固定格式的字节码（.class 文件）供 JVM 使用，以实现一次编译，到处运行

### Oracle JDK 和 OpenJDK 的区别

#### Oracle JDK

- 三年发布一次
- OpenJDK 的一个实现，不完全开源
- 相对更稳定
- 不提供长期支持

#### OpenJDK

- 三个月发布一次
- 开源参考模型
- 不如 Oracle 稳定
- 提供长期支持

### 数据类型

#### 基本数据类型

##### 数值型

###### 整数类型

- byte 1 字节
- short 2 字节
- int 4 字节
- long 8 字节

###### 浮点类型

- float 4 字节 1 位符号位 + 8 位指数位 + 23 位尾数位
- double 8 字节

##### 字符型

- char 2 字节

##### 布尔型

- boolean 1 字节

#### 引用数据类型

- 类
- 接口
- 数组

### 访问修饰符

#### default

##### 可见范围

- 同一包内可见

##### 使用对象

- 类
- 接口
- 变量
- 方法

#### private

##### 可见范围

- 同一类内可见

##### 使用对象

- 变量
- 方法

#### public

##### 可见范围

- 所有类

##### 使用对象

- 类
- 接口
- 变量
- 方法

#### protected

##### 可见范围

- 同一包内类和子类可见

##### 使用对象

- 变量
- 方法

### final、finally、finalize 区别

#### final

##### 变量

- 必须初始化
- 引用不可变

##### 方法

- 不可被重写

##### 类

- 不可被继承

#### finally

- 只能用在 try/catch 语句中
- 通常用于释放资源
- 会被 System.exit(0) 打断

#### finalize

- Object 方法
- 在 GC 启动，对象被回收时候调用
- 是会调用一次，但不一定会立即回收，后续可能又不需要回收了；因此产生冲突，不建议使用

### 静态变量、代码块、静态方法执行顺序

- 静态代码块 -> 构造代码块 -> 构造函数 -> 普通代码块

### 面向过程 / 面向对象

#### 面向过程

##### 优点

- 性能高，类调用不需要实例化

##### 缺点

- 不易维护/复用/扩展

#### 面向对象

##### 三大特性

###### 封装

- 客观事物封装成抽象的类
- 类可以控制数据和方法只被可信的类或对象操作

###### 继承

- 无需编写原来的类的情况下进行功能扩展

###### 多态

- 同一个属性或方法在父类及其各个子类中具有不同的含义
- 编译时多态：重载（Overload）
- 运行时多态：子类重写方法（Override）

### 创建对象方式

#### new 创建新对象

- 编译期确定要创建的类型
- 调用类的构造方法
- 类型检查更加严格
- 性能通常较好

#### 反射创建新对象

- 运行时动态获取 Class 对象
- 通过构造方法创建对象
- 常用于 Spring、MyBatis 等框架
- 可以在不知道具体类型的情况下创建对象
- 性能通常低于直接使用 new

#### clone 创建新对象

- 通过已有对象复制出一个新对象
- Object 提供了 `clone()` 方法
- 类需要实现 Cloneable 接口
- Cloneable 只是标记接口，不提供方法
- `clone()` 默认执行浅拷贝
- `clone()` 通常需要重写为 public

##### 深拷贝

- 创建一个新的对象
- 基本类型属性复制值
- 引用类型属性也会创建新的对象
- 原对象和新对象之间不存在共享的可变引用对象

##### 浅拷贝

- 创建一个新的对象
- 基本类型属性会复制值
- 引用类型属性只复制引用地址
- 原对象和新对象会共享引用类型属性

#### 序列化机制

##### 实现 Serializable

- Serializable 是标记接口
- 不需要实现具体方法
- 使用 ObjectOutputStream 序列化
- 使用 ObjectInputStream 反序列化
- 反序列化时通常不会调用可序列化类的构造方法
- 第一个不可序列化父类的无参构造方法会被调用

##### 实现 Externalizable

- Externalizable 继承自 Serializable
- 必须实现 `writeExternal()` 和 `readExternal()`
- 由开发者完全控制序列化和反序列化过程
- 必须提供 public 无参构造方法
- 反序列化时会先调用无参构造方法
- 可以只序列化需要的字段
- 灵活性高，但编码成本更高

### `==`、`equals`、`hashCode`

#### `==`

##### 比较基本类型

- 比较两个变量的值是否相等
- 例如：`int a = 10; int b = 10;`
- `a == b` 的结果为 `true`

##### 比较引用类型

- 比较两个引用变量是否指向同一个对象
- 不比较对象的具体内容
- 例如两个内容相同但地址不同的对象，使用 `==` 比较结果可能为 `false`

#### `equals`

- `equals` 方法定义在 `Object` 类中
- `Object.equals()` 默认比较对象地址
- 子类可以重写 `equals`，按照业务规则比较对象内容
- `String`、包装类等通常已经重写了 `equals`

#### `hashCode`

- `hashCode` 是对象的哈希值
- 默认由 `Object` 提供
- 主要用于 `HashMap`、`HashSet` 等哈希容器
- 重写 `equals` 时通常必须重写 `hashCode`

#### `equals` 和 `hashCode` 的约定

- 如果两个对象 `equals` 返回 `true`
- 那么两个对象的 `hashCode` 必须相同
- 如果两个对象的 `hashCode` 相同
- 两个对象不一定相等
- 不同对象可能产生相同的哈希值

#### HashMap 中的使用

- 先调用 `hashCode` 计算对象所在的桶
- 再调用 `equals` 判断对象是否相等
- `hashCode` 用于快速定位
- `equals` 用于最终确认

#### String 的比较

##### 使用 `==`

- 比较两个字符串对象的引用地址
- 不推荐使用 `==` 比较字符串内容

##### 使用 `equals`

- 比较两个字符串的实际内容
- 推荐使用 `equals` 比较字符串

#### 字符串常量池

- 字符串字面量会优先从字符串常量池中获取
- 内容相同的字符串字面量可能指向同一个对象
- 因此某些情况下使用 `==` 比较字符串可能返回 `true`
- 不能因此认为 `==` 可以比较字符串内容

#### equals 的特性

##### 自反性

- `x.equals(x)` 必须返回 `true`

##### 对称性

- 如果 `x.equals(y)` 返回 `true`
- 那么 `y.equals(x)` 也必须返回 `true`

##### 传递性

- 如果 `x.equals(y)` 返回 `true`
- `y.equals(z)` 返回 `true`
- 那么 `x.equals(z)` 也必须返回 `true`

##### 一致性

- 对象没有发生变化时
- 多次调用 `equals` 的结果应该一致

##### 非空性

- `x.equals(null)` 必须返回 `false`

#### 重写不完整的问题

- 只重写 `equals`，不重写 `hashCode`
- 逻辑相等的对象可能拥有不同的哈希值
- 放入 `HashSet` 后可能出现重复数据
- 从 `HashMap` 中可能无法正确获取数据

#### 实际开发注意事项

- 重写 `equals` 和 `hashCode` 时使用相同的字段
- 不要使用会变化的字段作为 HashMap 的 Key
- 对象放入集合后，不要修改参与计算的字段
- 推荐使用 `Objects.equals()` 处理可能为 `null` 的字段
- 推荐使用 `Objects.hash()` 生成哈希值
- 可以使用 IDE 自动生成 `equals` 和 `hashCode`
- 继承关系复杂时要注意 `equals` 的对称性

#### 总结

- `==`：基本类型比较值，引用类型比较地址
- `equals`：默认比较地址，重写后可以比较业务内容
- `hashCode`：用于哈希容器快速定位
- 重写 `equals` 时必须同步重写 `hashCode`
- `equals` 相等的对象，`hashCode` 必须相等
- `hashCode` 相等的对象，`equals` 不一定相等

### String、StringBuffer、StringBuilder

#### String

- `String` 是不可变字符串
- `String` 类使用 `final` 修饰，不能被继承
- 字符串内容创建后不能被修改
- 对字符串进行修改时，会创建新的 String 对象
- 适合字符串内容不经常变化的场景
- 线程安全，因为对象不可变

##### 字符串常量池

- 字符串字面量会优先放入字符串常量池
- 内容相同的字符串字面量通常会复用同一个对象
- 可以通过 `intern()` 将字符串放入字符串常量池
- 使用 `new String()` 通常会创建新的字符串对象

##### 示例

- `String a = "Java";`
- `String b = "Java";`
- `a` 和 `b` 可能指向常量池中的同一个对象
- `String c = new String("Java");`
- `c` 通常是堆中的新对象

##### 字符串拼接

- 使用 `+` 拼接字符串时，编译器可能转换为 `StringBuilder`
- 循环中使用 `+` 可能创建大量临时对象
- 循环拼接字符串时，推荐使用 `StringBuilder`

##### 底层存储

- Java 8 中，String 底层主要使用 `char[]`
- Java 9 以后，String 底层使用 `byte[]`
- Java 9 引入 Compact Strings
- Latin-1 字符使用一个字节存储
- 其他字符通常使用两个字节存储

#### StringBuffer

- `StringBuffer` 是可变字符串
- 修改字符串时通常不会创建新的对象
- 方法使用 `synchronized` 修饰
- 线程安全
- 多线程环境下可以使用
- 性能通常低于 `StringBuilder`

##### 适用场景

- 多个线程共同修改同一个字符串对象
- 需要保证字符串操作线程安全

#### StringBuilder

- `StringBuilder` 是可变字符串
- 修改字符串时通常不会创建新的字符串对象
- 方法没有使用 `synchronized`
- 线程不安全
- 性能通常高于 `StringBuffer`
- 单线程环境下优先使用

##### 适用场景

- 单线程字符串拼接
- 循环拼接字符串
- 构建 SQL、JSON 或日志内容

#### 三者对比

##### String

- 不可变
- 线程安全
- 适合内容不经常变化的字符串
- 频繁修改时性能较低

##### StringBuffer

- 可变
- 方法使用 `synchronized`
- 线程安全
- 多线程环境下性能相对较低

##### StringBuilder

- 可变
- 方法没有同步控制
- 线程不安全
- 单线程环境下性能较高

#### 性能对比

- 频繁修改字符串时：
  - `StringBuilder` 通常最快
  - `StringBuffer` 次之
  - `String` 通常最慢
- `String` 每次修改都可能创建新的对象
- `StringBuilder` 和 `StringBuffer` 通常在原有缓冲区上修改
- 实际性能还与字符串长度、扩容次数和使用方式有关

#### StringBuilder 扩容

- `StringBuilder` 内部维护字符缓冲区
- 初始容量不足时会自动扩容
- 默认初始容量通常为 `16`
- 扩容后容量通常按照旧容量乘以 `2` 再加 `2`
- 频繁扩容会产生额外开销
- 可以根据预计长度指定初始容量

#### 常用方法

##### `append`

- 追加字符串
- 可以追加基本类型、对象和字符数组

##### `insert`

- 在指定位置插入内容

##### `delete`

- 删除指定范围内的内容

##### `reverse`

- 反转字符串内容

##### `toString`

- 将可变字符串转换为 String 对象

#### 线程安全说明

- `String` 的线程安全来自不可变性
- 多个线程可以安全共享 String 对象
- `StringBuffer` 的线程安全来自方法同步
- `StringBuilder` 没有同步机制
- 多线程环境下不要让多个线程同时修改同一个 StringBuilder

#### 选择建议

- 字符串不修改或修改次数很少：使用 `String`
- 单线程频繁拼接：使用 `StringBuilder`
- 多线程共享并修改同一个对象：使用 `StringBuffer`
- 多线程场景下也可以使用线程隔离的 `StringBuilder`
- 如果可以避免共享状态，通常优先使用 `StringBuilder`

#### 总结

- `String`：不可变字符串
- `StringBuffer`：线程安全的可变字符串
- `StringBuilder`：非线程安全的可变字符串
- `String` 适合不经常修改的场景
- `StringBuilder` 适合单线程频繁拼接
- `StringBuffer` 适合多线程共享修改

### 反射

#### 什么是反射

- Java 在运行时获取类信息并操作对象的机制
- 可以获取类名、父类、接口、构造器、字段、方法和注解
- 可以在运行时创建对象、调用方法和操作字段
- 将部分操作从编译期推迟到运行期

#### 原理

- 类加载后会生成对应的 `Class` 对象
- `Class` 对象保存类的元数据信息
- 反射 API 通过 `Class` 对象获取类结构
- 再通过构造器、字段和方法操作对象

#### 获取 Class 对象

##### `.class`

- 编译期已知类的类型时使用
- 通常不会触发类的初始化
- 例如：`Class<User> clazz = User.class`

##### `getClass()`

- 已经拥有对象实例时使用
- 例如：`Class<?> clazz = user.getClass()`

##### `Class.forName()`

- 根据全限定类名动态加载类
- 常用于配置文件、插件和框架
- 默认会触发类的初始化
- 例如：`Class<?> clazz = Class.forName("com.example.User")`

#### 常用 API

##### `Class`

- 获取类的基本信息
- 获取父类、接口、构造器、字段、方法和注解

##### `Constructor`

- 获取构造方法
- 创建对象实例

##### `Field`

- 获取、读取和修改成员变量

##### `Method`

- 获取和调用成员方法

##### `Modifier`

- 判断 `public`、`private`、`static`、`final` 等修饰符

#### 获取成员

##### 构造器

- `getConstructor()`
  - 获取公共构造器
- `getDeclaredConstructor()`
  - 获取当前类声明的构造器，包括私有构造器

##### 字段

- `getField()`
  - 获取公共字段
- `getDeclaredField()`
  - 获取当前类声明的字段，包括私有字段

##### 方法

- `getMethod()`
  - 获取公共方法，包括父类方法
- `getDeclaredMethod()`
  - 获取当前类声明的方法，包括私有方法

#### 使用步骤

- 获取 `Class` 对象
- 获取构造器、字段或方法
- 设置访问权限
- 创建对象、读取字段、修改字段或调用方法

#### 创建对象

- 推荐使用 `Constructor.newInstance()`
- `Class.newInstance()` 已不推荐使用
- `Constructor.newInstance()` 可以调用指定参数的构造方法
- 例如：`Object user = constructor.newInstance("张三")`

#### 操作字段和方法

- 使用 `Field.get()` 获取字段值
- 使用 `Field.set()` 修改字段值
- 使用 `Method.invoke()` 调用方法
- 私有成员可能需要调用 `setAccessible(true)`
- Java 9 以后受到模块系统访问限制

#### 应用

##### Spring

- 扫描类和注解
- 创建 Bean
- 完成依赖注入
- 创建 AOP 代理

##### MyBatis

- 创建实体对象
- 获取实体字段
- 完成数据库字段和实体属性映射

##### JUnit

- 扫描测试类和测试方法
- 执行带有测试注解的方法

##### 插件机制

- 根据配置动态加载实现类
- 创建插件对象并调用统一接口

##### 注解处理

- 获取类、字段和方法上的注解
- 实现参数校验、权限校验和路由映射

#### 优点

- 支持运行时动态操作
- 灵活性高
- 适合框架和通用工具开发
- 可以根据配置动态加载实现类

#### 缺点

- 性能通常低于直接调用
- 编译期无法发现部分错误
- 可读性和类型安全性较差
- 可能破坏封装性
- 异常处理更加复杂
- 受到访问权限和模块系统限制

#### 总结

- `Class` 是反射的核心入口
- `Constructor` 用于创建对象
- `Field` 用于操作字段
- `Method` 用于调用方法
- 反射是 Spring、MyBatis、JUnit 等框架的重要基础
- 业务代码中应谨慎使用反射

### 泛型

#### 基本概念

##### 什么是泛型

- Java 提供的参数化类型机制
- 定义类、接口和方法时使用类型参数
- 使用时指定具体类型
- 提高类型安全性
- 减少类型转换
- 提高代码复用性

##### 类型参数命名

- `T`：类型
- `E`：元素
- `K`：键
- `V`：值
- `N`：数字
- `?`：未知类型

#### 泛型使用

##### 泛型类

- 在类定义时声明类型参数
- 例如：`class Box<T>`

##### 泛型接口

- 在接口定义时声明类型参数
- 例如：`interface Repository<T>`

##### 泛型方法

- 方法独立声明类型参数
- 类型参数写在返回值前面
- 例如：`static <T> T getFirst(List<T> list)`

#### 通配符和上下界

##### 通配符 `?`

- 表示未知类型
- 适合只读取或判断集合
- 不能向 `List<?>` 中添加具体对象
- `null` 除外

##### 泛型上界 `extends`

- 类型必须是指定类型或其子类
- 适合读取数据
- 不适合向集合中写入具体类型对象
- 例如：`List<? extends Number>`

##### 泛型下界 `super`

- 类型必须是指定类型或其父类
- 适合向集合中写入数据
- 读取结果通常只能按照 `Object` 处理
- 例如：`List<? super Integer>`

##### PECS 原则

- Producer Extends
  - 生产者使用 `extends`
  - 主要用于读取数据
- Consumer Super
  - 消费者使用 `super`
  - 主要用于写入数据

#### 类型擦除

##### 类型擦除

- 泛型主要在编译期生效
- 编译后通常会擦除泛型类型信息
- 泛型类型会被替换为原始类型或上界类型
- `List<String>` 和 `List<Integer>` 运行时通常都是 `List`

##### 类型擦除影响

- 不能使用基本类型作为泛型参数
- 只能使用包装类型
- 不能直接创建泛型数组
- 不能直接创建 `T` 类型的对象
- 不能在静态成员中使用类级别的泛型参数
- 不能使用 `instanceof List<String>`

#### 泛型特性

##### 优点

- 编译期进行类型检查
- 减少强制类型转换
- 提高代码复用性
- 提高代码可读性
- 降低类型转换异常风险

##### 缺点

- 语法相对复杂
- 存在类型擦除
- 不能直接使用基本类型
- 通配符和上下界较难理解
- 某些场景需要额外保存类型信息

#### 总结

- 泛型是参数化类型
- 泛型主要用于编译期类型检查
- `extends` 适合读取数据
- `super` 适合写入数据
- 记忆原则：Producer Extends，Consumer Super
- Java 泛型通过类型擦除实现

### 集合 Collection

#### List

- 有序
- 允许重复元素
- 允许使用索引访问
- 常见实现：`ArrayList`、`LinkedList`、`Vector`

##### ArrayList

- 底层使用动态数组
- 查询速度快，时间复杂度接近 `O(1)`
- 中间插入和删除速度较慢
- 允许存储 `null`
- 线程不安全

###### 扩容机制

- 默认初始容量通常为 `10`
- 容量不足时自动扩容
- 新容量通常为原容量的 `1.5` 倍
- 扩容时需要创建新数组并复制原数据
- 可以通过构造方法指定初始容量

##### LinkedList

- 底层使用双向链表
- 查询速度较慢
- 头部和尾部插入、删除速度较快
- 同时实现了 `List` 和 `Deque`
- 允许存储 `null`
- 线程不安全

##### Vector

- 底层使用动态数组
- 方法使用 `synchronized` 修饰
- 线程安全
- 并发性能较低
- 扩容时默认扩大为原容量的 `2` 倍
- 现在通常使用 `ArrayList` 替代

#### Queue

- 队列通常遵循先进先出
- 常用于任务排队和消息处理
- 常见实现：`LinkedList`、`PriorityQueue`、`ArrayDeque`

##### Deque

- 双端队列
- 可以从队头和队尾插入、删除元素
- 可以作为队列使用
- 可以作为栈使用
- 常用实现：`ArrayDeque`、`LinkedList`

##### PriorityQueue

- 优先级队列
- 底层使用堆结构
- 默认是小根堆
- 队头元素是优先级最高的元素
- 不保证遍历结果完全有序
- 不允许存储 `null`
- 线程不安全

#### Set

- 元素唯一
- 通常不允许重复元素
- 是否有序取决于具体实现

##### HashSet

- 底层基于 `HashMap`
- 元素作为 `HashMap` 的 Key
- 使用固定的占位对象作为 Value
- 查询、添加和删除平均时间复杂度接近 `O(1)`
- 无序
- 允许存储一个 `null`
- 线程不安全

###### LinkedHashSet

- 继承自 `HashSet`
- 底层基于 `LinkedHashMap`
- 通过双向链表维护插入顺序
- 元素唯一
- 查询、添加和删除平均时间复杂度接近 `O(1)`
- 允许存储一个 `null`

##### SortedSet

- 有序集合接口
- 元素会按照自然顺序或比较器排序
- 常见实现是 `TreeSet`

###### TreeSet

- 底层基于 `TreeMap`
- 底层使用红黑树
- 元素唯一
- 默认按照自然顺序排序
- 可以通过 `Comparator` 自定义排序
- 增删改查时间复杂度接近 `O(logN)`
- 不允许存储 `null`，除非比较器支持

##### EnumSet

- 专门用于存储枚举类型
- 底层通常使用位向量实现
- 性能高，内存占用少
- 元素唯一
- 按枚举定义顺序遍历
- 不允许存储 `null`

#### Map

- 保存键值对
- Key 不能重复
- Value 可以重复
- 一个 Key 最多对应一个 Value
- 常见实现：`HashMap`、`LinkedHashMap`、`TreeMap`、`Hashtable`

##### HashMap

- 底层使用数组、链表和红黑树
- Key 允许一个 `null`
- Value 允许多个 `null`
- 线程不安全
- 查询、添加和删除平均时间复杂度接近 `O(1)`

###### 底层数据结构

- 数组：保存哈希桶
- 链表：解决哈希冲突
- 红黑树：优化过长链表的查询效率
- Java 8 以后，链表长度较长时可能转换为红黑树

###### 解决 Hash 冲突

- 不同 Key 计算出相同的数组下标时会产生哈希冲突
- HashMap 使用链表或红黑树保存冲突元素
- 先比较哈希值
- 哈希值相同时再使用 `equals()` 比较 Key
- `equals()` 相等时覆盖原 Value
- `equals()` 不相等时保存为新的节点

###### `put` 流程

- 计算 Key 的 `hashCode`
- 对哈希值进行扰动处理
- 根据哈希值计算数组下标
- 如果桶为空，直接创建节点
- 如果桶不为空，比较 Key 是否相同
- Key 相同则覆盖原 Value
- Key 不同则插入链表或红黑树
- 插入后判断是否需要扩容
- 链表过长时可能转换为红黑树

###### 扩容方式

- 默认初始容量通常为 `16`
- 默认负载因子为 `0.75`
- 扩容阈值等于容量乘以负载因子
- 超过阈值后扩容
- 新容量通常为原容量的 `2` 倍
- 扩容后需要重新分配节点位置
- Java 8 中扩容时节点通常只在原位置或原位置加旧容量的位置移动

###### 链表树化

- 链表长度达到 `8` 时尝试树化
- 数组容量小于 `64` 时优先扩容
- 数组容量达到 `64` 后，链表才可能转换为红黑树
- 红黑树节点数量较少时可能退化为链表

##### LinkedHashMap

- 继承自 `HashMap`
- 底层使用数组、链表、红黑树和双向链表
- 可以维护插入顺序
- 也可以维护访问顺序
- 允许一个 `null` Key 和多个 `null` Value
- 可以用于实现简单的 LRU 缓存

##### ConcurrentHashMap

- 线程安全的 Map
- 适合高并发读写场景
- 不允许 `null` Key 和 `null` Value
- 通过分段或桶级别的锁减少并发冲突
- 相比 `Hashtable`，并发性能更高

###### JDK 1.7 底层结构

- `Segment[]` 数组
- 每个 `Segment` 内部维护一个 `HashEntry[]` 数组
- `Segment` 继承自 `ReentrantLock`
- 每个 Segment 是一个独立的锁
- 不同 Segment 可以同时执行写操作
- 同一个 Segment 内的写操作需要竞争锁

###### JDK 1.7 结构

- ConcurrentHashMap
  - `Segment[]`
    - `Segment`
      - `HashEntry[]`
        - `HashEntry`
        - `HashEntry`
        - `HashEntry`

###### JDK 1.7 并发特点

- 将整个 Map 划分成多个 Segment
- 每个 Segment 管理一部分数据
- 写入不同 Segment 时可以并发执行
- 写入同一个 Segment 时需要加锁
- 默认最多支持多个 Segment 并发写入
- 读取通常不需要加锁

###### JDK 8 底层结构

- 底层结构与 `HashMap` 类似
- 数组、链表和红黑树
- 数组类型为 `Node<K,V>[]`
- 链表节点类型为 `Node<K,V>`
- 红黑树节点类型为 `TreeBin`
- 红黑树中的节点类型为 `TreeNode`
- 使用 CAS 和 `synchronized` 保证并发安全

###### JDK 8 加锁粒度

- JDK 7 主要锁定 Segment
- JDK 8 不再使用 Segment
- JDK 8 主要锁定发生冲突的桶
- 不同桶可以同时执行写操作
- 锁的粒度比 JDK 7 更细
- 读取通常不需要加锁

###### JDK 8 `put` 流程

- 计算 Key 的哈希值
- 对哈希值进行扰动处理
- 根据哈希值计算数组下标
- 判断数组是否初始化
- 未初始化时，使用 CAS 初始化数组
- 判断当前桶是否为空
- 桶为空时，使用 CAS 放入新节点
- 桶不为空时，锁定当前桶
- 判断桶首节点是否正在扩容
- 如果正在扩容，则协助数据迁移
- 如果是链表，则遍历链表
- Key 相同则更新 Value
- Key 不同则追加新节点
- 如果是红黑树，则按照红黑树规则插入
- 链表长度达到树化条件时转换为红黑树
- 更新元素数量
- 达到扩容阈值时触发扩容

###### JDK 8 `put` 的 CAS 和锁

- CAS 主要用于：
  - 初始化数组
  - 向空桶写入第一个节点
  - 更新部分控制变量
- `synchronized` 主要用于：
  - 锁定非空桶
  - 修改桶中的链表
  - 修改桶中的红黑树
- CAS 失败后不代表整个 Map 被锁住
- 通常只需要竞争当前桶

###### JDK 8 `get` 流程

- 计算 Key 的哈希值
- 根据哈希值计算数组下标
- 读取对应桶
- 桶为空时返回 `null`
- 桶首节点 Key 匹配时直接返回 Value
- 桶是链表时遍历链表
- 桶是红黑树时按照红黑树规则查找
- 找到 Key 后返回 Value
- 找不到 Key 时返回 `null`

###### 为什么 `get` 通常不加锁

- Node 的 `hash` 和 `key` 通常不可变
- Node 的 `val` 使用 `volatile` 修饰
- Node 的 `next` 使用 `volatile` 修饰
- `volatile` 保证读取线程能够看到最新值
- 读操作不会修改链表或红黑树结构
- 因此读取通常不需要加锁

###### `volatile` 的作用

- 保证 Value 的可见性
- 保证链表 next 指针的可见性
- 防止读取线程使用过期数据
- 不能单独保证复合操作的原子性
- 复合修改操作仍需要 CAS 或锁

###### JDK 8 扩容

- 扩容时创建容量更大的数组
- 原数组容量通常扩大为原来的 `2` 倍
- 多个线程可以协助扩容
- 正在扩容的桶会被标记
- 其他线程访问该桶时可以协助迁移数据
- 链表节点根据哈希值拆分到新数组的两个位置
- 红黑树节点也会参与数据迁移
- 数据迁移完成后更新新数组中的桶

###### 扩容中的特殊节点

- 扩容过程中会使用 `ForwardingNode`
- `ForwardingNode` 表示当前桶的数据已经迁移
- 其他线程访问到该节点时，会到新数组中查找
- 其他线程也可以协助完成扩容
- 扩容完成后，旧数组逐渐被新数组替代

###### 计数方式

- JDK 8 不使用单一锁保护元素数量
- 低并发时使用 `baseCount`
- 高并发更新时使用 `CounterCell[]`
- 不同线程可以更新不同的 CounterCell
- 最终统计各个 CounterCell 的值
- 减少多个线程更新同一个计数变量的竞争

###### 与 HashMap 的区别

- `HashMap` 线程不安全
- `ConcurrentHashMap` 线程安全
- `HashMap` 允许 `null` Key 和 `null` Value
- `ConcurrentHashMap` 不允许 `null` Key 和 `null` Value
- `HashMap` 写操作可能导致数据结构异常
- `ConcurrentHashMap` 使用 CAS 和锁保证并发安全

###### 与 Hashtable 的区别

- `Hashtable` 的方法通常使用 `synchronized`
- `Hashtable` 锁的粒度较大
- 高并发下多个操作容易相互阻塞
- `ConcurrentHashMap` 使用更细粒度的桶级别控制
- `ConcurrentHashMap` 读操作通常不加锁
- `ConcurrentHashMap` 并发性能通常高于 `Hashtable`
- 两者都不允许 `null` Key 和 `null` Value

###### 弱一致性

- 遍历期间允许其他线程执行修改
- 迭代器不会抛出 `ConcurrentModificationException`
- 遍历结果不一定反映某一时刻的完整快照
- 可能读取到修改前或修改后的部分数据
- 适合并发场景下的弱一致性遍历
- 不适合依赖遍历结果进行严格业务判断

###### 常见复合操作

- `putIfAbsent()`
  - Key 不存在时才写入
- `computeIfAbsent()`
  - Key 不存在时计算并写入 Value
- `computeIfPresent()`
  - Key 存在时重新计算 Value
- `merge()`
  - 根据旧值和新值合并数据
- 这些方法可以保证单次复合操作的原子性
- 多个独立操作组合在一起时，仍需要额外控制

###### 注意事项

- 不要使用 `null` 表示特殊业务状态
- 不要依赖遍历结果作为精确快照
- 不要把多个 Map 操作简单组合后认为整体原子
- 需要复合更新时，优先使用 `putIfAbsent()`、`compute()` 或 `merge()`
- 需要严格一致性时，应使用额外锁或事务控制

###### 总结

- JDK 1.7：`Segment` 分段锁
- JDK 8：数组、链表、红黑树、CAS 和 `synchronized`
- JDK 7 锁的是 Segment
- JDK 8 主要锁的是发生冲突的桶
- 空桶写入主要依靠 CAS
- 非空桶修改主要依靠 `synchronized`
- `get` 通常不加锁，依靠 `volatile` 保证可见性
- 扩容时多个线程可以协助迁移
- ConcurrentHashMap 只保证单次操作的线程安全
- 多个操作组合时仍需考虑原子性

##### Hashtable

- 线程安全的哈希表
- 方法使用 `synchronized` 修饰
- 不允许 `null` Key
- 不允许 `null` Value
- 并发性能低于 `ConcurrentHashMap`
- 现在通常使用 `ConcurrentHashMap` 替代

###### Properties

- `Hashtable` 的子类
- 主要用于保存配置项
- Key 和 Value 通常都是字符串
- 支持从文件读取和写入配置
- 常用于 `.properties` 配置文件

##### SortedMap

- 有序 Map 接口
- 按照 Key 的自然顺序或比较器排序
- 常见实现是 `TreeMap`

###### TreeMap

- 底层使用红黑树
- Key 唯一
- 按照 Key 排序
- 支持自然排序和自定义排序
- 增删改查时间复杂度接近 `O(logN)`
- 不允许存储 `null` Key，除非比较器支持
- Value 可以为 `null`
- 线程不安全

#### 常见集合对比

##### ArrayList 和 LinkedList

- 随机访问多：使用 `ArrayList`
- 频繁头尾插入和删除：可以使用 `LinkedList`
- 实际开发中通常优先考虑 `ArrayList`
- `LinkedList` 的节点对象会产生额外内存开销

##### HashMap 和 TreeMap

- 需要快速查找：使用 `HashMap`
- 需要按照 Key 排序：使用 `TreeMap`
- `HashMap` 平均复杂度接近 `O(1)`
- `TreeMap` 操作复杂度接近 `O(logN)`

##### HashSet 和 TreeSet

- 只要求元素唯一：使用 `HashSet`
- 要求元素有序：使用 `TreeSet`
- `HashSet` 平均操作复杂度接近 `O(1)`
- `TreeSet` 操作复杂度接近 `O(logN)`

##### HashMap 和 ConcurrentHashMap

- 单线程环境：使用 `HashMap`
- 多线程环境：使用 `ConcurrentHashMap`
- `HashMap` 不保证线程安全
- `ConcurrentHashMap` 支持并发读写
- `ConcurrentHashMap` 不允许 `null` Key 和 `null` Value

## Spring

### Spring 的好处

#### 轻量级

- 体积小，核心包约 2MB

#### 控制反转

- 降低对象之间的耦合
- 对象依赖由容器提供，不再由对象自行创建或查找

#### 面向切面编程 AOP

##### 将业务逻辑和系统级服务分离

##### 实现方式

###### 静态代理

- 使用 AOP 框架提供的命令进行编译
- 在编译阶段就可生成 AOP 代理类

###### 动态代理

- JDK 动态代理：通过==反射==来接收被代理的类，并且要求被代理的类必须实现一个接口，核心是 `InvocationHandler` 接口和 `Proxy` 类
- CGLIB 动态代理：通过==继承==的方式做动态代理，如果某个类被标记为 `final`，那么它无法使用 CGLIB 做动态代理

#### 容器

- 管理对象的生命周期和配置

#### MVC 框架

- 提供 Web 应用分层开发支持

#### 事务管理

- 提供统一的事务管理接口
- 支持本地事务和全局事务扩展

#### 异常处理

- 提供 API 将具体异常转换为统一的 unchecked 异常

#### 设计模式

##### 工厂

- 通过 `BeanFactory`、`ApplicationContext` 创建 Bean 对象

##### 代理

- AOP

##### 单例

- Bean 默认单例

##### 模板方法

- `JdbcTemplate`
- `HibernateTemplate`

##### 包装器

- 切换数据源

##### 观察者

- 事件驱动模型

##### 适配器模式

- AOP 增强或通知
- MVC

### IOC / DI

#### IOC

- ==不用自己创建实例，由 Spring 工厂创建管理==
- Inversion of Control，控制反转
- 对象创建权和依赖管理权交给 Spring 容器
- 程序不再主动创建或查找依赖对象

#### DI

- ==组件之间的依赖关系由容器在运行期间决定==
- Dependency Injection，依赖注入
- IOC 的具体实现方式
- Spring 将依赖对象注入目标对象

#### 常见注入方式

##### 构造器注入

- 通过构造方法传入依赖对象
- 适合==必须==依赖
- 支持 final 字段，便于单元测试
- 推荐使用
- 依赖变更时需要重新创建对象

##### Setter 注入

- 通过 Setter 方法注入依赖
- 适合==可选==依赖
- 依赖可能未被注入
- 可以覆盖已注入的依赖

##### 字段注入

- 直接在成员变量上注入依赖
- 通过==反射==完成注入
- 依赖关系不够明显
- 不支持 final 字段，单元测试不方便
- 不推荐使用
- `@Autowired` 由 Spring 提供，默认按类型注入
- `@Resource` 是 Java 标准规范，默认按名称注入

### Bean

#### 注解

##### `@Component`

- 通用注解，可标注任意类为 Spring 组件

##### `@Repository`

- 持久层组件

##### `@Service`

- 服务层组件

##### `@Controller`

- 控制层组件

#### Bean 的作用域

##### singleton

- ==默认==
- 单例

##### request

- 每个请求产生一个 Bean
- 仅在当前 HTTP request 中生效

##### session

- 每个 session 对应一个 Bean
- 仅在基于 Web 的 Spring `ApplicationContext` 下有效

##### global-session

- 全局 session 作用域
- 仅在基于 portlet 的 Web 应用中有意义

### 生命周期

#### 创建

##### 实例化 Bean

- 根据 BeanDefinition 创建 Bean 对象
- 调用构造方法或工厂方法

##### 设置 Bean 属性

- 注入依赖对象
- 设置普通属性
- 处理 `@Autowired`、`@Resource` 等注入注解

##### Aware 接口回调

- `BeanNameAware`
  - 获取当前 Bean 的名称
- `BeanFactoryAware`
  - 获取 BeanFactory
- `BeanClassLoaderAware`
  - 获取类加载器
- `ApplicationContextAware`
  - 获取 ApplicationContext
- `EnvironmentAware`
  - 获取环境配置
- `ApplicationEventPublisherAware`
  - 获取事件发布器

##### BeanPostProcessor 前置处理

- 执行 `postProcessBeforeInitialization()`
- 可以修改 Bean
- 可以进行初始化前的检查和处理

##### 初始化

###### `@PostConstruct`

- 执行标注了 `@PostConstruct` 的方法
- 常用于初始化资源和校验参数

###### `InitializingBean`

- 调用 `afterPropertiesSet()`

###### 自定义初始化方法

- 调用配置中指定的 `init-method`

##### BeanPostProcessor 后置处理

- 执行 `postProcessAfterInitialization()`
- Spring AOP 通常在这里创建代理对象

##### 放入容器

- 单例 Bean 放入单例缓存池
- 后续通过 Spring 容器获取 Bean

#### 使用

- Bean 正常提供服务
- 默认情况下，单例 Bean 在容器中只有一个实例

#### 销毁

##### 容器关闭

- Spring 容器关闭时，触发单例 Bean 的销毁流程

##### `@PreDestroy`

- 执行标注了 `@PreDestroy` 的方法
- 常用于释放资源

##### `DisposableBean`

- 调用 `destroy()` 方法

##### 自定义销毁方法

- 调用配置中指定的 `destroy-method`
- 常用于关闭连接、线程池、文件流等资源

### 循环依赖问题

#### 场景

- A 依赖 B，B 依赖 A

#### 解决方式

- 通过三级缓存实现

#### 处理流程

##### 创建 Bean A

- Spring 实例化 Bean A
- Bean A 的属性还没有注入完成
- 将 Bean A 的早期引用工厂放入三级缓存

##### 创建 Bean B

- Bean B 依赖 Bean A
- Spring 从一级缓存中找不到 Bean A
- 从二级缓存中也找不到 Bean A
- 从三级缓存中获取 Bean A 的早期引用

##### 暴露 Bean A

- 三级缓存中的工厂生成 Bean A 的早期引用
- 如果存在 AOP，则生成 Bean A 的代理对象
- 将 Bean A 的早期引用放入二级缓存
- 删除三级缓存中的 Bean A 工厂

##### 完成 Bean B

- 将 Bean A 的早期引用注入 Bean B
- Bean B 完成初始化
- 将 Bean B 放入一级缓存

##### 完成 Bean A

- Bean A 完成属性注入和初始化
- 将 Bean A 放入一级缓存
- 删除二级缓存中的早期引用

### 事务传播规则

#### `PROPAGATION_REQUIRED`

- 支持当前事务
- 如果当前存在事务，则加入当前事务
- 如果当前不存在事务，则新建事务
- Spring 默认的传播行为
- 最常用

#### `PROPAGATION_SUPPORTS`

- 支持当前事务
- 如果当前存在事务，则加入当前事务
- 如果当前不存在事务，则以非事务方式执行

#### `PROPAGATION_MANDATORY`

- 强制要求当前存在事务
- 如果当前存在事务，则加入当前事务
- 如果当前不存在事务，则抛出异常

#### `PROPAGATION_REQUIRES_NEW`

- 总是创建一个新事务
- 如果当前存在事务，则挂起当前事务
- 新事务执行完成后，恢复原来的事务
- 新事务与原事务相互独立

#### `PROPAGATION_NOT_SUPPORTED`

- 不支持事务
- 如果当前存在事务，则挂起当前事务
- 当前方法以非事务方式执行
- 方法执行完成后，恢复原来的事务

#### `PROPAGATION_NEVER`

- 不允许在事务中执行
- 如果当前存在事务，则抛出异常
- 如果当前不存在事务，则正常执行

#### `PROPAGATION_NESTED`

- 如果当前存在事务，则创建保存点
- 嵌套事务回滚时，只回滚到保存点
- 不会直接回滚外层事务
- 如果外层事务最终回滚，嵌套事务也会回滚
- 如果当前不存在事务，则创建新事务
- 通常依赖数据库保存点实现

### MVC 工作原理

- 客户端发送请求，请求到达 `DispatcherServlet`
- `DispatcherServlet` 调用 `HandlerMapping`
- `HandlerMapping` 根据请求路径找到对应的 `Handler`
- `DispatcherServlet` 调用 `HandlerAdapter`
- `HandlerAdapter` 执行对应的 Controller 方法
- Controller 处理业务逻辑并返回 `ModelAndView`
- `DispatcherServlet` 调用 `ViewResolver`
- `ViewResolver` 根据视图名称查找实际的 `View`
- `View` 使用 `Model` 中的数据进行渲染
- `DispatcherServlet` 将渲染后的结果返回给客户端

### Spring Boot Starter

- 启动时在 Starter 包中寻找 `resources/META-INF/spring.factories` 文件
- 根据 `spring.factories` 加载对应的 AutoConfigure 类
- 根据 `@Conditional` 注解的条件，自动配置并将 Bean 注入 Spring Context

## 并发

### 进程和线程

#### 进程

##### 系统分配资源的最小单位

##### 进程崩溃不影响其他进程

##### 不共享内存，切换开销大

#### 线程

##### CPU 调度和执行的最小单位

##### 线程崩溃直接影响进程

##### 共享内存，切换开销小

#### 创建线程的方式

##### 实现 Runnable 接口

- 可以继承类
- 重写 run 方法，无返回值
- 不能抛出异常

##### 实现 Callable 接口

- 可以继承类
- 重写 call 方法，有返回值
- 可以抛出异常

##### 继承 Thread 类

- 不能继承类

#### 状态

##### 新建

- 创建，new Thread

##### 就绪

- 等待 CPU 执行

##### 运行

- 执行

##### 阻塞

- 等待阻塞 wait()
- 同步阻塞 synchronized
- 其他阻塞 sleep()

##### 死亡

- 执行完毕
- 异常

#### 死锁

##### 互斥

- 资源只能被一个线程占用
- 无法破坏，多线程必须共享资源

##### 请求与保持

- 获取不到资源时，不释放当前资源
- 破坏：一次申请全部资源

##### 不剥夺

- 不能剥夺其他线程资源
- 破坏：拿不到资源，释放自己的

##### 循环等待

- 循环等待资源
- 破坏：锁排序，指定获取锁的顺序，ReentrantLock.tryLock()

### 线程池

#### execute 和 submit 的区别

- `execute`：提交不需要返回的任务，无法判断是否成功
- `submit`：提交需要返回值的任务，返回 `Future` 对象

#### 核心参数

##### 核心线程大小 `corePoolSize`

- 线程池运行，核心线程不停止

##### 最大线程数量 `maximumPoolSize`

- 非核心 + 核心
- CPU 密集型：线程数约等于 CPU 核心数
- IO 密集型：线程数约等于 `CPU 核心数 / (1 - 阻塞系数)`

##### 非核心线程的心跳时间 `keepAliveTime`

##### 阻塞队列 `workQueue`

###### newCachedThreadPool

- `SynchronousQueue`：直接转发队列，无限线程
- 线程数量无限制
- 需要控制数量避免 OOM

###### newFixedThreadPool

- `LinkedBlockingQueue`：无界队列
- 提交任务创建线程
- 达到最大数量，任务存入队列

###### newSingleThreadExecutor

- `LinkedBlockingQueue`：无界队列
- 单线程的 `Executor`
- FIFO

###### newScheduleThreadPool

- `DelayedWorkQueue`：延时队列
- 定长，核心线程数量固定
- 定时
- 周期

##### 饱和策略 `defaultHandler`

- `AbortPolicy`：丢弃并直接报错，默认策略
- `DiscardPolicy`：丢弃但不报错
- `DiscardOldestPolicy`：丢弃队列首部任务
- `CallerRunsPolicy`：在线程池外直接调用 `run` 执行

##### 线程工厂 `threadFactory`

#### 怎么复用线程的

##### ThreadPoolExecutor

###### 内置对象 Worker

- while 死循环从对象中拉取数据
- 置换 `Worker` 中的 `Runnable` 对象
- 运行 `run` 方法

#### Executor 和 Executors 的区别

##### Executor

- 接口，ExecutorService 继承并扩展，获取任务状态 / 返回值

##### Executors

- 工具类，创建线程池

### ThreadLocal

#### 每个线程保存变量副本，每次只修改自己的副本

#### 应用

- 数据库连接上下文
- 用户上下文
- 会话

#### 原理

- 每个线程保存一个 ThreadLocalMap（Entry 数组）
- key 为 ThreadLocal 对象，value 为变量值

#### 内存泄漏

##### Entry 数组的 key 是弱引用

##### key 回收后 value 仍可能存在

##### 调用 remove() 方法及时回收

### ReentrantLock

#### 可重入锁 / 可公平锁

#### 依赖 AQS 实现，维护一个阻塞队列

#### 缺点：读写互斥时可能阻塞只读线程

##### ReadWriteLock 接口

##### ReentrantReadWriteLock 实现

- 读共享，写独占
- 原理：把 AQS 的 state 拆分为读计数 / 写计数

### AbstractQueuedSynchronizer

#### 结构

##### 一个 CAS 修改的 volatile int 类型的 state

- park
- unpark

##### 一个 FIFO ==双向== 同步队列

- 线程异常时，方便移除
- 忙等时挂起（前驱已经在等待了，挂起当前）
- 支持从队尾开始判断是否在队列中
- 减少并发，源码很多从==队尾==遍历，猜测是减少队头的并发

##### Condition 条件队列

- 开发者显示调用 Condition.signal() 放入同步队尾，而不是直接执行

##### Node

###### 被封装的线程

###### 前驱 / 后继

###### 共享模式 tryAcquireShared

- Semaphore：计数信号量，控制并发许可数
- CountDownLatch：倒计时门闩，等待多个线程完成
- ReadLock

###### 独占模式 tryAcquire

- ReentrantLock

### Compare And Swap

#### 保证原子性

##### 基于硬件的原子指令 cmpxchg

###### 锁定总线

###### CPU 禁止中断

###### 硬件实现

### CompletableFuture

#### 概念

##### Future

- 未来才会有结果，现在返回 Future

##### result

- Future 里存放的结果

##### CompletableFuture

- 相比 Future，get 结果后还能执行任务

##### Completion

- 后续任务

##### Stack

- 存放后续任务

#### 底层实现

##### result

- volatile
- null 未完成
- 正常值
- AltResult 包装异常 / null / 取消

##### stack

- Completion 依赖栈
- thenApply / thenAccept 注册回调
- 上游完成后触发下游

##### CAS

- 原子设置 result
- 只能完成一次
- 完成后 postComplete

##### Executor

- Async 提交线程池
- 默认 ForkJoinPool.commonPool
- 可自定义 Executor
- 非 Async 复用完成线程

### synchronized

#### 锁类型

##### 偏向锁 01

- 锁对象的 Mark Word 中存储 Thread ID

##### 轻量级锁 00

- 线程在自己的栈帧中创建 Lock Record
- 使用 CAS 将对象 Mark Word 替换为指向 Lock Record 的指针
- 如果是自己，再压一个空锁
- 不是自己，CAS 等待
- 举例：线程 A 有栈帧 A，想要获取 Object 的锁，就 CAS 把 Object 的 Mark Word 移到自己的栈帧中

##### 重量级锁 10

- Mark Word 存的是指向 ObjectMonitor 的指针
- ObjectMonitor（owner, WaitSet, EntryList）

##### 11 为 GC 标记，不参与 synchronized 锁

#### 锁升级

##### 偏向锁 --> 轻量级锁

###### 线程不一致升级

##### 轻量级锁 --> 重量级锁

###### 自旋获取，超次数升级

#### 锁降级

##### 锁状态检查

- STW 时，检查所有 Monitor

##### 确定降级对象

- Monitor 计数器
- 持有者

##### 降级操作

- 恢复锁对象的 Mark Word 对象头
- 重置 ObjectMonitor

#### 优化

##### 锁升级

##### 锁消除

##### 锁粗化

##### 自旋锁 / 自适应自旋锁

- 自旋锁：轻量级锁没获取到时，不让出 CPU，进行循环等待
- 自适应自旋锁，自旋次数由上一次的类似情况决定

#### 用法

##### 普通方法

###### 当前实例

##### 静态方法

###### 当前类

##### 代码块

###### 进入前要获取指定的锁

#### 特性

- 原子性
- 可见性
- 有序性

### ForkJoin - 分而治之

#### 概念

- compute：当前任务具体怎么执行
- fork：把子任务丢出去异步执行
- join：等待子任务执行完并拿结果

#### 适用场景

##### 大任务分解为小任务

##### 计算密集型

##### 异构任务并行处理

##### 递归算法并行化

##### 数据聚合任务

#### 优势

##### 可分解

- 天然适合快排
- 左 fork，右 compute，左 join

##### 并行减少处理时间

##### 可伸缩

- 默认根据CPU核心数初始化
- 可以手动指定
- ==managedBlock== 阻塞时会考虑是否增加线程

### 常见对比

#### run / start、wait / sleep、notify / notifyAll区别?

##### start / run

###### start

- 线程的入口
- 启动后会执行 run 方法

###### run

- 普通方法调用，不会启动新线程
- 由当前线程同步执行

##### wait / sleep

###### sleep

- 任何地方使用
- 不会释放锁
- 针对的是==线程==，所以定义在==Thread==中

###### wait

- 同步方法 / 同步代码块中使用
- 释放锁
- 针对的是==对象==，所以定义在==Object==中

##### notify / notifyAll

###### 针对的都是对象，定义在==Object==中

###### notify

- 唤醒一个

###### notifyAll

- 唤醒所有

## JVM

### 内存结构

#### 程序计数器

##### 线程私有

##### 存储

- 当前正在执行的线程指令地址

#### 虚拟机栈

##### 线程私有

##### 存储

- 局部变量表
- 操作数
- 动态链接
- 方法返回

#### 本地方法栈

##### 线程私有

##### 存储

- Native 方法信息
- 通过动态链接调用

#### 堆

##### 共享

##### 存储

- 对象的实例或数组
- 垃圾回收的区域
- 运行时常量池（1.8 后）

#### 方法区 1.8 以前

##### 共享

##### 存储

- 已经被加载的类信息、常量、静态变量

#### 元数据 1.8 后

##### 共享

##### 存储

- 加载的类信息

### 内存模型 JMM

#### Happens-Before

- 线程间的可见性

#### 规则

- 单线程规则
- 监视器锁定规则
- volatile 变量规则
- 线程 start 规则
- 线程 join 规则
- 传递性

### 堆和栈区别

#### 堆

- 程序员手动申请
- 遍历空闲内存地址链表
- 效率低
- 不连续内存
- 程序员决定存储内容

#### 栈

- 系统分配
- 系统为程序分配内存
- 效率高
- 连续内存
- 存储局部变量表、操作数、动态链接、方法返回等

### 栈溢出与 OOM

#### 栈溢出 StackOverflowError

- 线程请求的深度超过了虚拟机允许的最大深度
- 无限递归
- `-Xss` 调整 JVM 栈大小

#### 内存溢出 OutOfMemoryError

##### 可能发生 OOM 的区域

###### Java Heap

- 对象数量过多
- 对象长期被引用，无法被 GC 回收
- 例如无限创建对象、缓存没有上限
- 可能出现 `Java heap space`

###### 方法区 / Metaspace

- 动态生成大量类
- 频繁创建 ClassLoader
- 代理类、动态字节码类没有释放
- 可能出现 `Metaspace`

###### 运行时常量池

- 属于方法区概念
- 动态创建大量字符串或常量可能造成内存压力
- 字符串常量池在 Java 7 以后位于 Java Heap

###### Java 虚拟机栈和本地方法栈

- 无限创建平台线程
- 每个线程都需要线程栈和 Native 内存
- 可能出现：
  - `unable to create native thread`
  - `OutOfMemoryError`
- 单个线程递归过深通常出现 `StackOverflowError`

###### 直接内存 Direct Memory

- NIO、Netty 等可能使用直接内存
- 不属于 Java Heap
- 直接内存超限可能出现 `OutOfMemoryError: Direct buffer memory`

##### 不容易出现 OOM 的区域

###### 程序计数器

- 保存当前线程下一条要执行的字节码指令地址
- 占用空间很小
- JVM 规范没有规定该区域抛出 OOM

#### 排查 OOM 方法

- 增加参数，OOM 发生时自动 dump 堆内存信息到指定目录
- `jstat` 查看 JVM 内存和 GC 情况，判断发生区域
- MAT 工具载入 dump 文件，分析大对象

### 常量池

#### Class 文件常量池

- 编译后存储在 `.class` 文件中
- 保存字面量和符号引用
- 例如：
  - 类名
  - 方法名
  - 字段名
  - 字符串字面量
  - 方法和字段的符号引用

#### 运行时常量池

- 类加载时由 Class 文件常量池转换而来
- 属于方法区概念
- 保存：
  - 字面量
  - 类和接口引用
  - 字段引用
  - 方法引用
  - 动态常量
- Java 8 中类元数据主要存储在 Metaspace

#### 字符串常量池

- 用于复用字符串对象
- `String.intern()` 可以将字符串放入或获取字符串池中的对象
- Java 6 及以前位于 PermGen
- Java 7 以后位于 Java Heap

#### 包装类对象缓存

- 由包装类维护对象缓存
- 不是严格意义上的 JVM 常量池
- 常见缓存：
  - `Integer`：通常缓存 `-128` 到 `127`
  - `Long`：通常缓存 `-128` 到 `127`
  - `Short`：通常缓存 `-128` 到 `127`
  - `Byte`：通常缓存全部范围
  - `Character`：通常缓存常用字符
  - `Boolean`：缓存 `true` 和 `false`

### 垃圾回收

#### 判断存活

##### 引用计数法

- 引用计数器，每次有引用 +1，引用失效 -1
- 缺点：无法解决循环引用问题

##### 可达性分析

###### GC Roots

- 虚拟机栈中正在使用的局部变量和参数引用
- 活跃线程中的对象引用
- 方法区中类的静态字段引用
- 方法区中常量引用的对象
- 本地方法栈中的 JNI 引用
- 被 synchronized 锁持有的对象
- JVM 内部使用的对象引用

###### 流程

- 从 GC Roots 开始遍历对象引用关系
- 能够从 GC Roots 到达的对象视为存活对象
- 无法到达的对象视为不可达对象
- 不可达对象不一定立即回收，还需要经过最终处理和下一轮 GC 确认
- GC 使用三色标记法辅助遍历：
  - 白色：尚未发现
  - 灰色：已发现，但引用对象还没有扫描完成
  - 黑色：自身及其引用对象都已扫描完成
- 标记完成后，剩余白色对象可以作为垃圾回收

##### 引用类型

###### 强引用

- 默认的引用类型
- 不会被回收

###### 软引用

- `SoftReference` 创建
- 内存不足时会被回收

###### 弱引用

- `WeakReference` 创建
- 无论内存是否充足都会被回收

###### 虚引用

- `PhantomReference` 创建
- 主要用于跟踪对象被垃圾回收的活动

##### 垃圾回收算法

###### 标记清除法

- 遍历内存，标记存活对象和垃圾对象
- 再次遍历，回收垃圾对象
- 缺点：空间利用率、效率都较差。连续大空间不足时会触发 GC

###### 标记整理法

- 遍历内存，标记存活对象和垃圾对象
- 将==存活==对象向一侧移动，回收边界以外对象
- 特点：适用于存活对象多，垃圾少

###### 复制算法

- 内存分为两块
- 每次使用一块
- 满了就把存活对象移到另一块
- 缺点：内存利用率低

###### 分代收集法

- 新生代（大量对象死去，少量存活），采用==复制法==
- 老年代（存活率高）采用==标记整理==或==标记清除==

##### 垃圾回收器

###### Serial

- 单线程
- 必须 Stop The World
- 复制算法

###### ParNew

- Serial 的多线程版本
- 必须 Stop The World
- 复制算法

###### Parallel Scavenge

- 新生代收集器
- 动态设置参数，提供最优停顿时间/吞吐量
- 复制算法

###### Serial Old

- Serial 的老年代版本
- 单线程
- 标记整理算法

###### Parallel Old

- Parallel Scavenge 的老年代版本
- 多线程
- 标记整理算法

###### CMS

- 最短回收停顿时间
- 初始标记（STW）-> 并发标记 -> 重新标记 -> 并发清除
- 大量内存碎片 / 并发抢占 CPU 资源 / 并发失败会 STW
- 标记清除算法

###### G1

- 初始标记（STW）-> 并发标记 -> 最终标记（STW）-> 筛选回收（STW）
- 将堆分为大小相等的多个 Region，回收价值大的区域
- 整体基于标记整理，局部基于复制

### 类加载

#### 类加载过程

##### 加载

- 通过全限定类名加载类的==二进制==流
- 二进制流转为==运行时==数据结构
- 在==堆==中生成 `Class` 对象

##### 验证

- 验证 `Class` 对象的==字节流==信息符合 JVM 要求

##### 准备

- 为 `Class` 对象的==静态变量==分配内存，初始化初始值

##### 解析

- 惰性符号引用转换成直接引用

##### 初始化

- 执行 Java 代码和构造器

#### 类加载器

##### 启动类加载器

- Java 核心类库
- 无法被 Java 程序直接引用

##### 扩展类加载器

- Java 扩展库
- 提供扩展库目录，从中查找并加载 Java 类

##### 系统类加载器

- 根据 Java 的类路径加载类
- 一般都是由此加载器加载

##### 自定义类加载器

- Java 语言实现
- 继承 `ClassLoader`

#### 双亲委派

##### 什么是双亲委派模型

- 收到类加载请求
- 委派给父类加载器
- 父类查找不到
- 子类加载器加载

##### 目的

- 保证类的唯一性
- 比如防止用户定义 `java.lang.String`

##### 打破双亲委派

- 自定义类加载器
- 继承 `ClassLoader` 类
- 重写 `loadClass` 和 `findClass` 方法

##### 打破双亲委派的例子

###### Tomcat

- WebApp 各自加载自己的类
- 核心 API 仍然交由父类加载器加载

###### JDK 9

- 扩展类加载器由 Platform 加载器取代
- 委派给父加载器前，先判断是否能归属到某个模块中
- 添加模块化特性

#### JVM 调优

##### `jps`

- 查看虚拟机进程

##### `jstat`

- 运行时状态信息
- 类装载、内存、垃圾回收、JIT

##### `jmap`

- 生成 dump 文件
- 查询 finalize 执行队列
- 堆和永久代详细信息

##### `jhat`

- 分析 `jmap` 生成的 dump
- 内置 HTTP / HTML 服务器，在浏览器查看

##### `jstack`

- 生成线程快照
- 查看线程调用堆栈

## MySQL

### 基础

#### 数据库三范式

##### 第一范式

- 每一列都是不可分割的原子项

##### 第二范式

- 每一列的属性都完全依赖于主属性
- 根据主属性能够完全确定

##### 第三范式

- 任何非主属性都不依赖于其他非主属性

#### 存储引擎

##### InnoDB

- 支持事务
- 支持外键
- 聚簇索引
- 不支持全文索引
- 不保存表的行数
- 支持行级锁，表级锁

##### MyISAM

- 不支持事务
- 不支持外键
- 非聚簇索引
- 全文索引
- 保存表的行数
- 表级锁

#### 键

##### 超键

###### 主键

- 唯一完整标识

###### 候选键

- 最小超键，没有冗余

##### 外键

- 另外一个表的主键

#### IN / EXISTS

##### IN

- 外表和内表做 Hash 连接
- ==子表大==的时候用
- NOT IN 没有用索引
- 新版本将 NOT IN / NOT EXISTS 优化成 Anti Join，可能使用索引

##### EXISTS

- 对外表做 Loop 循环
- ==子表小==的时候用
- NOT EXISTS 用索引了

#### MySQL 执行查询过程

- 连接：TCP 请求到 MySQL 服务器
- 鉴权：权限验证，连接资源分配
- 缓存：查询完全相同的 SQL 缓存
- 分析：预处理器进行语法分析
- 优化：是否使用索引，生成执行计划
- 执行：结果保存到结果集，逐步同步缓存，返回给客户端

### 索引

#### 索引分类

##### 应用层次

###### 普通索引

- 只包含单个列

###### 唯一索引

- 索引值唯一
- 可以有多个 NULL

###### 复合索引

- 多个列组成

##### 聚簇 / 非聚簇

###### 聚簇索引

- 索引和值存在一起
- 一般使用主键作为聚簇索引

###### 非聚簇索引

- 索引和地址放在一起
- 需要二次查询

##### 存储结构

###### Hash 索引

- 为所有的索引列建立哈希码
- 精确匹配索引所有列的查询才生效

###### B-Tree

- 数据直接在树的各个节点上

###### B+Tree

- 数据在叶子节点上
- 增加顺序访问指针
- 叶子节点为聚簇索引，非叶子节点为非聚簇索引

#### 最左前缀

- WHERE 中最频繁的一列放在最左边
- 一直向右匹配直到范围查询
- = 和 IN 可以==乱序==

#### 索引下推

- 5.6 版本引入，默认开启
- 减少回表次数
- 只对二级索引有效
- 没有使用会把数据返回到服务端进行过滤
- 使用后会在==服务端==对==索引==进行过滤

#### EXPLAIN

##### id

- 每个 SELECT 关键字映射成一个 id
- 如果两个有相同的 id，说明把子查询优化成连接查询了

##### select_type

- SIMPLE
- PRIMARY
- SUBQUERY
- DEPENDENT
- UNION

##### table

##### ==type==

- system
- const
- eq_ref
- ref
- range
- index
- all

##### possible_keys

- 可能用到的 key

##### key

- 实际用到的 key

##### filtered

- 查询器预测满足下一次查询条件的百分比

##### ==rows==

- 估算需要扫描的行数

##### extra

- 额外信息
- 如 Using where、Start temporary、End temporary、Using temporary 等

#### 创建索引需要注意

##### 非空

- 很难进行查询优化，难以统计
- 使用 0、空串占位

##### 离散值大

- count 查看字段差异值

##### 索引字段越小越好

#### 建索引原则

- 最左前缀匹配
- = 和 IN 可以乱序
- 区分度高
- 索引列不参与计算
- 尽可能扩展，不要新建

#### 索引失效

- != 或者 <>
- 类型不一致
- 函数
- 运算符
- OR
- 模糊查询
- NOT EXISTS

### 事务

#### 四个特征

##### 原子性

- 所有操作要么都做，要么都不做

##### 一致性

- 从一个状态到另一个状态

##### 隔离性

- 一个事务不能影响另一个事务

##### 持久性

- 事务提交，数据库影响是永久的

#### 隔离级别

##### 读未提交 RU

###### 会读到其他未提交事务修改的内容

###### ==脏读==

- 读到了其他事务未提交的数据

##### 读已提交 RC

###### 会读到其他事务提交修改的内容

###### ==不可重复读==

- 其他事务多次提交，导致每次读取到的不一样

###### 每条==SQL==创建一个 Read View

###### 大多数数据库默认级别

##### 可重复读 RR

###### 同一事务处理过程中读到的是一致的

###### ==幻读==

- 修改后某一行没有修改过来，像幻觉一样
- 侧重于插入 / 删除

###### 每个==事务==创建一个 Read View

###### MySQL 默认隔离级别

##### 可串行化 Serializable

###### 强制事务排序

###### 加共享读锁

###### 分布式事务

#### MVCC

##### 流程

- 执行 `UPDATE balance = 200`
- 对记录加排他锁
- 将数据页从磁盘加载到 Buffer Pool
- 将旧版本 `balance = 100` 写入 Buffer Pool 中的 Undo Page
- 修改 Buffer Pool 中当前数据页的记录为 `balance = 200`
- 当前记录通过回滚指针关联 Undo Log
- 将本次修改写入 Redo Log Buffer
- 其他事务在 RC/RR 的普通快照读中，根据 Read View 判断可见性：
  - 看不到未提交的 `200`
  - 沿 Undo Log 读取旧版本 `100`
- 事务提交时，将 Redo Log Buffer 刷入磁盘上的 Redo Log
- Redo Log 持久化成功后，事务提交具备崩溃恢复能力
- Buffer Pool 中被修改的数据页和 Undo Page 标记为脏页
- 脏页之后由后台线程异步刷入数据文件和 Undo Tablespace

#### binlog

##### statement

###### 基于 SQL 语句的模式

##### row

###### 基于行的模式，记录的是行的变化，很安全

##### mixed

###### 根据语句来选用是 statement 还是 row 模式

### 主从同步 / 读写分离

#### Binary log / Relay log

##### Binary log

- 主数据库的二进制日志
- update -> binlog cache -> 准备提交 -> cache 刷盘 -> 提交完成

##### Relay log

- 从服务器的中继日志

##### 格式

- `STATEMENT`：记录 SQL 语句
- `ROW`：记录具体行的前后变化
- `MIXED`：由 MySQL 根据情况选择 `STATEMENT` 或 `ROW`

#### 主从同步过程

##### 主库执行事务

- 主库将变更记录为 Binlog Event
- 事务提交时，将事务事件写入 Binlog

##### 从库接收日志

- 从库 I/O 线程连接主库
- 拉取主库 Binlog Event
- 将事件写入从库 Relay Log

##### 从库应用日志

- 从库 Applier 线程读取 Relay Log
- 解析 Binlog Event
- 根据 `STATEMENT` 或 `ROW` 格式执行变更
- 更新已执行位置或 GTID

## Redis

### 优缺点

#### 优点

- 读写性能高
- 支持持久化
- 支持事务
- 数据结构丰富
- 支持主从复制
- 丰富特性

#### 缺点

- 容量受物理内存限制，不支持海量数据
- 主机宕机会有部分数据未能及时同步
- 切换 IP 引入数据不一致问题

### 为什么这么快

#### 内存存储

- 没有磁盘 IO 开销

#### 单线程实现

- 6.0 以前
- 一个线程处理全部网络请求
- 没有线程切换开销

#### 非阻塞 IO

- IO 多路复用
- epoll 作为 IO 多路复用实现
- 事件处理模型，将 epoll 中的连接、读写、关闭转换成事件

#### 优化的数据结构

#### 底层模型不同

- 构建了自己的 VM 缓存机制，冷数据交换到磁盘中

### 常用场景

#### 缓存

#### 排行榜

- 有序集合数据结构

#### 计数器

- `incr` 命令
- 内存操作，性能好

#### 分布式会话

- Session 不再由容器管理，由 Session 服务以及内存数据库管理

#### 分布式锁

#### 社交网络

- 点赞、关注、共同好友
- Set 集合

#### 最新列表

- Redis 列表结构
- 列表头指定关键字和数量，指定列表永远为 N 个 ID，不需要查最新的列表

### 数据类型

#### String

- 底层实现
  - `int`：整数值
  - `embstr`：短字符串
  - `raw`：较长字符串
- 缓存
- 常规计数

#### Hash

- 底层实现：
  - 数据量较小时：`listpack`
  - 数据量较大时：`hashtable`
- String 类型的 field 和 value 的映射表

#### Set

- 底层实现：
  - 元素都是整数且数量较少：`intset`
  - 其他情况：`hashtable`
- 提供交集、并集
- 共同好友、共同关注

#### List

- 底层实现：
  - 底层使用 `quicklist`
  - 旧版本中常见 `linkedlist + ziplist`
- FIFO 双向链表
- 最新回复

#### SortedSet

- 底层实现：
  - 数据量较小时：`listpack`
  - 数据量较大时：`hashtable + skiplist`（查询 + 排序）
- 优先级排序，维护 score
- 排行榜、权重消息队列

#### Bitmap

- bit 位数组，存 0 / 1
- 商品有没有

#### Hyperloglog

- 统计基数，输入元素数量 / 体积非常大时计算基数所需空间固定
- 不重复访客

#### Geospatial

- 存储地理位置信息
- 附近的人

### 持久化机制

#### RDB

- 快照直接读到内存
- 大规模数据恢复，完整性和一致性要求不高
- 宕机丢失最近一次快照

#### AOF

- 记录写指令
- 超过阈值压缩成 AOF
- 效率较低
- 不同步 / 每秒 / 每次修改三种同步策略

### 过期策略

#### 定时删除

- 优点：到期删除
- 缺点：CPU 不友好

#### 惰性删除

- 优点：CPU 友好
- 缺点：占用内存

#### 定期删除

- 优点：CPU、内存相对友好
- 缺点：存在过期 key

### 内存淘汰策略

#### volatile-lru

- 设置过期时间 key 中最近最少使用

#### allkeys-lru

- 全部 key 中最近最少使用

#### volatile-random

- 设置过期时间 key 中随机

#### allkeys-random

- 全部 key 中随机

#### volatile-ttl

- 设置过期时间中将要过期

#### no-eviction

- 禁止驱逐数据

#### volatile-lfu

- 设置过期时间 key 中最不经常使用

#### allkeys-lfu

- 全部 key 中最不经常使用

### 缓存异常

#### 缓存不一致

##### 延时双删

- 先删除缓存
- 写数据库
- 休眠 X 秒，再删除缓存
- 注意：延迟时间应覆盖查询和回填缓存的最长耗时
- 注意：只能降低概率

##### 更新与读取操作异步串行化

- 异步串行化，同一数据请求发送到同一队列
- 读操作去重

##### 次级方案

- 先写数据库
- 删除缓存
- 删除缓存消息队列兜底

#### 缓存雪崩

##### 概念

- 大规模 key 失效，请求打到数据库

##### 解决方案

###### 事前

- 随机抖动，均匀过期
- 分级缓存，每级过期时间不同
- 热 key 永不过期
- Redis 高可用

#### 缓存击穿

##### 概念

- 热点 key 失效，请求打到数据库

##### 解决方案

- 热点 key 永不过期
- 限制可以打到数据库上的请求数量

#### 缓存穿透

##### 概念

- 缓存不存在，直接访问数据库

##### 解决方案

- 缓存无效 key
- 布隆过滤器

### 事务

#### 三个阶段

- `multi` 开启事务
- 大量指令入队
- `exec` 执行命令
- `discard` 取消事务
- `watch` 监视一个或多个 key，如果事务执行前 key 被改动，事务将被打断

#### 其他实现

##### Lua 脚本

- 异常后剩下的命令会继续执行完

### 主从 / 哨兵 / 集群

#### Redis 单副本

##### 架构

- 单个 Redis 节点部署

##### 优点

- 部署方便
- 高性价比
- 高性能

##### 缺点

- 不保证数据可靠性
- 重启后数据丢失
- 性能受 CPU 限制

#### 多副本主从

##### 架构

- 主从实例部署在不同的服务器上
- 提供持久化和备份策略
- 同时提供服务 / 读写分离

##### 解决了什么问题

- 数据可靠
- 扩展读能力

##### 优点

- 主库出现问题时支持主从切换
- 持久化功能和备份策略
- 读写分离策略

##### 缺点

- 需要手动切换主从节点，业务方变更配置
- 主库写能力和存储能力受到单机限制

#### Redis Sentinel

##### 架构

###### Redis Sentinel 集群

- Sentinel 节点组成的分布式集群
- 奇数个

###### Redis 数据集群

##### 解决了什么问题

- 自动切换主从

##### 优点

- 集群部署简单
- 主从切换容易

##### 缺点

- Slave 不提供服务
- 读写分离复杂

#### Redis Cluster

##### 架构

- 虚拟槽分区，16384 个槽（0-16383）
- 最少 3 主 3 从
- 主节点提供读写操作
- 从节点作为备用，可通过配置提供只读

##### 解决了什么问题

- 单机存储性能问题

##### 优点

- 无中心架构
- 可扩展
- 高可用

##### 缺点

- 多 key 操作受限制，需要 hash 放到同分区
- 数据通过异步复制，不保证一致性
- 热 key 分布不当导致集中在某个分片上
- 客户端和运维复杂

#### 主从同步原理

##### 复制连接建立

- Replica 启动后连接 Primary
- Replica 向 Primary 发送 `PSYNC` 请求
- Primary 根据 Replica 提供的复制 ID 和 offset 判断同步方式

##### 全量同步 Full Resynchronization

- Replica 第一次连接 Primary，或无法进行部分同步时，执行全量同步
- Primary 生成 RDB 快照
- Primary 在生成 RDB 期间，将新的写命令写入复制缓冲区
- Primary 将 RDB 发送给 Replica
- Replica 加载 RDB，建立基础数据
- Primary 再将复制缓冲区中的增量命令发送给 Replica
- Replica 执行增量命令，追上 Primary
- 全量同步期间，Primary 仍然可以继续处理客户端请求

##### 部分同步 Partial Resynchronization

- Replica 网络短暂断开后重新连接
- Replica 携带复制 ID 和 offset 发送 `PSYNC`
- Primary 检查复制积压缓冲区是否仍保留缺失数据
- 如果存在缺失数据：
  - Primary 只发送断开期间的增量命令
  - Replica 从上次 offset 继续同步
- 如果缺失数据已经被覆盖：
  - 无法部分同步
  - 转为全量同步

##### 复制积压缓冲区

- Primary 在内存中维护复制积压缓冲区
- 用于保存最近传播过的写命令
- Replica 记录自己的复制 offset
- Primary 根据 Replica 的 offset 判断需要补发哪些命令
- 缓冲区大小由 `repl-backlog-size` 配置

### 分布式锁

#### SETNX + EXPIRE

##### 存在的问题

- A 没结束，锁超时释放，B 获取锁，A 结束，解除 B 锁

##### 解决方式

- 添加守护线程自动续期
  - 添加线程标识信息

#### RedLock

##### 特性

- 互斥访问
- 避免死锁
- 高可用

##### 架构

- N 个独立的 Redis Master
- 通常是 5 个节点
- 不依赖主从复制
- 不依赖 Sentinel 或 Redis Cluster
- 获取锁需要成功写入多数节点

##### 获取锁流程

- 生成全局唯一的锁 Token
- 向多个节点执行加锁
- 统计成功数量和加锁总耗时

##### 原理

## 消息队列

### 总结

#### Kafka

- 偏事件流 / 日志流，核心是 Partition Log、offset、长期保留、回放和大数据生态。

#### RocketMQ

- 偏业务消息，核心是事务消息、延迟消息、顺序消息、重试、死信和消息轨迹。

#### RabbitMQ

- 偏灵活路由和任务队列，核心是 Exchange、Binding、RoutingKey、Queue、ACK / Confirm。

### Kafka

#### 一句话概括

- 分布式提交日志

#### 架构

##### Producer：ack = 0 / 1 / -1

##### Broker：

- Topic 是逻辑分类，Partition 是 Topic 的分片，Partition 的 Leader / Follower 副本分布在不同 Broker 上
- ISR 旧 Follower 落后条数，短期大量消息会导致 Follower 下线
- ISR 新 Follower 落后条数 + 落后时间

##### Consumer：手动提交偏移量，无法依靠 Kafka 保证幂等；

#### Kafka 元数据管理

##### ZooKeeper 模式

- Broker 注册临时节点
- 临时节点依赖 session
- session 超时后节点删除
- Controller 监听 ZK 感知 Broker 上下线
- 元数据存储在 ZooKeeper

##### KRaft 模式

- Kafka 自管理元数据
- Controller Quorum 基于 Raft
- Controller Leader 由多数派选举
- 元数据变更由多数派提交
- 不再依赖 ZooKeeper

#### 消息模型

##### 发布-订阅模型

- Kafka 基于 Topic 发布消息
- 不同 Consumer Group 可以各自消费同一条消息
- 适合日志采集、事件通知、数据分发等场景

##### 点对点消费语义

- Kafka 底层不是传统 Queue 模型
- 但可以通过 Consumer Group 实现类似点对点效果
- 同一个 Consumer Group 内，一条消息只会被其中一个 Consumer 消费
- 不同 Consumer Group 之间互不影响，各自维护 offset

##### 拉数据

#### 高吞吐量设计

##### 磁盘顺序写

- Partition 追加写：Producer -> Broker -> Partition Log -> Segment File
- 避免随机 IO，顺序写性能高

##### Page Cache

- 写入优先进入操作系统页缓存
- 读取命中 Page Cache 时不需要真实读磁盘
- 依赖操作系统管理缓存和刷盘

##### 零拷贝

- Consumer 拉取消息时减少用户态和内核态之间的数据复制
- 降低 CPU 开销，提高网络发送效率

##### 批量处理

- Producer 批量发送
- Broker 批量追加写入
- Consumer 批量拉取
- 减少网络请求、系统调用和 IO 次数

##### 分区并行

- Topic 拆成多个 Partition
- 不同 Partition 可以分布在不同 Broker
- Producer 可并行写多个 Partition
- Consumer Group 可并行消费多个 Partition
- 分区内有序，分区间并行

##### 存储结构简单

- 追加日志 + offset
- 主要维护 segment、index、time index
- Consumer 消费进度通过 offset 管理

#### 副本同步

##### Leader / Follower

- Partition 有 Leader 和 Follower 副本
- Producer 和 Consumer 主要访问 Leader
- Follower 从 Leader 拉取数据

##### ISR

- ISR 表示和 Leader 保持同步的副本集合
- 落后太多的 Follower 会被移出 ISR

##### 可靠性参数

- acks = all 表示等待 ISR 中副本确认
- min.insync.replicas 表示最少同步副本数
- acks = all + min.insync.replicas 可以提高消息可靠性

#### 顺序性

##### 分区内有序

- Kafka 只保证单个 Partition 内有序
- 不保证 Topic 全局有序
- 保证有序：Partition 中只有一个 Topic / 业务侧排序

##### 如何保证业务顺序

- 需要有序的业务使用相同 key
- 相同 key 会进入同一个 Partition
- 例如同一个 orderId 的创建、支付、发货消息进入同一分区

#### 重平衡

##### 触发条件

- Consumer 数量变化
- Topic 数量变化
- Partition 数量变化

##### 具体步骤

- 暂停消费
- 重新计算
- 通知消费者
- 重新分区
- 恢复消费

#### 事务

- 业务处理后提交 offset + 幂等

### RocketMQ

#### 一句话概括

- 面向业务事件的消息中间件

#### 架构

##### Producer

##### Broker（Topic（独立 Queue，Queue，...），Topic，...）

##### Consumer

##### NameServer：维护 Broker 的元数据

#### 集群

- 单 Master
- 多 Master
- 多 Master，多 Slave

#### 消息模型

- 拉数据
- 基于拉的推数据

#### 可靠性保证

- 生产者选用同步推送，接收回调
- Broker 使用集群部署，且改为同步刷盘
- 消费者 ACK 确认

#### 顺序性

- 同步发送
- 指定 Queue
- 有序消费模式（另一种并发消费模式）

#### 事务

- 提交半消息，保存成功，全消息

#### 消费模式

##### 集群消费

- 默认模式
- 同组 Consumer 分摊消息
- 一条消息同组内只消费一次
- 支持水平扩容
- 适合异步任务和业务处理

##### 广播消费

- 同组每个 Consumer 都消费一遍
- 不做负载均衡分摊
- 适合配置通知、本地缓存刷新
- 不适合扣库存、加积分等业务

#### 延时消息

- Timer
- 时间轮

### RabbitMQ

#### 一句话概括

- 基于 Exchange 的灵活路由消息队列

#### 架构

- Producer
- Broker（Exchange <-- Binding --> Queue）
- Consumer

#### 可靠性保证

##### 持久化交换机

##### 持久化队列

##### 消费者确认

##### 默认生产者不会等待回执

##### Publisher Confirm

- 解决有没有到达 Broker，不保证倒霉到达 Queue，仍有可能丢失

#### 重复消费

- 正常流程：发送 -> 确认 -> 删除
- 消费者：一锁二判三更新

#### 可靠性保证

##### 普通集群模式

- 元数据所有实例共享
- 数据本质分片

##### 镜像模式

- 所有实例一样
- 需同步

#### 消息模型

##### 简单模式

- 同步
- 单生产者 / 单消费者

##### 工作队列模式

- 单生产者 / 多消费者

##### 发布订阅模式

##### 路由模式

##### 主题模式

##### RPC 模式

- 分布式

#### 事务

- 主要是生产者的 AMQP 事务
- 开启事务 -> 提交消息 -> 提交事务 -> 失败回滚
- 通常用 Publisher Confirm 替代
- 到达 Broker 靠确认，到达 Queue 靠开启默认退回

#### 死信队列

- 处理失败
- 过期
- 拒绝
- 无法路由

### ActiveMQ

## Agent 开发

### 常见问题

#### 避免 PDF 中表格被切断

##### 页面解析

##### 判断文本 / 表格

##### 正文按段 / 表格按表

##### 注意事项

- 表格分片按行
- 均保留表头
- 表格转 MD 而不是文本
- 添加元数据
- 分片器添加规则

#### 多模态检索分配权重

##### 固定权重

- 文字说明多 / 图表截图多 / 商品图片检索
- 归一化

##### 按问题类型分配

- 这个图片里的异常点在哪里？ text 0.3 + image 0.7
- 这个参数是什么意思？ text 0.8 + image 0.2
- 按关键词规则判断

##### 两路召回再重排

- 文本向量召回 TopK
- 图片向量召回 TopK
- 合并去重
- 用 reranker / 多模态模型重排

##### 学习型权重最终得分

- text_score
- image_score
- query_type
- 是否包含“图 / 表 / 截图”
- chunk 类型：text / table / image
- 历史命中率

#### 工程和 Prompt 防止大模型幻觉

##### 产生原因

- 预测补全
- 上下文不足
- 上下文腐化
- 检索召回错误
- 没有 reranker
- 没有对工具结果进行校验
- Prompt 约束不足

##### 工程为主，Prompt 为辅

###### RAG 只允许基于检索结果而非常识

###### 返回答案必须带引用，否则标记为推测

- 规定引用范围，不允许生成
- 引用做二次校验

###### 检索质量控制

- TopK 召回 / 多路召回
- reranker 重排
- 相似度阈值
- 主子分片
- 表格 / 图片结构化

###### 上下文完整性

###### 答案后校验

##### Prompt 添加规则，Agent 调用 Prompt 时检查

##### 实用组合

- 混合检索：BM25 + 向量
- reranker 精排
- 相似度阈值
- 主子分片
- 答案引用，无证据不回答
- 结果校验，高风险工具确认

#### 长期画像 / 短期记忆存储

##### 长期画像：跨会话保留

- MySQL / PostgreSQL 结构化画像
- Redis 高频短期缓存

```json
{
  "user_id": "u123",
  "profile": {
    "role": "Java 后端开发",
    "experience_years": 3,
    "language": "zh-CN",
    "goals": ["准备面试", "构建 Java 后端知识图谱"],
    "preferences": ["解释要具体", "先讲原理再讲面试话术"]
  },
  "updated_at": "2026-07-29"
}
```

##### 短期聊天

- message 表
- Redis 会话缓存
- 本地上下文窗口
- 对象存储归档
- (conversation_id, user_id, role, content, created_at, token_count, metadata)

##### 压缩上下文

###### 长期记忆

- 长期目标
- 项目背景
- 反复出现的薄弱点
- 稳定决策

###### 短期记忆

- 最新目标
- 未解决问题
- 已确认结论
- 去除冲突后可转入长期记忆

###### 避免腐化

- 加时间戳，新覆盖旧
- 加置信度
- 可失效
- 避免推测
- 用户纠正优先

#### 大模型生成 JSON 调用工具

##### 最好用填参数形式而非 JSON

##### Prompt 中明确需要 JSON 格式

##### 代码中做转换 / 校验

##### 做幂等校验

##### 缺失信息提示用户补充

#### 检索策略

##### 问题理解

- 判断问题类型：事实、步骤、故障、对比、表格、图片
- 识别关键词：型号、字段、故障码、时间、版本

##### 多路召回

- 向量召回：语义相似
- BM25 / 关键词召回：精确匹配
- 表格 / OCR / 图片召回：结构化和多模态内容
- 多路结果合并去重

##### 过滤

- 权限过滤
- 版本过滤
- 时间过滤
- 文档类型过滤
- 业务域过滤

##### 重排

- Reranker 精排候选 chunk
- 解决语义相似但答非所问
- TopK 召回后取最相关 TopN

##### 上下文组装

- 命中小 chunk，回填 parent chunk
- 表格保留表头和标题
- 段落保留章节路径和相邻上下文
- 注入 citation_id

##### 置信度判断

- 分数低：拒答或追问
- 证据冲突：列出冲突来源
- 证据充分：回答并引用

#### Agent

##### Agent 自主规划工具使用

- 规划受 Skill 影响
- 理解目标
- 判断缺少信息
- 选择工具
- 执行工具
- 观察结果
- 更新计划
- 继续 / 结束

##### 主子 Agent 通信链路设计

###### 主 Agent

- 目标拆解
- 任务分派
- 结果校验
- 最终决策

###### 子 Agent

- 单一任务执行
- 返回结构化结果

###### 调度层

- 通信 / 状态 / 重试 / 日志 / 权限

###### 共享存储

- 保存任务上下文、文件与中间产物、工具结果、任务状态、Agent 通信消息、检索证据和引用
- 按 `task_id`、`tenant_id` 隔离数据
- 大文件只传 `artifact_id`，不直接复制内容
- Redis 保存短期状态，数据库保存任务记录，对象存储保存文件

###### 子任务挂了返回空怎么办？

- 格式化空结果让主任务能够识别
- 主任务采用状态机而不是猜
- 提供重试策略
- 主任务提供降级子任务
- 返回结果透明

###### 子任务返回错误数据怎么办？

- 子任务返回结构化数据
- 结构校验
- 证据校验
- 业务校验
- 一致性校验
- 决策处理

##### 能力复用 / Skill 管理

###### 能力分层

- Skill 流程模板
- Tool 工具调用
- Prompt 角色和约束
- Memory 用户记忆和长期偏好
- Policy 权限和安全

#### Prompt

##### System Prompt（规范）

- 定义角色、边界和安全规则
- 约束回答风格和可用能力
- 优先级高于用户输入

##### Few Shot（如样例 BPMN）

- 给模型提供输入 / 输出示例
- 稳定回答格式
- 降低理解偏差
- 适合分类、抽取、JSON 输出、复杂格式任务

##### Chain of Thought（mini 版本 Skill）

- 引导模型分步分析
- 适合复杂推理、规划、诊断
- 生产环境不一定展示完整推理
- 可改为输出简短理由或结构化步骤

#### RAG

##### 增量文本 RAG

###### 给文档和 chunk 建立稳定 ID（内容 hash）

- doc / chunk ID
- hash

###### 文档变化重新分片做 diff

- hash 相同：不变
- 新 chunk：新增
- 更新 chunk：更新
- 删除已删除的 chunk

###### 查询注意版本一致性

##### 检索召回不准

###### 检查分块

- chunk 是否能表达完整语义
- 表格是否切断
- chunk 是否过长或过短
- chunk 是否保存了元数据
- 标题和正文是否一起保存了

###### 检查 Embedding 和查询改写

- 问题和检索是否使用相同 Embedding 模型
- 中英文、数字、专业词汇是否支持良好
- 是否需要问题改写
- 是否需要补充同义词和关键词

###### 加混合检索（解决语义和关键词匹配问题）

- 向量检索
- BM25 关键词检索
- 合并去重

###### 优化 Reranker

- Reranker 模型
- TopK 候选数量
- TopN 截断数量
- 问题和 Chunk 拼接格式
- 分数预支

##### 优化推荐顺序

- 建立评测集
- 检测正确 Chunk 是否进入 TopK
- 优化分块
- 优化 Embedding 和查询改写
- 增加混合检索
- 优化 Reranker
- 优化上下文组装

#### Skill 过多无法命中

- 修改 description，说明什么时候调用
- 添加负向样例，说明什么时候不要调用
- Skill 分层，先找大的，后找小的
- 召回 + 重排

#### Function Calling 模型如何决定调用工具

##### 流程

- HTTPS
- JSON API 请求 / 响应
- 模型输出结构化 tool call
- 应用代码执行工具

##### 坑

###### JSON 格式错误

- 代码解析
- 失败重试

###### 参数类型错误

- JSON Schema 校验 / 业务校验

###### 参数缺失或多余

- 后端严格限制格式

###### 工具选错

- 工具白名单
- 用户权限校验
- 高风险操作二次确认

###### 重复调用

- 业务幂等
- 唯一键
- 状态查询

###### 并行 Tool Call

- 是否允许并发
- 是否需要串行
- 是否存在依赖
- 是否共享事务

###### 上下文过大

- 分页
- 摘要
- 字段过滤
- 结果引用

##### 安全推荐流程

- 模型输出 tool call
- 解析 JSON
- 校验 tool name 白名单
- 校验 JSON Schema
- 校验用户权限 / 租户
- 校验业务规则
- 幂等检查
- 执行工具
- 校验结果
- 返回 Tool Result

##### 工具调用出错

###### Tool 层 **工具可靠**

- 参数校验
- 业务字段校验
- 返回结果校验
- 超时
- 限流
- 幂等
- 错误码转换
- 脱敏

###### Agent 层 **重试降级**

- Schema 是否正确
- 数据是否完整
- 证据是否可信
- 业务规则是否满足

###### System 层 **统一治理**

- 超时 / 重试 / 熔断 / 限流
- 并发控制
- 权限校验
- 审计日志
- Trace 链路
- 结果校验
- 秘钥管理
- 任务取消

#### MCP 工具如何被发现、描述和连接

##### MCP 能不能返回流式

###### 支持 HTTP / SSE 流式传输

- HTTP + SSE 已不受 Codex 支持

###### 支持新版 Streamable HTTP

###### 但标准 tool call 通常返回完整 result

##### MCP 如何保证并发

###### 协议层

- 基于 JSON-RPC 2.0，每个 request 有不同的 id

###### 服务层

- HTTP Server 使用异步框架
- 每个 request 分配独立 task
- 工具调用使用线程池
- 长任务异步

###### Tool 层

- 无共享状态 / 加锁

#### Lang

##### LangChain 组件库：搭建 Agent / 工具调用 / Prompt / 模型调用链

###### RAG 问答

- Retriever
- PromptTemplate
- LLM Model

###### 简单工具调用

- Tool
- OutputParser

###### 文档加载 / 分片

- DocumentLoader
- TextSplitter

###### 向量库接入

- Embedding Model
- VectorStore

###### Prompt 编排

- PromptTemplate
- Chain

###### 简单链式流程

- Chain

##### LangGraph 流程编排框架：构建有状态 / 多步骤 / 可循环 / 可中断的 Agent 工作流

###### 状态 / 节点 / 边 / 条件跳转

###### 循环 / 中断 / 恢复 / checkpoint

###### 多 Agent 协作

#### 验证准确性

##### 构建评测集分层识别

###### 意图识别

###### 实体抽取

- 人工标注问题里的关键实体
- 判断是否抽对

###### 检索召回

- 人工标注
- 系统召回
- 看目标 chunk 是否在 TopK 里

###### 重排

###### 答案

###### 引用

###### 工具调用

#### 大模型推理高并发优化手段

##### 请求调度

###### 批处理

- 已完成立即移出
- 新请求动态加入当前批次

###### 请求队列

- 排队削峰
- 限制并发数，防止打满

###### 优先级调度

- VIP 优先，实时请求优先
- 长文本限制并发，避免单个大请求阻塞

##### 缓存

###### KV Cache

- 缓存 Transformer 已计算的 Key / Value
- 避免重复计算
- 降低 Decode 阶段计算量

###### Prefix Cache

- 缓存请求前缀
- 比如相同 System Prompt 不重复计算

###### KV Cache 管理

- 分页管理缓存
- 按需分配和回收
- 避免碎片

##### 批处理与并行

###### Dynamic Batching

- 将多个请求合并成一个 Batch
- 减少 GPU Kernel 调度次数

###### Tensor Parallel

- 一个模型拆到多个 GPU
- 适合模型太大无法放入单卡

###### Data Parallel

- 每张 GPU 部署完整模型副本
- 请求按负载分发
- 适合提高整体吞吐量

###### Pipeline Parallel

- 模型不同层分布到不同 GPU
- 适合超大模型
- 但可能增加通信和流水线空隙

##### 模型压缩

###### 量化

- FP16/BF16 -> INT8/INT4
- 降低显存占用
- 提高并发和推理速度
- 可能损失部分精度

###### 蒸馏

- 用大模型训练小模型
- 降低单次推理成本

###### 剪枝

- 删除部分不重要参数或结构
- 实际收益取决于硬件和推理框架

##### 减少计算量

###### 控制上下文长度

- 限制输入 Token 数
- 压缩历史对话
- 只检索相关 RAG 片段

###### 限制输出长度

- 设置 max_tokens
- 避免模型无意义地生成过长答案

###### Speculative Decoding

- 小模型先预测多个 Token
- 大模型批量验证
- 预测正确时可以加速生成

###### 路由小模型

- 简单问题交给小模型
- 复杂问题才使用大模型

#### LLM 生成 SQL

##### 生成过程

###### LLM 生成结构化查询意图

###### LLM 生成 SQL

###### SQL 解析成 AST

- 是否是 SELECT
- 访问的表是否在白名单
- 访问的字段是否允许
- 是否存在 DELETE / DROP
- 是否有 LIMIT
- WHERE 条件是否包含 tenant_id

###### 安全规则校验

###### 业务规则校验

###### EXPLAIN / 预检查

###### 只读数据库执行

###### 脱敏返回

##### 三层 RAG

###### DDL / Schema 检索

- 按表为单位
- 基本信息、枚举信息、索引信息、关联关系
- 检索部分，返回时补充元数据

###### 业务规则检索

- 一条规则一个 chunk
- 规则过多时，处理方式与 Skill 类似，进行分层检索

###### Few-shot 示例

- 问题 + SQL + 解释为一个完整样例

#### Redis 在 Agent 系统作用

##### 作用

###### 会话上下文

###### 短期记忆

###### 任务状态

###### 幂等键

###### 分布式锁

###### 限流

###### 缓存 RAG 结果

###### 缓存 Tool 结果

###### Stream 任务队列

##### 流程

- 读取 summary
- 读取最近消息
- 读取当前任务状态
- 检索长期记忆
- 组装 Prompt

##### 注意问题

- A 和 B 同时修改会话导致消息顺序错乱
- session + version
- WATCH / MULTI / EXEC
- Redis Stream
- 会话级分布式锁

#### 评估 Agent 效果

##### 组件评估

- 意图识别：Accuracy / F1
- 召回：Recall@K
- 重排：MRR / nDCG
- 工具选择：Tool Accuracy
- 参数生成：Schema Accuracy
- 结构化输出：JSON Valid Rate

##### 链路评估

- 问题
- 意图识别
- RAG 检索
- 工具调用
- 结果组装
- 最终回答

##### 端到端任务评估

###### 有真实用户

- 用户目标是否真正完成

###### 无真实用户

- 构造评测集
- 运行 Agent
- 记录完整轨迹
- 自动计算指标
- 抽检异常案例
- 分析失败原因
- 修改 Prompt、RAG、Skill 或工具
- 重新评测

###### 关键指标

- 任务成功率
- 答案准确率
- 引用是否真正支持答案
- 正确知识是否出现在前 K 个召回结果中
- 工具选择准确率
- 工具参数准确率
- 结构化输出可解析比例
- 无依据结论比例
- 证据不足时正确拒答比例
- P95 延迟
- 单任务成本

#### 用户取消

##### 流程

- 用户取消
- 记录取消意图
- 任务和子任务传播取消信号
- 各执行节点主动检查
- 停止后续步骤
- 清理临时资源
- 任务进入 `CANCELLED`

##### 任务状态机

- `PENDING`
- `RUNNING`
- `CANCEL_REQUESTED`
- `CANCELLING`
- `CANCELLED` / `CANCEL_FAILED`

##### 取消信号怎么传递

- 任务表保存状态
- 执行线程周期检查（Agent 级 / Tool 调用级 / Loop）

##### 多 Agent 取消传播

- 主 Agent 收到取消传播给子任务，不再调度新子任务
- 子任务检查主任务和子任务是否已取消

##### Tool 如何真正取消

###### 普通 Java 任务

- `Future.cancel(true)`
- 任务内部检查中断标志

###### HTTP 调用

- 取消请求

###### 数据库查询

- 取消 `Statement`
- 关闭查询连接

###### Shell / 容器

- 记录进程 ID、发送终止信号

###### 异步外部任务

- 调用外部系统 cancel API

###### 模型流式生成

- 关闭流连接并停止后续 token

##### 最终保证边界

- 取消请求被持久化
- 取消信号能传播到子任务
- 未开始的步骤不再执行
- 正在执行的任务在检查点主动退出
- 任务最终进入明确状态
- 任务可以从检查点恢复
- 写操作幂等

#### 定位 Agent 问题

##### 给每次请求建立 Trace

- 意图识别
- RAG 召回
- Reranker
- 上下文组装
- 模型生成
- Tool 调用
- 最终回答

##### 每个阶段记录 Span

```text
Trace t-001
+-- intent_detection     80ms   SUCCESS
+-- retrieval            220ms  SUCCESS
|   +-- vector_recall    100ms
|   +-- bm25_recall      50ms
|   +-- reranker         70ms
+-- context_assembly     30ms   SUCCESS
+-- llm_generation       900ms  SUCCESS
+-- tool_call            300ms  FAILED
```

##### 保留中间产物

- 意图识别结果
- 实体提取结果
- 召回文档 ID 和分数
- Reranker 分数
- 最终注入模型的上下文
- 模型输出
- 工具名和参数
- 工具返回结果
- 最终答案和引用

##### 每一步使用结构化状态

- `SUCCESS`：继续
- `EMPTY`：扩大召回或换检索方式
- `TIMEOUT`：重试或降级
- `SCHEMA_ERROR`：修复输出
- `PERMISSION_DENIED`：直接拒绝

##### 可重放机制

- 问题
- Prompt 版本
- 模型版本
- Skill 版本
- 检索结果
- 工具参数
- 工具结果
- 配置版本

### Agent 项目

#### AgentRun / Attempt / Step

##### 解决问题

- 重试时覆盖历史状态
- 超时后无法判断谁有权提交结果
- 迟到结果污染当前运行
- 无法还原 Agent 执行过程

##### 解决方式

- `AgentRun`
  - 表示一次完整业务运行
- `Attempt`
  - 表示一次实际执行
  - 每次重试创建新的 Attempt
- `Step`
  - 表示一次具体步骤
  - 例如模型调用、检索、工具调用、结果校验
- 旧 Attempt 的结果不能覆盖当前 Attempt

#### Outbox / Inbox

##### 解决问题

- 业务数据提交成功但消息发送失败
- 消息重复消费
- Agent 与工作流状态不一致

##### 解决方式

- `Outbox`
  - 业务数据和待发送事件在同一事务中提交
  - 后台异步发送
  - 失败后可以重试
- `Inbox`
  - 消费事件前记录事件 ID
  - 通过幂等键防止重复处理
- 目标是至少一次投递和业务幂等
- 不是绝对只发送一次

#### Lease + Heartbeat + Recovery

##### 解决问题

- 模型调用时间过长
- Worker 执行中崩溃
- 任务永久卡在执行中
- 多个 Worker 重复执行同一任务

##### 解决方式

- `Lease`
  - Worker 领取任务后获得临时执行权
- `Heartbeat`
  - 长时间执行期间定期续租
- `Recovery`
  - 租约过期后回收任务
  - 重新排队并创建新的 Attempt
- 旧 Worker 的迟到结果不能覆盖新结果

#### Worker 如何避免重复执行任务

##### 领取阶段

- 使用 `FOR UPDATE SKIP LOCKED`
- 多个 Worker 可以并发领取不同任务
- 领取任务、创建 Attempt 和设置租约在同一事务完成

##### 执行阶段

- Worker 持有有期限的 Lease
- 长时间执行时通过 Heartbeat 续租
- 提交结果前再次校验 Lease 和 Attempt
- 旧 Worker 的迟到结果不得覆盖新的 Attempt

##### 注意

- `SKIP LOCKED` 只解决并发领取
- 不能代替业务幂等
- 不能保证外部模型或者工具只调用一次


#### Recovery 如何回收失败任务

##### 实现方式

- 周期性扫描租约已过期的运行
- 多实例通过 `SKIP LOCKED` 分批领取
- 将原 Attempt 标记为超时或失败
- 根据重试策略设置新的 `availableAt`
- 到期后由 Worker 创建新 Attempt 执行

##### 雪崩保护

- 限制每次扫描数量
- 使用指数退避和随机抖动
- 设置租户并发配额
- 设置最大重试次数和死信状态
- Recovery 只负责状态回收，不直接执行模型调用

#### 如何防止迟到结果污染状态

##### 场景

- Attempt 1 调用模型超时
- Recovery 创建 Attempt 2
- Attempt 1 随后返回成功结果

##### 处理方式

- AgentRun 保存当前有效 Attempt
- Worker 提交结果时校验 Attempt ID
- 同时校验 Lease Token 或版本号
- 只有当前有效 Attempt 可以改变 AgentRun
- 迟到结果只能记录审计，不能推进流程

#### Checkpoint 能解决什么问题

##### 能解决

- Worker 重启后恢复已完成的执行步骤
- 避免重复进行已经确认完成的纯计算步骤
- 保存计划、工具结果和中间上下文

##### 不能解决

- 不能自动撤销已经产生的外部副作用
- 不能修复语义上错误但技术上成功的工具调用
- 不能保证概率模型再次执行得到相同答案

##### 外部副作用处理

- 优先使用幂等键
- 写操作需要人工确认或补偿机制
- Checkpoint 只能恢复执行位置
- 不能代替 Saga、补偿事务和业务对账

#### DeepSeek 如何调用 MCP 工具

##### 职责划分

- DeepSeek 不直接连接 MCP Server
- 平台是 MCP Host 和 MCP Client
- 平台负责初始化、发现和调用 MCP Server
- DeepSeek 只负责选择工具并生成参数

##### 调用过程

- 平台从 Tool Registry 获取当前允许的工具
- 将工具 Schema 发送给 DeepSeek
- DeepSeek 返回原生 `tool_calls`
- 平台校验工具名称和 arguments
- Tool Registry 再校验租户授权、Schema 和风险策略
- MCP Client 执行 `tools/call`
- 工具结果回注模型生成最终答案

#### 为什么需要 Tool Registry

##### 解决问题

- 不能把 MCP Server 返回的所有工具直接暴露给模型
- 不同租户拥有不同的数据和工具权限
- 工具调用可能产生外部副作用

##### 职责

- 管理平台注册的工具
- 管理租户授权
- 管理 AgentVersion 的工具绑定
- 校验参数 Schema
- 执行超时、幂等、审计和结果大小限制
- 根据风险决定是否需要人工确认

##### 权限关系

- 模型可见工具集合
- 等于平台注册、租户授权和 AgentVersion 绑定的交集

#### 原生 tool_calls 和文本 JSON 有什么区别

##### 原生 tool_calls

- Provider 使用固定协议返回工具调用
- 工具名称和参数位于明确的协议字段
- 避免从普通文本中提取 JSON
- 协议结构更加稳定

##### 仍然需要校验

- 工具名称可能不存在
- arguments 可能缺少必填字段
- 参数类型和业务值可能错误
- 模型可能选择了无权限工具

##### 结论

- 原生 `tool_calls` 降低格式解析失败
- 不能代替 Schema、权限和业务校验

#### Agent 配置为什么必须版本化

##### 版本内容

- System Prompt
- 模型和 Provider
- 输入输出 Schema
- 工具绑定
- RAG 配置
- Guardrail 和预算策略

##### 原因

- 运行中的 Agent 不能受到配置更新影响
- 历史结果必须能够解释和审计
- 新版本失败时可以回滚
- 流程定义必须绑定明确的 AgentVersion

##### 更新方式

- 已发布版本不可直接修改
- 创建新的草稿版本
- 测试通过后发布
- 新流程实例使用新版本
- 历史实例继续使用原版本

#### 项目最大的技术难点是什么

##### 不是调用大模型

- 调用模型本身只是一个 HTTP 请求
- 真正困难的是将不确定执行接入确定性业务流程

##### 具体难点

- 长时间任务的可靠执行和恢复
- 模型、工具和流程之间的一致性
- 迟到结果、重复消息和重复工具调用
- 租户权限、凭据和数据隔离
- 概率结果的校验、证据和人工兜底
- Agent 配置和运行事实的版本化

## 问题排查

### 问题记录

#### 代码未生效 2026/07/29

##### 现象：代码已提交，Jenkins 中 Maven 成功生成新 JAR，但新代码未生效

##### 原因：之前调整过 JDK 17 拉取配置，与当前 Dockerfile 不匹配，导致镜像构建、推送失败。流水线未中断，继续部署了旧镜像。

##### 解决方案

- 统一 Jenkins 与 Dockerfile 使用的 JDK 17 基础镜像，建议使用公司 Harbor 中的固定镜像。
- 清理或修复 Jenkins 节点的 BuildKit/镜像代理缓存。
- Docker build 或 push 失败时立即终止流水线，禁止继续部署。
- 新镜像推送成功后重新拉取并创建容器，确认镜像 digest 和运行 JAR 为最新版本。

## 场景题

### 千万级大表如何做数据清理

#### 问题：`create_time` 没有索引会大范围加锁

#### 思路

- 获取主键 `min_id`，`max_id`
- 分段取出第一个区间所有数据
- 每条记录增加一个额外的字段 `hasNeedDelItem`
- 根据 ID 删除

#### 具体实现

- 时间索引 + 主键
- 游标分页
- JPA 流式查询

### ZSet 排行榜，分数-时间排序

#### 具体实现

- 使用 `userId` 作为 ZSet 的 `member`
- 使用“业务分数 + 时间权重”作为 ZSet 的 `score`
- 不建议直接把分数和时间戳当字符串拼接
- 应该计算出一个 Double 类型的数值
- `score = 业务分数 + 时间戳 / 时间倍率`

### Redis 挂了怎么办

#### 发现问题

- Redis 自己做监控
- 业务中做成功率监控

#### 事前-限流&降级

- 限制用户流量，仅对部分用户提供服务
- 优先保护数据库不被打崩

#### 备份

- 切换 Redis 备份实例

#### 本地缓存

- 存在不一致问题
- 根据业务场景考虑

### 商品加入购物车断网，联网时如何同步

#### 本地缓存

- 断网时缓存加入购物车的请求
- 使用本地数据库 SQLite
- 需要设计数据结构包含 ID、操作、时间戳等

#### 网络检测

- 网络状态监听 API
- 网络可用时触发同步

#### 重试

- 指数退避

#### 幂等

- 服务器存储唯一请求 ID

### 不使用分布式锁，防止用户重复点击

#### 按钮置灰

- 点击后立即禁用按钮
- 请求完成后恢复按钮
- 只能改善用户体验
- 可以被刷新、重试或脚本绕过

#### 幂等 Token

- 为每次业务请求生成唯一 Token
- 客户端提交请求时携带 Token
- 服务端校验 Token 是否已经使用
- 第一次请求成功后标记 Token 已使用
- 后续相同 Token 的请求直接拒绝
- Token 校验和业务处理需要保证原子性
- 可以使用 Redis 或数据库保存 Token

#### 数据库唯一索引

- 为业务请求生成唯一幂等号
- 将幂等号保存到业务表
- 对幂等号建立唯一索引
- 重复请求触发唯一键冲突
- 作为防止重复提交的最终兜底

#### 乐观锁

- 使用版本号或状态字段控制并发更新
- 更新时携带原版本号或原状态
- 根据影响行数判断是否处理成功
- 适合订单状态、支付状态等场景
- 例如：`UPDATE order_request SET status = 'SUCCESS', version = version + 1 WHERE id = ? AND status = 'INIT' AND version = ?`

#### 悲观锁

- 查询数据时加排他锁
- 使用 `SELECT ... FOR UPDATE`
- 其他事务需要等待当前事务完成
- 需要在同一个事务中完成查询和更新
- 适合必须串行处理的场景
- 会增加锁竞争和等待时间

#### 滑动窗口限流

- 统计最近一段时间内的请求次数
- 窗口随着当前时间持续移动
- 限制用户在单位时间内的请求次数
- 相比固定窗口更加平滑
- 适合接口防刷和操作频率限制

#### 固定窗口限流

- 将时间划分为固定时间段
- 统计每个时间段内的请求次数
- 超过限制后拒绝请求
- 实现简单，性能较高
- 存在窗口边界突发问题

#### 布隆过滤器

- 记录用户是否可能执行过某个操作
- 快速过滤明显重复请求
- 内存占用较小
- 查询性能较高
- 判断不存在时一定不存在
- 判断存在时可能误判
- 不能作为最终幂等判断依据

#### 推荐组合

- 按钮置灰：改善用户体验
- 限流：限制请求频率
- 幂等 Token：识别同一次业务请求
- 数据库唯一索引：防止业务数据重复
- 乐观锁：防止状态被并发修改
- 布隆过滤器：减少无效请求和数据库查询
