# Reflection

Reflection in Java provides a way to change/modify the state of an object in runtime.

1. Helps to inspect the classes and look/modify the fields even if they are marked as private and final
2. Dynamically invoke methods on objects, even private methods, at runtime

## What are some practical use case of Reflections?



## Getting name fields:

```java
import java.lang.reflect.Field;

public class Reflection {
    public static void main(String[] args) throws NoSuchFieldException, IllegalAccessException {
        Cat myCat = new Cat("Fred", 3);

        System.out.println("Initially");
        System.out.println(myCat);
        System.out.println("###########################");

        // Iterating through all the fields by calling getDeclaredFields
        Field[] fields = myCat.getClass().getDeclaredFields();

        for(Field field: fields) {
            // System.out.println(field.getName()); // Prints the field name
            if(field.getName().equals("name")) {
                field.set(myCat, "Mercury");
            }
        }

        System.out.println("After setting name with reflection loop");
        System.out.println(myCat);
        System.out.println("###########################");

        // Get a field name based on the given name
        Field ageField = myCat.getClass().getDeclaredField("age");
        ageField.set(myCat, 6);

        System.out.println("After setting age with reflection getField");
        System.out.println(myCat);

    }

    static class Cat {
        private String name;
        private int age;

        public Cat(String name, int age) {
            this.name = name;
            this.age = age;
        }

        @Override
        public String toString() {
            return "Cat{" +
                    "name='" + name + '\'' +
                    ", age=" + age +
                    '}';
        }
    }
}
```

### Output:

```shell-session
Initially
Cat{name='Fred', age=3}
###########################
After setting name with reflection loop
Cat{name='Mercury', age=3}
###########################
After setting age with reflection getField
Cat{name='Mercury', age=6}

```

## Invoking methods

```java
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class Reflection {
    public static void main(String[] args) throws InvocationTargetException, IllegalAccessException {
        Cat myCat = new Cat("Fred", 3);

        // Gets the methods in an array
        Method[] methods = myCat.getClass().getDeclaredMethods();

        for (Method method: methods) { // Loop through the array
            if(method.getName().equals("privateMethod")) {
                method.setAccessible(true); // If it is private, set it to method.setAccessible(true)
                method.invoke(myCat, "Mercury"); // Invoke the method by passing the clas and relevant parameters
            } else if(method.getName().equals("privateStaticMethod")) {
                method.setAccessible(true);
                method.invoke(null); // If it's a static method, invoke the method with null
            }
        }
    }

    static class Cat {
        private String name;
        private int age;

        public Cat(String name, int age) {
            this.name = name;
            this.age = age;
        }

        private void privateMethod(String name) {
            System.out.println("I am a private method with name: " + name);
        }

        private static void privateStaticMethod() {
            System.out.println("I am a private STATIC method");
        }
    }
}
```

### Output

```shell-session
I am a private method with name: Mercury
I am a private STATIC method
```

## Usecases:

* **Framework Development:** Frameworks often need to work with various classes and their properties without knowing them in advance. Reflection helps achieve this flexibility.
* **Generic Programming:** Reflection allows writing code that operates on different types of objects without explicitly handling each type.
* **Dynamic Class Loading:** You can load classes dynamically based on user input or configuration at runtime using reflection.

## Issues with Reflection

1. Reflection often causes performance overhead, as it needs to iteratre through all the fields or methods during runtime
2. It might break the codebase as the behaviour might not be consistent, if implemented haphazardly
3. Modifying private fields or invoking private methods out of the context could be a serious cause of concern for security.
