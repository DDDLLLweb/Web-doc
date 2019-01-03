1,简介
    HashSet是一个没有重复元素的集合。
    它是由HashMap实现的，不保证元素的顺序，而且HashSet允许使用null元素。
    HashSet是非同步的，如果多个线程同时访问一个哈希 set，而其中至少一个线程修改了该 set，那么它必须 保持外部同步。这通常是通过对自然封装该 set 的对象执行同步操作来完成的。如果不存在这样的对象，则应该使用 Collections.synchronizedSet 方法来“包装” set。

      // 带集合的构造函数
 public HashSet(Collection<? extends E> c) {
// 创建map。
// 为什么要调用Math.max((int) (c.size()/.75f) + 1, 16)，从 (c.size()/.75f) + 1 和 16 中选择一个比较大的树呢？        
  // 首先，说明(c.size()/.75f) + 1
 //   因为从HashMap的效率(时间成本和空间成本)考虑，HashMap的加载因子是0.75。
       //   当HashMap的“阈值”(阈值=HashMap总的大小*加载因子) < “HashMap实际大小”时，
//   就需要将HashMap的容量翻倍。
  //   所以，(c.size()/.75f) + 1 计算出来的正好是总的空间大小。
 // 接下来，说明为什么是 16 。
  //   HashMap的总的大小，必须是2的指数倍。若创建HashMap时，指定的大小不是2的指数倍；
//   HashMap的构造函数中也会重新计算，找出比“指定大小”大的最小的2的指数倍的数。
 //   所以，这里指定为16是从性能考虑。避免重复计算。
  map = new HashMap<E,Object>(Math.max((int) (c.size()/.75f) + 1, 16));
     // 将集合(c)中的全部元素添加到HashSet中
    addAll(c);
  }

2:知识点
  - HashSet是Set接口的典型实现，HashSet按照Hash算法来存储集合中的元素的。
    - 不能保证元素的顺序，元素是无序的。
    - HashSet不是同步的，需要外部保持线程之间的同步问题
    - 集合元素值允许为null
  - HashSet的底层是通过HashMap实现的。而HashMap在1.7之前使用的是数组+链表实现，而1.8+使用的数组+链表+红黑树实现的。HashSet的底层实现和HashMap使用的是相同的方式，因为Map是无序的，因此HashSet也无法保证顺序
  - HashSet的方法，也是借助HashMap的方法来实现的