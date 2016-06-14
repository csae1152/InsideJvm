# InsideJvm
In this tutorial we will dive deep into the JVM habits and how the hopefully should influcence your programming style. Both within Java and Scala.

trait Cloneable extends java.lang.Cloneable 

Let's have a look at Cloneable inside the JVM.

Java Virtual Machine Support for Non-Java Languages
===================================================


Static and Dynamic Typing
A programming language is statically typed if it performs type checking at compile time. Type checking is the process of verifying that a program is type safe. A program is type safe if the arguments of all of its operations are the correct type.

Java is a statically typed language. All typed information for class and instance variables, method parameters, return values, and other variables is available when a program is compiled. The compiler for the Java programming language uses this type information to produce strongly typed bytecode, which can then be efficiently executed by the JVM at runtime.

The following example of a Hello World program demonstrates static typing. Types are shown in bold.

```
import java.util.Date;

public class HelloWorld {
    public static void main(String[] argv) {
        String hello = "Hello ";
        Date currDate = new Date();
        for (String a : argv) {
            System.out.println(hello + a);
            System.out.println("Today's date is: " + currDate);
        }
    }
}
```

A programming language is dynamically typed if it performs type checking at runtime. JavaScript and Ruby are examples of dynamically typed languages. These languages verify at runtime, rather than at compile time, that values in an application conform to expected types. These languages typically do not have any type information available at compile time. The type of an object can be determined only at runtime. Hence, in the past, it was difficult to efficiently implement them on the JVM.

In many contexts, type parameters are actually saved in class-files and exploitable via reflection, even despite erasure. For example, the following program prints class java.lang.String:

import java.lang.reflect.Field;
import java.lang.reflect.ParameterizedType;
import java.util.ArrayList;

public class ErasureWhatErasure {
    private final ArrayList<String> foo = null;

    public static void main(final String... args) throws Exception {
        final Field fooField = ErasureWhatErasure.class.getDeclaredField("foo");
        final ParameterizedType fooFieldType =
            (ParameterizedType) fooField.getGenericType();
        System.out.println(fooFieldType.getActualTypeArguments()[0]);
    }
}
All "erasure" means is that when you create an instance of a parameterized type, the type parameter is not recorded as part of the instance; new ArrayList<String>() and new ArrayList<Integer>() create identical instances. But even in Java, there are a few well-known workarounds, such as:

Something like new ArrayList<String>() { } actually creates a new instance of an anonymous subclass of ArrayList<String>, and the subclassing relationship does record the type parameter. So you can retrieve the String reflectively. (Specifically: ((ParameterizedType) new ArrayList<String>() { }.getClass().getGenericSuperclass()).getActualTypeArguments()[0] is String.class.) This is exploited by various frameworks, such as Guice and Gson.

If you're creating your own class, you can require the type parameter to be passed via the constructor, and store it in a field. (The compiler can help you enforce that the constructor argument matches the type parameter. And if the type argument is itself generic, you can combine this with workaround #1 to ensure that you have the whole type argument.)

Another language residing on the JVM could reify generics by making #2 happen implicitly; whenever you declared a generic class, the type-parameter field(s) and constructor-parameter(s) would be added implicitly, and whenever you instantiate or extend it, type-argument(s) would be implicitly copied to constructor-argument(s) to populate them. Java already does the same thing — implicit fields and constructor-parameters/arguments — in a different context, namely, for local classes that refer to final local variables in their containing methods.

The main limitation is that generic classes in the JDK — the collections framework, java.util.Class, etc. — don't already have this setup, and alternative languages running in the JVM can't add it to them. So such languages would need to provide their own equivalents of the JDK classes. But they can still use the JVM itself.




