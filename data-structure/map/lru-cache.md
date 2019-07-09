---
description: Leetcode 146
---

# LRU Cache

Design and implement a data structure for [Least Recently Used \(LRU\) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU). It should support the following operations: `get` and `put`.

`get(key)` - Get the value \(will always be positive\) of the key if the key exists in the cache, otherwise return -1.  
 `put(key, value)` - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

The cache is initialized with a **positive** capacity.

**Follow up:**  
 Could you do both operations in **O\(1\)** time complexity?

**Example:**

```text
LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```

思路：

这道题作为一道HashMap的经典题，可以学习如何实现LinkedHashMap来达到LRU的功能。LinkedHashMap的实现是由HashMap和DoubleLinkedList组成的。

具体原理：[https://yikun.github.io/2015/04/02/Java-LinkedHashMap工作原理及实现/](https://yikun.github.io/2015/04/02/Java-LinkedHashMap工作原理及实现/)

这道题的难点就在于代码长，容易出BUG。需要提前构思好框架，设计使用的函数。  
DoubleLinkedList需要先实现addNode和removeNode，然后才能实现moveToHead，以及popTail。  
set函数的实现易错。代码如下，将易错的地方标准出来

```java
public class LRUCache {
    
    // 这里必须要设置key, value
    class DLinkedNode{
        int key;
        int val;
        DLinkedNode prev;
        DLinkedNode next;
    }
    
    public void addNewNode(DLinkedNode node){ 
        //make sure head.next is the last step to be assigned.
        head.next.prev = node;
        node.next = head.next;
        head.next = node;
        node.prev = head;
    }
    
    public void removeNode(DLinkedNode node){
        node.next.prev = node.prev;
        node.prev.next = node.next;
        node.next = null;
        node.prev = null;
    }
    
    public void moveToHead(DLinkedNode node){
        // move the node to the head
        removeNode(node);
        addNewNode(node);
    }
    
    // 这里需要返回末尾的值，是因为需要末尾的key，来删除map中的entry
    public DLinkedNode popTail(){
        DLinkedNode res = tail.prev;
        removeNode(res);
        return res;
    }
    
    // 这里要注意value是node
    HashMap<Integer, DLinkedNode> map = new HashMap<>();
    DLinkedNode head = new DLinkedNode();
    DLinkedNode tail = new DLinkedNode();
    int capacity;
    /*
    * @param capacity: An integer
    */public LRUCache(int capacity) {
        // do intialization if necessary
        this.capacity = capacity;
        head.next = tail;
        tail.prev = head;
    }

    /*
     * @param key: An integer
     * @return: An integer
     */
    public int get(int key) {
        if(map.containsKey(key)){
            DLinkedNode node = map.get(key);
            moveToHead(node);
            return node.val;
        }else{
            return -1;
        }
    }

    /*
     * @param key: An integer
     * @param value: An integer
     * @return: nothing
     */
    public void set(int key, int value) {
        // remove the least recently used node
        if(map.containsKey(key)){
            DLinkedNode node = map.get(key);
            node.val = value;
            moveToHead(node);
        } else{
            DLinkedNode node = new DLinkedNode();
            node.key = key;
            node.val = value;
            map.put(key, node);
            addNewNode(node);
            // 这里需要注意删除的顺序，是先放完值之后再删除，不然的可能将新更新的值提前删掉
            if(this.capacity < map.size()){
                DLinkedNode tail = popTail();
                map.remove(tail.key);
            }
        }
    }
}
```



