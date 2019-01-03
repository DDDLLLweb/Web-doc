1.LinkedList简介
    LinkedList 是一个继承于AbstractSequentialList的双向链表。它也可以被当作堆栈、队列或双端队列进行操作。
    LinkedList 实现 List 接口，能对它进行队列操作。
    LinkedList 实现 Deque 接口，即能将LinkedList当作双端队列使用。
    LinkedList 实现了Cloneable接口，即覆盖了函数clone()，能克隆。
    LinkedList 实现java.io.Serializable接口，这意味着LinkedList支持序列化，能通过序列化去传输。
    LinkedList 是非同步的。

2,构造函数
    // 默认构造函数
    LinkedList()
    // 创建一个LinkedList，保护Collection中的全部元素。
    LinkedList(Collection<? extends E> collection)
3, LinkedList的本质是双向链表。
    LinkedList继承于AbstractSequentialList，并且实现了Dequeue接口。 
    LinkedList包含两个重要的成员：header 和 size。
    　　header: 双向链表的表头，它是双向链表节点所对应的类Entry的实例。
               Entry中包含成员变量： previous, next, element。
               其中，previous是该节点的上一个节点，next是该节点的下一个节点，element是该节点所包含的值。
    　　size: 双向链表中节点的个数。
4, Entry是双向链表节点所对应的数据结构
