---
title: LRU 缓存与 LinkedHashMap
comments: true
categories: [Java]
tags: [Java, 算法]
updated: 2021-08-26 16:46:38
cover: https://ruafafa-photobed.oss-cn-beijing.aliyuncs.com/illust_111121482_20230925_001424.jpg
---
最近在重温 [146. LRU 缓存](https://leetcode.cn/problems/lru-cache/solutions/) 这道题时，想看看Java标准库中有没有相关的实现，于是发现了
LinkedHashMap。

#### 什么是 LRU 缓存？

LRU 缓存是一种缓存淘汰策略，当缓存满了之后，再有新的数据需要加入缓存时，它会把最近最少使用的数据移除。这种策略通过保持最近使用的数据，保证了高频数据的快速访问。

#### LinkedHashMap 简介

`LinkedHashMap` 是 Java 集合框架中的一个类，它继承自 `HashMap`，并且在保持键值对无序的 `HashMap` 基础上，增加了按插入顺序或访问顺序排列的功能。这使得 `LinkedHashMap` 可以高效地实现 LRU 缓存。
官方有关该类的文档是这么写的：
![](https://ruafafa-photobed.oss-cn-beijing.aliyuncs.com/202407051316952.png)

其中的构造方法是这个 ——>
**指定初始容量、负载因子和排序模式的构造方法**：
 ```java
 LinkedHashMap<K, V> map = new LinkedHashMap<>(int initialCapacity, float loadFactor, boolean accessOrder);
 ```
那么LinkedHashMap在更新元素时都做了什么呢？
我们找到 removeEldestEntry 方法
![](https://ruafafa-photobed.oss-cn-beijing.aliyuncs.com/202407051319683.png)
然后搜索发现这个方法被 afterNodeInsertion 调用
![](https://ruafafa-photobed.oss-cn-beijing.aliyuncs.com/202407051320867.png)
afterNodeInsertion 方法重写了父类 HashMap 的 afterNodeInsertion 方法
HashMap 中的 afterNodeInsertion 为空实现，LinkedHashMap 重写了这个方法，用于实现 LRU 缓存的逻辑。
那么 HashMap 中的 afterNodeInsertion 在哪里实现呢？在这里
![](https://ruafafa-photobed.oss-cn-beijing.aliyuncs.com/202407051329366.png)
这样就找到了 LinkedHashMap 中实现 LRU 缓存的流程。

#### 使用 LinkedHashMap 实现 LRU 缓存


我们可以先学学其他官方项目中使用 LinkedHashMap 做缓存的实现
![](https://ruafafa-photobed.oss-cn-beijing.aliyuncs.com/202407051332344.png)

我们可以通过继承 `LinkedHashMap` 并覆盖其 `removeEldestEntry` 方法，来实现一个 LRU 缓存。下面是完整的示例代码：

```java
import java.util.LinkedHashMap;
import java.util.Map;

public class LRUCache<K, V> extends LinkedHashMap<K, V> {
    private final int capacity;

    public LRUCache(int capacity) {
        super(capacity, 0.75f, true);
        this.capacity = capacity;
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        return size() > capacity;
    }

    public static void main(String[] args) {
        LRUCache<Integer, Integer> cache = new LRUCache<>(2);
        cache.put(1, 1);
        cache.put(2, 2);
        System.out.println(cache.get(1)); // 输出 1
        cache.put(3, 3);                  // 移除键 2
        System.out.println(cache.get(2)); // 输出 -1 (未找到)
        cache.put(4, 4);                  // 移除键 1
        System.out.println(cache.get(1)); // 输出 -1 (未找到)
        System.out.println(cache.get(3)); // 输出 3
        System.out.println(cache.get(4)); // 输出 4
    }
}
```

```java
class LRUCache {

    // 使用 LinkedHashMap 实现 LRU 缓存, K - Integer | V - Integer
    private LinkedHashMap<Integer, Integer> cache;

    public LRUCache(int capacity) {
        cache = new LinkedHashMap<>(capacity, 0.75F, true) {
              protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
                return this.size() > capacity;
            }
        };
    }

    public int get(int key) {
        Integer v = cache.get(key);
        return v == null ? -1 : v;
    }

    public void put(int key, int value) {
        cache.put(key, value);
    }
}
```

#### 总结

`LinkedHashMap` 提供了一种简单而高效的方式来实现 LRU 缓存。通过覆盖 `removeEldestEntry` 方法，我们可以控制缓存的容量，并在缓存达到上限时自动移除最老的数据。希望这篇文章能够帮助大家更好地理解和使用 `LinkedHashMap` 来实现 LRU 缓存。