# Python基础

### 类和实例

和静态语言不同，Python允许对实例变量绑定任何数据。也就是说，对于两个实例变量，虽然它们都是同一个类的不同实例，但拥有的变量名称都可能不同

### 访问限制

实例的变量名以`__`开头，表示私有变量；变量名类似`__xxx__`，表示特殊变量，特殊变量是可以直接访问的

### 多态

* 对于静态语言（例如Java）来说，如果需要传入`Animal`类型，则传入的对象必须是`Animal`类型或者它的子类，否则，将无法调用`run()`方法。
* 对于Python这样的动态语言来说，则不一定需要传入`Animal`类型。我们只需要保证传入的对象有一个`run()`方法就可以了

## Json序列化

### Json的简单形式

{"firstName" : "Value"}

Json表示出来就是一个字符串

### python对象和Json的转换

python object -> json :  `json.dumps()`

json -> python object : `json.loads`

### 类的实例和Json的转换

需要写一个转换函数，然后将这个函数传入作为参数传入json的方法中

