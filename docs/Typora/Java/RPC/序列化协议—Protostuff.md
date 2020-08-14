# 序列化协议—Protostuff

## 简介

protostuff 基于Google protobuf，提供了更多的功能和更简易的用法

## 相关依赖

```xml
<dependency>
       <groupId>io.protostuff</groupId>
       <artifactId>protostuff-core</artifactId>
       <version>1.6.0</version>
</dependency>
<dependency>
      <groupId>io.protostuff</groupId>
      <artifactId>protostuff-runtime</artifactId>
      <version>1.6.0</version>
</dependency>
```

## 序列化工具类

可以不必修改这个工具类的代码，直接使用

```java
public class ProtostuffUtils {
    //避免每次序列化都重新申请Buffer空间
    private static LinkedBuffer buffer = LinkedBuffer.allocate(LinkedBuffer.DEFAULT_BUFFER_SIZE);
    //缓存Schema
    private static Map<Class<?>, Schema<?>> schemaCache = new ConcurrentHashMap<Class<?>, Schema<?>>();
    //序列化方法，把指定对象序列化成字节数组
    @SuppressWarnings("unchecked")
    public static <T> byte[] serialize(T obj) {
        Class<T> clazz = (Class<T>) obj.getClass();
        Schema<T> schema = getSchema(clazz);
        byte[] data;
        try {
            data = ProtostuffIOUtil.toByteArray(obj, schema, buffer);
        } finally {
            buffer.clear();
        }
        return data;
    }
    //反序列化方法，将字节数组反序列化成指定Class类型
    public static <T> T deserialize(byte[] data, Class<T> clazz) {
        Schema<T> schema = getSchema(clazz);
        T obj = schema.newMessage();
        ProtostuffIOUtil.mergeFrom(data, obj, schema);
        return obj;
    }
    //获取序列化对象的组织结构
    @SuppressWarnings("unchecked")
    private static <T> Schema<T> getSchema(Class<T> clazz) {
        Schema<T> schema = (Schema<T>) schemaCache.get(clazz);
        if (schema == null) {
            schema = RuntimeSchema.getSchema(clazz);
            if (schema == null) {
                schemaCache.put(clazz, schema);
            }
        }
        return schema;
    }
}
```

### 参考资料

[protoStuff实现序列化和反序列化（java）](https://juejin.im/post/6844903919022243848)