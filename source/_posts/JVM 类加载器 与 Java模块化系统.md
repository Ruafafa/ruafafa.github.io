---
title: JVM 类加载器 与 Java模块化系统
comments: true
categories: [Java]
tags: [Java,JVM]
cover: https://ruafafa-photobed.oss-cn-beijing.aliyuncs.com/illust_111121482_20230925_001424.jpg
---
> Java 虚拟机设计团队有意把类加载阶段中的“通过一个类的全限定名来获取描述该 类的二进制字节流”这个动作放到 Java 虚拟机外部去实现，以便让应用程序自己决定如 何去获取所需的类。**实现这个动作的代码被称为“类加载器”（Class Loader）。 **  *// 类加载器是一段代码*

## 类与类加载器

对于任意一个类，都必须由加载它的类加载器和这个类本身一起共同确立其在 Java 虚拟机中的唯一性，每一个类加载器，都拥有一个独立的类名称空间

![image-20231207202637272](https://ruafafa-photobed.oss-cn-beijing.aliyuncs.com/image-20231207202637272.png)

被不同的类加载器所加载的同一个`.Class`文件，在 Java 虚拟机中是两个互相独立的类，使用 `instanceof` 进行比较时 或 做对象所属类型检 查时，结果会返回 false

## 双亲委派模型

Java 虚拟机视角下的两种类加载器：

```mermaid
flowchart LR
    a[类加载器]
    b1[启动类加载器]
    b2[其他所有的类加载器]
    a--ob1
    a--ob2
```

**启动类加载器（Bootstrap ClassLoader）**：使用 C++语言实现，是虚拟机自身的一部 分

**其他所有的类加载器**：这些类加载器都由 Java 语言实现，独立存在于 虚拟机外部，并且全都继承自抽象类 java.lang.ClassLoader

##### 

### 三层类加载器

- 启动类加载器 （Bootstrap ClassLoader）

  > 负责加载 存放在 \lib 目录，或者被-Xbootclasspath 参数所指定的路径中存放的， 而且是 Java 虚拟机能够识别的

- 扩展类加载器 （Extension ClassLoader）

  > 它负责加载 \lib\ext 目录中，或者被 java.ext.dirs 系统变量所指定的路径中所有的类 库

- 应用程序类加载器 （Application ClassLoader）

  > 负责加载用户类路径（ClassPath）上所有的类库

### 双亲委派模型

![image-20231207225619655](https://ruafafa-photobed.oss-cn-beijing.aliyuncs.com/image-20231207225619655.png)

> 双亲委派模型要求除了顶层 的启动类加载器外，其余的类加载器都应有自己的父类加载器。不过这里类加载器之间 的父子关系一般不是以继承（Inheritance）的关系来实现的，而是通常使用组合 （Composition）关系来复用父加载器的代码。

**工作过程：**如果一个类加载器收到了类加载的请求，它首先不会 自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成，每一个层次的类加 载器都是如此，因此所有的加载请求最终都应该传送到最顶层的启动类加载器中，只有 当父加载器反馈自己无法完成这个加载请求（它的搜索范围中没有找到所需的类）时， 子加载器才会尝试自己去完成加载。

**优点：**Java 中的 类随着它的类加载器一起具备了一种带有优先级的层次关系，保证 Java 程序的稳定运作



## Java 模块化系统

> JDK9引入，Java 模块化系统（Java Platform Module System，JPMS）
>
> 模块化的关键目标——可配置的封装隔离机 制

#### 模块概念

以下是一些关于Java模块的基本概念：

1. **模块描述文件 (`module-info.java`)**: 模块由一个特殊的文件`module-info.java`定义，该文件位于模块的根目录下。这个文件包含了模块的声明、依赖关系和其他元数据。

   ```
   javaCopy codemodule com.example.mymodule {
       requires some.other.module;
       exports com.example.mymodule.package1;
       exports com.example.mymodule.package2;
   }
   ```

   上述例子中，`com.example.mymodule` 模块声明了对 `some.other.module` 模块的依赖，同时导出了两个包 `com.example.mymodule.package1` 和 `com.example.mymodule.package2`。

2. **依赖性管理**: 模块系统引入了明确的依赖关系，开发人员需要在`module-info.java`文件中声明他们的模块依赖于哪些其他模块。这取代了之前的类路径（classpath）的方式，提供了更清晰、更安全的依赖性管理。

   ```
   javaCopy codemodule com.example.mymodule {
       requires some.other.module;
   }
   ```

3. **导出和封装**: 模块系统允许开发人员选择性地导出模块中的包，以使其对其他模块可见。这有助于实现更好的封装和隔离，减少了不必要的访问权限。

   ```
   javaCopy codemodule com.example.mymodule {
       exports com.example.mymodule.package1;
       exports com.example.mymodule.package2;
   }
   ```

4. **命名空间的隔离**: 模块系统引入了一种新的命名空间，使得不同模块中的相同包名不会发生冲突。这有助于解决类路径下的一些常见问题，例如JAR包之间的冲突。

5. **模块路径 (module path)**: 模块使用模块路径来替代传统的类路径。在启动Java应用程序时，开发人员可以通过`--module-path`选项指定包含模块的路径，模块系统会根据这个路径来加载和组织模块。

   ```
   bashCopy code
   java --module-path /path/to/modules -m com.example.mymodule/com.example.mymodule.Main
   ```

6. **模块化JAR (JMOD)**: Java模块系统引入了模块化JAR，这是一种新的JAR文件格式，用于支持模块化的打包和分发。

Java模块系统带来了许多优势，包括更好的封装性、可维护性和安全性。然而，在使用模块时，开发人员需要更加注意依赖关系的声明、模块的导出和其他模块系统的新特性。

#### 优势

- 解决了JDK 9 之前基于类路径（ClassPath）来查找依赖 的可靠性问题，在虚拟机启动时就可以检验出依赖关系在运行期是否完整
- 解决了原来类路径上跨 JAR 文件的 public 类型的可访问性 问题，模块提供了更精细的可访问性控制，必须明确声明其中哪一些 public 的类型可以被 其他哪一些模块访问



### 模块化下的类加载器

是扩展类加载器（Extension Class Loader）被平台类加载器（Platform Class  Loader）取代