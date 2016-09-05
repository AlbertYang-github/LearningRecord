# Java集合框架
## 集合体系
**Java集合类主要由两个接口派生而出：Collection 和 Map**
![](http://i.imgur.com/lbVNhbZ.png)
![](http://i.imgur.com/t8Cs3tN.png)

## 集合三大类：List、Set、Map
- List集合是有序集合，集合中的元素可以重复，可以根据索引来访问集合中的元素
- Set集合是无序集合，集合中的元素不能重复，只能根据元素本身来访问集合中的元素
- Map集合以key-value的形式保存元素，只能根据key来访问集合中的元素

## 实现类概述
### List实现类
- ArrayList与Vector用法相似（底层都采用数组实现），ArrayList是线程不安全的，后者是线程安全的，故前者性能比后者好
- Vector提供了一个模拟"栈"数据结构的子类Stack
- LinkedList底层以链表的形式实现，因此插入和删除的性能比上述两者好
- 使用ArrayList或Vector时，如果知道需要保存多少个元素，可以直接使用ensureCapacity(int minCapacity)方法一次性增加initialCapacity，这样可以减少重分配的次数，从而提高性能

### Set实现类
- Set的三个实现类HashSet、TreeSet和EnumSet都是线程不安全的
- LinkedHashSet需要维护插入元素的顺序，所以性能比HashSet差
- TreeSet是SortedSet接口的实现类，它确保集合中的元素处于排序状态
- EnumSet中所有的元素都必须是指定枚举类型

### Map实现类
- HashMap是线程不安全的，Hashtable是线程安全的
- Map中的key值存储在Set集合中，因此不能重复
- SortedMap接口有一个实现类TreeMap，根据key对节点进行排序
- WeakHashMap与HashMap用法基本相似，HashMap的key保留了对实际对象的强引用，而WeakHashMap的key保留了对实际对象的弱引用