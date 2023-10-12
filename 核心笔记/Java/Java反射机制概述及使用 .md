





# **Java反射机制概述及使用**

## 1.概述：

原则上来说Java并不属于动态语言，但是Java通过借助Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性及方法在一定意义上实现了动态语言的特征。               **框架 = 反射 + 注解 + 设计模式。**

## 2.反射机制提供的功能：

1. 在运行时判断任意一个对象所属的类
2. 在运行时构造任意一个类的对象
3. 在运行时判断任意一个类所具有的成员变量和方法
4. 在运行时获取泛型信息
5. 在运行时调用任意一个对象的成员变量和方法
6. 在运行时处理注解
7. 生成动态代理

## 3.Class(运行时类)的理解：

- 3.1 类的加载过程：程序经过javac.exe命令以后，会生成一个或多个字节码文件(.class结尾)。接着我们使用java.exe命令对某个字节码文件进行解释运行。相当于将某个字节码文件加载到内存中。此过程就称为类的加载。<u>加载到内存中的类，我们就称为运行时类</u>，此运行时类，就作为Class的一个实例。
- 3.2 换句话说，Class的实例就对应着一个运行时类。
- 3.3 加载到内存中的运行时类，会缓存一定的时间。在此时间之内，我们可以通过不同的方式获取此运行时类。

## 4.获取Class实例的几种方式：（前三种重点）

```java
//方式一：调用运行时类的属性：.class
        Class clazz1 = Person.class;
        System.out.println(clazz1);
```

```java
//方式二：通过运行时类的对象,调用getClass()
        Person p1 = new Person();
        Class clazz2 = p1.getClass();
        System.out.println(clazz2);

```

```Java
//方式三：调用Class的静态方法：forName(String classPath)
        Class clazz3 = Class.forName("com.atguigu.java.Person");
        System.out.println(clazz3);

        System.out.println(clazz1 == clazz2); //true
        System.out.println(clazz1 == clazz3); //true
```

```java
//方式四：使用类的加载器：ClassLoader  (了解)
ClassLoader classLoader = ReflectionTest.class.getClassLoader();
Class clazz4 = classLoader.loadClass("com.atguigu.java.Person");

        System.out.println(clazz4);
        System.out.println(clazz1 == clazz4);
```

## 5.使用类的加载器来读取文件

```java
public void test2() throws Exception {

        Properties pros = new Properties();
//此时的文件默认在当前的module下。
//读取配置文件的方式一：
//FileInputStream fis = new FileInputStream("jdbc.properties");
FileInputStream fis = new FileInputStream("src\\jdbc1.properties");
        pros.load(fis);

//读取配置文件的方式二：使用ClassLoader
//配置文件默认识别为：当前module的src下
//  ClassLoader classLoader = ClassLoadTest.class.getClassLoader();
// InputStream is = classLoader.getResourceAsStream("jdbc1.properties");
// pros.load(is);
        String user = pros.getProperty("user");
        String password = pros.getProperty("password");
    
        System.out.println("user =" + user);
        System.out.println("password =" + password);

    }
```

## 6.反射应用一：创建Class类的对象

```Java
1.代码举例：
Class<Person> clazz = Person.class;
Person obj = clazz.getDeclaredConstructor().newInstance();
        System.out.println(obj);

/*
 newInstance():调用此方法，创建对应的运行时类的对象。内部调用了运行时类的空参的构造器。
 jdk9.0 建议使用clazz.getDeclaredConstructor().newInstance();

        要想此方法正常的创建运行时类的对象，要求：
        1.运行时类必须提供空参的构造器
        2.空参的构造器的访问权限得够。通常，设置为public。

        在javabean中要求提供一个public的空参构造器。原因：
        1.便于通过反射，创建运行时类的对象
        2.便于子类继承此运行时类时，默认调用super()时，保证父类有此构造器

*/
```

## 7.反射应用二：获取Class类的完整结构(运行时类)

**我们可以通过反射，获取对应的运行时类中所有的属性、方法、构造器、父类、接口、父类的泛型、包、注解、异常等。。。。**

###### 1.属性

```Java
Class clazz = Person.class;
// 获取属性结构
// getFields():获取当前运行时类及其父类中声明为public访问权限的属性
        Field[] fields = clazz.getFields();
        for (Field f : fields) {
            System.out.println(f);
        }
        System.out.println();

// getDeclaredFields():获取当前运行时类中声明的所有属性。（不包含父类中声明的属性）
        Field[] declaredFields = clazz.getDeclaredFields();
        for (Field f : declaredFields) {
            System.out.println(f);
        }
```

###### 2.方法

