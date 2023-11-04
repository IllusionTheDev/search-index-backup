**Completable Futures**

This guide uses lambdas. If you're new to them, read my lambda guide.

A CompletableFuture (javascript seems to call it promise, don't quote me on this) is an object that represents an action that will be executed, tied to a "CompletionStage"

- The default thread pool is the ForkCommonPool, you can set your own executor with the provided parameters
- A task may be completed exceptionally (a throwable has been thrown during the execution), you can handle this using the `exceptionally` methods (usually just pass a Function<Throwable, T> where T is the future type, which returns a value)

- CompletableFutures usually complete with a defined value (return type of the function), it can be the boxed `Void`.

- You can handle completion using the following methods:
- `thenRun(Runnable)` - Just runs code once the future has been complete
- `thenAccept(Consumer<T>)` - Provides a variable, you do whatever
- `thenApply(Function<T, U>)` - Provides a variable, you can return whatever you'd like
- `thenCompose(Function<T, CompletionStage<U>>)` - Used for chaining futures, where the main future now runs the sub-future's task

- Most then... methods (thenRun, thenAccept, thenCompose) have an async variant, the main difference is that it runs on a separate thread from the default (the one the future is using) one, but you can pass your own Executor (You can make a funky executor that runs code on the main class)

- To initialize a CompletableFuture, you should use the `runAsync` or `supplyAsync` methods, depending if you'd like a simple Runnable with no return type, or if you'd like to pass a Supplier to obtain a value:

- CompletableFutures like to "suck up" exceptions. When they do so, they trigger an "exceptional" state. You can then handle exceptions with either the `exceptionally` method, or use *combined* methods like `whenComplete(? super T, ? super Throwable)`

Example:
```java
CompletableFuture<Data> future = CompletableFuture.supplyAsync(() -> {
  Data data = database.fetchData(playerId); // blocking method
  data.setSomeValue(true);
  return data;
});

future.exceptionally(error -> {
  error.printStackTrace();
  return new ErrorData(); 
});

future.thenAccept((data) -> {
  player.sendMessage("The value is " + data.getSomeValue());
});

// This code still executes first, as the database code is being ran on another thread and won't be available this soon
player.sendMessage("Fetching data.."); 
```

Real-life scenario:

```java
public interface MyFancyDatabase {

  CompletableFuture<PlayerData> fetchPlayerData(UUID playerId);
  CompletableFuture<Void> savePlayerData(UUID playerId, PlayerData data);
  COmpletableFuture<Void> deletePlayerData(UUID playerId);

}
```

```java
MyFancyDatabase database = ...;
database.fetchPlayerData(playerId).thenAccept(data -> {
  // This block of code will run once the data has been fetched
}
```