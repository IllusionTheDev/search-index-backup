**Asking for Help**

During your development journey, there might come a time where you get stuck and need help. This guide covers a few tips that will get you answered quicker and more efficiently.

Let's say you want to create a textured skull item with no prior knowledge:

Before asking for help, it is important to research and attempt a solution towards your problem, I usually follow the following steps:
- Break down the problem into simple steps (In this case we want to first create an item, and them apply a texture to it)
- Research the steps individually, and understand how your steps work together (We'd figure out that we need to use the ItemStack class, and apply the texture to its meta)
- If you're struggling, google your problem. Make sure to efficiently use keywords, I usually just write `spigot`, followed by a 2-3 word summary (`spigot custom head itemstack`)
- No solutions? Look at how existing projects solve your problem (In this case I'd look at the source code of a plugin that messes with player heads, probably on GitHub)
- Still unable to find an answer? Now is the time to ask for help

When you're asking for help, it is important to describe your problem, what your solution is and any attempts you have made, as well as the expected result.

Here's an example of how to properly ask for help:
> Hey there, I am currently trying to create a custom-textured skull ItemStack with a base64 texture value. I already tried using the SkullMeta class but this only allows me to pass a player instead of a base64 string. Here's my current code:
```java
public ItemStack createSkullItem(String base64) {
    ItemStack skull = new ItemStack(Material.PLAYER_HEAD);
    SkullMeta meta = (SkullMeta) skull.getItemMeta();
    
    // I'm stuck
    
    return skull;
}
```

Here's an example of what not to do:
> hello
> how player head item works base64

When asking for help, there are a few principles that will help you get answered quicker, feel free to do research about the following:
- No Hello
- XY Problem