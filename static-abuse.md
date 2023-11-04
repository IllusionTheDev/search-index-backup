## Static abuse

The concept of static in Java is quite simple when you get to it. To understand static we first must understand how Object Oriented Programming
works.

When creating a class, we're actually creating a structure that an object will follow. This structure defines
what variables are contained within the object, and how the implemented methods interact with such variables.

An object, in java, is created using the `new` keyword, which calls a constructor defined in the class. You create
a constructor to define some base logic that applies upon initialization of the object, such as the common design pattern
of Dependency/Constructor Injection.

When we talk about an instance of an Object, we're talking about the object as a whole, with its data included. Think of an
object as a box or package of information with a pre-defined structure that is set by the class. That structure indicates
how the data contained within the package behaves.

When passing around objects, with the exception of primitives (`int`, `long`, `boolean`), we're actually passing around a memory
reference to the object. Think of this memory reference as a tracking code, which is used in an internal map/dictionary to indicate
what data belongs to what address. This helps minimize the amount of memory used and is commonly refered to as "Pass by Reference".

## Now, how does static come into play?

`static` is a *keyword* that indicates that the reference in question should be statically allocated. A statically allocated
object is permamently present in memory (ram), and can be accessible anywhere, at any given time.

By design, a statically allocated object will not be garbage collected, as it can still be accessible by any class at any given
time, even from other static objects or methods. With such a choice, it is exclusively meant for constants, and should also be used
by utility classes.

The only non-constant *allowed* use of `static` is in Singletons, which are basically a single constant instance, but are lazily initialized.

TL;DR - Valid usage in:
- Constants
- Utility classes
- Singletons (when DI is impossible)

### Examples

Here is an example of a utility class, with a constant

```java
public final class MyUtilityClass { // final to prevent inheritance

    private static final int NUMBER_OFFSET = 100; // constant value, permanent
    
    private MyUtilityClass() { // Private constructor to prevent initialization
    
    }
    
    public static int offsetNumber(int original) {
        return NUMBER_OFFSET + original;
    }
}
```

Here is an example of a lazy singleton (lazy means it is only initialized when needed)

```java
public class MySingleton {

    private static MySingleton INSTANCE = null;
    
    public static MySingleton getInstance() {
        if(INSTANCE == null) {
            INSTANCE = new MySingleton();
        }
        
        return INSTANCE;
    }
    
    private MySingleton() { // prevent external initialization
        // optional: check for duplicate instances
        if(INSTANCE != null) {
            throw new IllegalStateException("Duplicate instance");
        }
    }
    
    private int currentNumber = 0;
    
    public int getOffsetNumber() {
        return MyUtilityClass.offsetNumber(currentNumber++);
    }
}
```

Here is what **NOT** to do:

```java
public class Circus {

    private static PlayerManager playerManager;
    private static SomeOtherManager someOtherManager;
    private static String status = "broke";
    private static int money = -1;
    
    public static PlayerManager getPlayerManager() {
        return playerManager;
    }
    
    public static int getMoney() {
        return money;
    }
    
    public static String getStatus() {
        return status;
    }
    
    public static SomeOtherManager getSomeOtherManager() {
        return someOtherManager;
    }
}
```