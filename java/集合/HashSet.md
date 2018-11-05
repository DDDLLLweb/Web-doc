1,简介
    HashSet是一个没有重复元素的集合。
    它是由HashMap实现的，不保证元素的顺序，而且HashSet允许使用null元素。
    HashSet是非同步的，如果多个线程同时访问一个哈希 set，而其中至少一个线程修改了该 set，那么它必须 保持外部同步。这通常是通过对自然封装该 set 的对象执行同步操作来完成的。如果不存在这样的对象，则应该使用 Collections.synchronizedSet 方法来“包装” set。