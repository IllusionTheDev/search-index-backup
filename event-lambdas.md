**Event Lambdas**

During the development of a project, you may come to a situation where your project's structure requires the registration of an event listener in runtime.
Below is a simple utility method that may end up coming handy.

```java
public interface EventBus {

  /**
   * Registers an event handler for the specified event class
   * @param eventClass The event class
   * @param handler The event handler
   * @param <T> The event type
   */
  <T extends Event> void subscribe(Class<T> eventClass, Consumer<T> handler);

  /**
   * Unregisters all associated handlers
   */
  void dispose(); 

}
```

And a simple implementation

```java
public class SimpleEventBus implements EventBus, Listener {

  private final JavaPlugin plugin;

  public SimpleEventBus(JavaPlugin plugin) {
    this.plugin = plugin; // Dependency inject the plugin variable
  }

  @Override
  public <T extends Event> void subscribe(Class<T> eventClass, Consumer<T> handler) {
    // Event Class, Listener, Event Priority, EventExecutor, Plugin, Ignore Cancelled
    Bukkit.getPluginManager().registerEvent(eventClass, this, EventPriority.NORMAL, (listener, event) -> {
      if(eventClass.isInstance(event)) {
        handler.accept(eventClass.cast(event));
      }
    }, plugin, true);
  }  

  @Override
  public void dispose() {
    HandlerList.unregisterAll(this);
  }
}
```

Sample usage:

```java
EventBus bus = new SimpleEventBus(plugin);

bus.subscribe(PlayerJoinEvent.class, (event) -> {
  Player player = event.getPlayer();
  ...
});
```

***Avoid using this when regular listener usage is possible. This is handy for niche situations (Minigame development)***