1,类加载器:
    顾名思义，类加载器(class loader) 用来加载Java类到Java虚拟机中。
    一般来说，Java虚拟机使用Java类的方式如下：
        Java源文件(.java文件)再经过Java编译器编译之后就被转换成Java字节码文件(.class文件),
        类加载器负责读取Java字节代码，并转换成java.lang.Class类的一个实例。每个这样的实例用来
        表示一个Java类，通过此实例的newInstance()方法就可以创建出该类的一个对象，
2,ClassLoader类介绍
    java.lang.ClassLoader类的基础职责就是根据一个指定的类的名称，找到或生成其对应的字节代码。然后从这些字节代码中
    定义出一个Java类，即java.lang.Class类的一个实例。除此之外，ClassLoader还负责加载 Java 应用所需的资源，
    如图像文件和配置文件等。
3,类加载器的树状结构
    一类是系统提供的，另一类则是开发人员完成的
    系统提供的主要有:
        引导类加载器(bootstrap class loader): 他用来加载Java的核心库，是用原生代码来实现的，并不继承Classloader
        拓展类加载器(extensions class loader): 他用来加载Java的拓展库，Java虚拟机的实现会提供一个拓展库目录。
            该类加载类在此目录里面查找并加载Java类。
        系统加载器(system class loader): 他根据Java应用的类路径(classpath)来加载Java类一般来说Java应用中的类都是由它来完成完成加载的；
4,类加载器的代理模式
    类加载器在尝试自己去查找某个类的字节码并定义它时，会先代理给其父类加载器，由父类加载器先去尝试加载这个类，依次类推
    *** Java虚拟机是怎么判断两个是相同的 ***
        Java虚拟机不仅要看类的全名是相同的，还要看加载此类的类加载器是否一样，只有两者都相同的情况下，才认为两个类是相同的。即便是同样的字节码文件，
        被不同的类加载器加载之后所得到的类也是不同的。
    代理模式是为了保证 Java 核心库的类型安全。所有 Java 应用都至少需要引用 java.lang.Object类，也就是说在运行的时候，java.lang.Object这个类需要被加载到 Java 虚拟机中。如果这个加载过程由 Java 应用自己的类加载器来完成的话，很可能就存在多个版本的 java.lang.Object类，而且这些类之间是不兼容的。通过代理模式，对于 Java 核心库的类的加载工作由引导类加载器来统一完成，保证了 Java 应用所使用的都是同一个版本的 Java 核心库的类，是互相兼容的。
5,加载类的过程
    真正完成类的加载工作是通过调用 defineClass来实现的；而启动类的加载过程是通过调用 loadClass来实现的。前者称为一个类的定义加载器（defining loader），后者称为初始加载器（initiating loader）。在 Java 虚拟机判断两个类是否相同的时候，使用的是类的定义加载器。也就是说，哪个类加载器启动类的加载过程并不重要，重要的是最终定义这个类的加载器。
6,Class.forName
    Class.forName是一个静态方法，同样可以用来加载类。该方法有两种形式：Class.forName(String name, boolean initialize, ClassLoader loader)和 Class.forName(String className)。第一种形式的参数 name表示的是类的全名；initialize表示是否初始化类；loader表示加载时使用的类加载器。第二种形式则相当于设置了参数 initialize的值为 true，loader的值为当前类的类加载器。Class.forName的一个很常见的用法是在加载数据库驱动的时候。如 Class.forName("org.apache.derby.jdbc.EmbeddedDriver").newInstance()用来加载 Apache Derby 数据库的驱动。