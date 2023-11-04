**Clickable Text Functions**

Here's a short guide on how to assign actions to clickable text.

The problem: The only away to assign a click action to a text component is to use the RUN_COMMAND action, which makes the player run a specified command when clicking on the component.

The solution: Assign a unique parameter indicating an internal ID for an action to execute. We don't even need to use a registered command.

Implementation:
We'll use the `Consumer` interface. The interface has a single method (`void accept(T element);`) that is perfect for our use-case scenario.
Here's an example:

```java
public class ClickableTextRegistry implements Listener { // Make sure to register this listener!

  private final Map<String, Consumer<Player>> functions = new HashMap<>();

  public String createCommand(String identifier, Consumer<Player> function) {
    functions.put(identifier, function);
    return "/" + idenfitier;
  }

  public String createCommand(Consumer<Player> function) {
    return createCommand(UUID.randomUUID().toString(), function);
  }

  // Feel free to make methods that accept runnables instead ..

  @EventHandler
  public void onCommand(PlayerCommandPreprocessEvent event) {
    String command = event.getMessage().substring(1); // Remove the first slash
    Player player = event.getPlayer();

    Consumer<Player> function = funcions.get(command); // change get to remove if you don't re-use identifiers, otherwise you end up with a memory leak. Fun!
    
    if(function == null) {
      return;
    }

    function.accept(player);
    event.setCancelled(true);
  }
}
```

Usage:

```java
String command = createCommand(player -> {
  player.sendMessage("You clicked on the text!");
});

// assign it to whatever component you have..
```