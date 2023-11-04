**Lambdas**

Lambdas in java are incredibly useful but a bit tough to learn. This guide aims at simplifying the concept so it's a little easier to understand.

The backbone of lambdas are interfaces. An interface can be seen as a sort of "template" of a class, with no implementation. It represents a schema that
must be overriden and implemented accordingly.

Lambdas in turn, create an *anonymous* temporary class that implements the required interface given it has **one** and only one method requiring implementation.

Let's break down the Runnable interface: `void run();`
The main points of interest are:
- The return type: `void`
- The parameters (empty in this case)

Now, facing a method that requires a Runnable parameter, we're faced with three choices:
- Create a class that implements Runnable, override its method and create a new instance
- Create an instance of an anonymous class, overriding its method
- Creating a lambda (shortened version of the method above)

Let's assume we have this interface:
```java
public interface MessageSender {
  
  boolean sendMessage(Player player, String message);

}
```

We start by creating a lambda expression, with the given parameters and return type:
```java
MessageSender implementation = (player, message) -> { // parameters
    player.sendMessage(message);
    return true; // return type (boolean in this case)
};
```

This is a valid replacement to either:
```java
MessageSender implementation = new MessageSender() {
    @Override
    public boolean sendMessage(Player player, String message) {
        player.sendMessage(message);
        return true;
    }
};
```

or

```java
public class SampleMessageSender implements MessageSender {

    @Override
    public boolean sendMessage(Player player, String message) {
        player.sendMessage(message);
        return true;
    }
}
```

You can embed lambdas directly as any other variable, without the need to specify it's a new instance of a class.

Edge-case scenarios:
- When dealing with a single parameter, the ()'s are irrelevant (Example: `player -> player.sendMessage("Hi");`)
- Braces are optional if you're doing one-liners
- When dealing with a method that matches your exact parameters and return type, you can use the :: syntax (Example: `this::sendMessage`)