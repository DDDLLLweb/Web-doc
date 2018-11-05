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
    类加载器在尝试自己去查找某个类的字节码并定义它时，会先代理给其父类加载器
