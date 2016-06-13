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
