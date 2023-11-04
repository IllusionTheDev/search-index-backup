**IProtocolLib Entity Metadata**

Pre-1.19, setting a packet's entity metadata values requires setting the desired fields on a (Wrapped)DataWatcher, and assigning that datawatcher (or the packed values, if doing with NMS) to the packet.

With 1.19, the entity metadata protocol changed. Instead of a (Wrapped)DataWatcher, you should create a List of (Wrapped) data values.

ProtocolLib example (1.19+)
```java
PacketContainer container = ...;

List<WrappedDataValue> values = List.of(
  new WrappedDataValue(0, Registry.get(Byte.class), (byte) 0x40)
);

container.getDataValueCollectionModifier().writeSafely(0, values);
```