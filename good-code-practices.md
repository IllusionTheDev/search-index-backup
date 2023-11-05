### When naming variables, use descriptive, intuitive names:
A common mistake is to use short names for variables, such as "p" for player, or "i" for index. This is a bad practice, as it makes your code harder to read and understand. Instead, use descriptive names, such as "player" or "index".        

Non-compliant code:
```java
@EventHandler
public void onPlayerJoin(PlayerJoinEvent e) {
    Player p = e.getPlayer();
    p.sendMessage("Welcome to the server!");
}
```

Compliant code:
```java
@EventHandler
public void onPlayerJoin(PlayerJoinEvent event) {
    Player player = event.getPlayer();
    player.sendMessage("Welcome to the server!");
}
```

### Re-use variables when possible:
Many common methods will return a new object in every call, a few common ones are:
- Player#getLocation
- Location#getBlock
- ItemStack#getItemMeta
- Enum#values

Following this practice will also result in cleaner code, with the added bonus of being more performant.

Non-compliant code:
```java
@EventHandler
public void onPlayerJoin(PlayerJoinEvent event) {
    boolean standingOnGround = event.getPlayer().getLocation().getBlock().getType() == Material.GRASS_BLOCK ||
            event.getPlayer().getLocation().getBlock().getType() == Material.DIRT;
    
    if(standingOnGround) {
        event.getPlayer().sendMessage("You're standing on grass or dirt!");
    } else {
        event.getPlayer().sendMessage("You're not standing on grass or dirt!");
    }
}
```

Compliant code:
```java
@EventHandler
public void onPlayerJoin(PlayerJoinEvent event) {
    Player player = event.getPlayer();
    Location location = player.getLocation();
    Block block = location.getBlock();
    
    Material blockType = block.getType();
    
    boolean standingOnGround = blockType == Material.GRASS_BLOCK || blockType == Material.DIRT;
    
    if(standingOnGround) {
        player.sendMessage("You're standing on grass or dirt!");
    } else {
        player.sendMessage("You're not standing on grass or dirt!");
    }
}
```

### Avoid repeating collections calls:
`Map#containsKey` and `Map#get` can be merged in a single call to `Map#get` and checking for null.<br>
`Map#keySet` and `Map#get` can be merged in a single call to `Map#entrySet` and iterating over the entries.<br>
`Map#containsKey` and `Map#remove` can be merged in a single call to `Map#remove`.<br>
`Map#containsKey` and `Map#put` can be merged in a single call to `Map#putIfAbsent`.<br>

Non-compliant code:
```java
public void doSomething(Map<String, Integer> map) {
    if(map.containsKey("key")) {
        Integer value = map.get("key");
        map.remove("key");
        map.put("key", value + 1);
    }
}
```

Compliant code:
```java
public void doSomething(Map<String, Integer> map) {
    map.computeIfPresent("key", (key, value) -> value + 1);
}
```

### Perform the most performant checks first
When checking for multiple conditions, always check the most performant ones first.

Non-compliant code:
```java
public boolean isApplicable(Player player) {
    return checkBlocks(player) && checkInventory(player) && player.isOp();
}
```

Compliant code:
```java
public boolean isApplicable(Player player) {
    return player.isOp() && checkInventory(player) && checkBlocks(player);
}
```

### Don't expose mutable collections
When returning a collection, make sure to return an immutable copy of it, to prevent the caller from modifying it.

Non-compliant code:
```java
public class MyManager {
   
   private final Map<UUID, PlayerData> playerData = new HashMap<>();

   public void setPlayerData(UUID uuid, PlayerData data) {
        playerData.put(uuid, data);
   }
   
   public Map<UUID, PlayerData> getPlayerData() {
        return playerData;
   }
}
```

Compliant code:
```java
public class MyManager {
   
   private final Map<UUID, PlayerData> playerData = new HashMap<>();

   public void setPlayerData(UUID uuid, PlayerData data) {
        playerData.put(uuid, data);
   }
   
   public Map<UUID, PlayerData> getPlayerData() {
        return Collections.unmodifiableMap(playerData);
   }
}
```

### Follow SOLID and DRY principles
SOLID is an acronym that encompasses five simple principles: <br>
#### Single Responsibility Principle
A class should have one, and only one, reason to change. <br>
#### Open-Closed Principle
You should be able to extend a classes behavior, without modifying it. A simple example is to reassign a variable to a subclass and use it as if it was the original class. <br>
#### Liskov Substitution Principle
Classes should be replaceable by their subclasses, without breaking the code. <br> Aim to make your code as generic as possible, and avoid using instanceof in favor of a more polymorphic data structure <br>
#### Interface Segregation Principle
Make fine grained interfaces that are client specific. <br> This is a bit more advanced, and is usually used in libraries. <br>
#### Dependency Inversion Principle
Depend on abstractions, not on concretions. You shouldn't depend on a specific implementation, but rather on an interface. <br>
<br>

### Use primitives when possible
Autoboxing and unboxing is a process where the compiler automatically converts between primitive types and their wrapper classes (int <-> Integer). This process is very expensive, and should be avoided when possible.
Use primitives when possible, and prefer using unboxed classes when possible. (Int2ObjectHashMap, IntSupplier, Predicate)

Non-compliant code:
```java
Map<Integer, String> map = new HashMap<>();
map.put(1, "Hello");
map.put(2, "World");

String hello = map.get(1);
```

Compliant code:
```java
Int2ObjectMap<String> map = new Int2ObjectHashMap<>();
map.put(1, "Hello");
map.put(2, "World");

String hello = map.get(1);
```

### Override uneceessary methods
When extending a class, make sure to override all methods that are wasteful, such as fire checks for custom ArmorStand entities.

### PSA: Always seek to improve your code
This guide is not a set of rules, but rather a set of guidelines. It is important to understand that there is no such thing as perfect code, and that there is always room for improvement. Always seek to improve your code, and don't be afraid to ask for feedback. A second pair of eyes can help you spot mistakes you might have missed.