```java
Class clazz = Class.forName("com.atguigu.java1.Person");
// getMethods():获取当前运行时类及其所有父类中声明为public权限的方法
        Method[] methods = clazz.getMethods();
        for (Method m : methods) {
            System.out.println(m);
        }
        System.out.println();

// getDeclaredMethods():获取当前运行时类中声明的所有方法。（不包含父类中声明的方法）
        Method[] declaredMethods = clazz.getDeclaredMethods();
        for (Method m : declaredMethods) {
            System.out.println(m);
        }
```

###### 3.构造器

```java
Class clazz = Person.class;
// getConstructors():获取当前运行时类中声明为public的构造器
    Constructor[] constructors = clazz.getConstructors();
        for (Constructor c : constructors) {
            System.out.println(c);
        }
        System.out.println();

// getDeclaredConstructors():获取当前运行时类中声明的所有的构造器
     Constructor[] declaredConstructors = clazz.getDeclaredConstructors();
        for (Constructor c : declaredConstructors) {
            System.out.println(c);
        }
```

###### 4.父类

```Java
/*
 获取运行时类的父类
  */
	Class clazz = Person.class;
        Class superclass = clazz.getSuperclass();
        System.out.println(superclass);
/*
 获取运行时类的带泛型的父类
   */
	Class clazz = Person.class;
		Type genericSuperclass = clazz.getGenericSuperclass();
		System.out.println(genericSuperclass);
	
```

###### 5.带泛型的父类的泛型

```Java
/*
 代码：逻辑性代码  vs 功能性代码
 */
Class clazz = Person.class;
        Type genericSuperclass = clazz.getGenericSuperclass();
        ParameterizedType paramType = (ParameterizedType) genericSuperclass;
//获取泛型类型
        Type[] actualTypeArguments = paramType.getActualTypeArguments();
//        System.out.println(actualTypeArguments[0].getTypeName());
System.out.println(((Class)actualTypeArguments[0]).getName());
```

###### 6.实现的接口

```Java
//获取运行时类实现的接口
Class clazz = Person.class;
        Class[] interfaces = clazz.getInterfaces();
        for (Class c : interfaces) {
            System.out.println(c);
        }
//获取运行时类的父类实现的接口
Class[] interfaces1 = clazz.getSuperclass().getInterfaces();
        for (Class c : interfaces1) {
            System.out.println(c);
        }
```

###### 7.所在的包

```Java
Class clazz = Person.class;
        Package aPackage = clazz.getPackage();
        System.out.println(aPackage);
```

###### 8.声明的注解

```Java
Class clazz = Person.class;
        Annotation[] annotations = clazz.getAnnotations();
        for (Annotation a : annotations) {
            System.out.println(a);
```

## 8.反射应用三：调用Class类的指定结构

###### 1.调用指定的属性

```java
Class clazz = Class.forName("com.atguigu.java1.Person");
//创建运行时类的对象
        Person p  = (Person)clazz.getConstructor().newInstance();
// 1. getDeclaredField(String fieldName):获取运行时类中指定变量名的属性
        Field name = clazz.getDeclaredField("name");

// 2.保证当前属性是可访问的
        name.setAccessible(true);
// 3.获取、设置指定对象的此属性值
        name.set(p,"Tom");
        System.out.println(name.get(p));
```

###### 2.调用指定的方法

```Java
Class clazz = Person.class;
//创建运行时类的对象
        Person p  = (Person)clazz.getConstructor().newInstance();

        /*
        1.获取指定的某个方法
        getDeclaredMethod():参数1 ：指明获取的方法的名称  参数2：指明获取的方法的形参列表
         */
Method show = clazz.getDeclaredMethod("show", String.class);

		//2.保证当前方法是可访问的
        show.setAccessible(true);

        /*
        3. 调用方法的invoke():参数1：方法的调用者  参数2：给方法形参赋值的实参
        invoke()的返回值即为对应类中调用的方法的返回值。
         */
        Object returnValue = show.invoke(p, "CHN");//String nation = p.show("CHN");
        System.out.println(returnValue);  //打印返回值
System.out.println("*************如何调用静态方法****************");

        // private static void showDesc()
        Method showDesc = clazz.getDeclaredMethod("showDesc");
        showDesc.setAccessible(true);

        //如果调用的运行时类中的方法没有返回值，则此invoke()返回null
//        Object returnVal = showDesc.invoke(null);
        Object returnVal = showDesc.invoke(Person.class);
        System.out.println(returnVal); //返回值为null
```

###### 3.调用指定的构造器

```Java
Class clazz = Person.class;
        //private Person(String name)
        /*
        1.获取指定的构造器
        getDeclaredConstructor():参数：指明构造器的参数列表
         */
        Constructor declaredConstructor = clazz.getDeclaredConstructor(String.class);

        //2.保证此构造器是可访问的
        declaredConstructor.setAccessible(true);

        //3.调用此构造器创建运行时类的对象
Person per = (Person) declaredConstructor.newInstance("Tom");
        System.out.println(per);
```

