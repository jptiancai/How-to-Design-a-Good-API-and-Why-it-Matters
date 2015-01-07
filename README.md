How-to-Design-a-Good-API-and-Why-it-Matters
===========================================

The offline pdf already in this repo.

##API的重要性

- 公司最大的资产
- 公司最大的负债

##好的API特征(和一个好的开源框架类似)

- 易于学习
- 即使没有文档,易于使用
- 很难用错
- 易于阅读和维护代码
- 很好的满足于需求
- 易于拓展
- 适合受众

##概览

- API设计流程

  - 收集需求:提取用户话语背后真实的需求,用户案例
  - 及早和经常更新API:单元测试
  - 写下服务提供接口非常重要:至少3个插件

-  一般规则

  - API应当只做一件事情并且要做好.
  - 实现细节不应该影响API本身.

```java
//设计API和散文一样.
//下列代码就很好地解释了如果汽车超速会发出警报的API
if (car.speed() > 2 * SPEED_LIMIT)
generateAlert("Watch out for cops!");

```

- 类设计

  - java中好的类API和坏的类API比较

```java
//最小的变动
Bad: Date, Calendar
Good: TimerTask
//子类要有意义
Bad:
Properties extends Hashtable
Stack extends Vector
Good: Set extends Collection
//继承的实现程度
Bad: Many concrete classes in J2SE libraries
Good: AbstractSet, AbstractMap

```


- 方法设计

  - 关注重载

```java

public TreeSet(Collection c); // Ignores order
public TreeSet(SortedSet s); // Respects order
```


- 异常设计

  - 避免返回需要异常处理的值:应该返回0长度的数组或者空collection,而不是null.


- 重构API设计

  - Vector中的分表操作

```java
//仅支持搜索,没有文档难于使用
public class Vector {
public int indexOf(Object elem, int index);
public int lastIndexOf(Object elem, int index);
...
//重构后
//支持所有操作
//易于使用,没有文档
public interface List {
List subList(int fromIndex, int toIndex);
...

}

}

```

  - Thread-Local变量

```java

// Broken - inappropriate use of String as capability.
// Keys constitute a shared global namespace.
public class ThreadLocal {
private ThreadLocal() { } // Non-instantiable
// Sets current thread’s value for named variable.
public static void set(String key, Object value);
// Returns current thread’s value for named variable.
public static Object get(String key);
}

//第一次重构
public class ThreadLocal {
private ThreadLocal() { } // Noninstantiable
public static class Key { Key() { } }
// Generates a unique, unforgeable key
public static Key getKey() { return new Key(); }
public static void set(Key key, Object value);
public static Object get(Key key);
}
//可以使用,但是需要引用代码
static ThreadLocal.Key serialNumberKey = ThreadLocal.getKey();
ThreadLocal.set(serialNumberKey, nextSerialNumber());
System.out.println(ThreadLocal.get(serialNumberKey));

//第二次重构
public class ThreadLocal {
public ThreadLocal() { }
public void set(Object value);
public Object get();
}

//剔除混乱,API使用起来很干净
static ThreadLocal serialNumber = new ThreadLocal();
serialNumber.set(nextSerialNumber());
System.out.println(serialNumber.get());

```




