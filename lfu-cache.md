## LeetCode每日一题：LFU缓存

设计并实现[最不经常使用（LFU）](https://baike.baidu.com/item/%E7%BC%93%E5%AD%98%E7%AE%97%E6%B3%95)缓存的数据结构。它应该支持以下操作：`get` 和 `put`。

`get(key)` - 如果键存在于缓存中，则获取键的值（总是正数），否则返回 -1。
`put(key, value)` - 如果键不存在，请设置或插入值。当缓存达到其容量时，它应该在插入新项目之前，使最不经常使用的项目无效。在此问题中，当存在平局（即两个或更多个键具有相同使用频率）时，最近最少使用的键将被去除。

**进阶：**

你是否可以在 **O(1)** 时间复杂度内执行两项操作？

**示例：**

```
LFUCache cache = new LFUCache( 2 /* capacity (缓存容量) */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // 返回 1
cache.put(3, 3);    // 去除 key 2
cache.get(2);       // 返回 -1 (未找到key 2)
cache.get(3);       // 返回 3
cache.put(4, 4);    // 去除 key 1
cache.get(1);       // 返回 -1 (未找到 key 1)
cache.get(3);       // 返回 3
cache.get(4);       // 返回 4
```



## Solve

**LFU缓存：**这个缓存算法使用一个计数器来记录条目被访问的频率。通过使用LFU缓存算法，最低访问数的条目首先被移除。

#### 1.双向链表 + 哈希表

记录大佬的做法。学习了，学习了。

```c++
class LFUCache {
private:
    // 双向链表节点
    struct DLNode {
        int key;
        int value;
        int freq; // 访问频率
        DLNode* prev;
        DLNode* next;
        DLNode() : key(-1), value(-1), freq(0), prev(NULL), next(NULL) {};
        DLNode(int k, int v, int f) : key(k), value(v), freq(f), prev(NULL), next(NULL) {};
    };
    // 双向链表
    struct DLList {
        DLNode* head;
        DLNode* tail;
        int size;
        DLList() {
            head = new DLNode;
            tail = new DLNode;
            head->next = tail;
            tail->prev = head;
            size = 0;
        }
        // 删除结点
        void remove(DLNode* node) {
            node->next->prev = node->prev;
            node->prev->next = node->next;
            --size;
        }
        // 访问链尾结点，并删除
        DLNode* pop_tail() {
            auto node = tail->prev;
            remove(node);
            return node;
        }
        // 头插法添加链表结点
        void add_front(DLNode* node) {
            node->prev = head;
            node->next = head->next;
            node->prev->next = node;
            node->next->prev = node;
            ++size;
        }
        ~DLList() {
            size = 0;
            delete head;
            delete tail;
        }
    };
    int capacity;
    int size;
    map<int, DLList> freqDict; // 记录频率到双向链表的映射
    map<int, DLNode*> cache;  // 记录key到双向链表节点的映射
    
    // 访问时，修改频率对链表的映射关系
    void incr(DLNode* node) {
        freqDict[node->freq].remove(node);
        if ((freqDict[node->freq].size) == 0) {
            freqDict.erase(node->freq);
        }
        ++node->freq;
        freqDict[node->freq].add_front(node);
    }
    
    void add(DLNode* node) {
        cache[node->key] = node;
        freqDict[node->freq].add_front(node);
        ++size;
    }
    
    void pop() {
        if (freqDict.empty()) return;
        auto it = freqDict.begin();
        auto node = it->second.pop_tail();
        if (it->second.size == 0) {
            freqDict.erase(it);
        }
        cache.erase(node->key);
        delete node;
        --size;
    }

public:    
    LFUCache(int capacity) {
        this->capacity = capacity;
        this->size = 0;
    }
    
    int get(int key) {
        if (cache.count(key) == 0) return -1;
        auto node = cache[key];
        incr(node);
        return node->value;
    }
    
    void put(int key, int value) {
        if (capacity == 0) return;
        if (cache.count(key) == 0) {
            if (size == capacity) pop();
            auto node = new DLNode(key, value, 1);
            add(node);
        } else {
            auto node = cache[key];
            node->value = value;
            incr(node);
        }
    }
};

/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache* obj = new LFUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */

作者：da-li-wang
链接：https://leetcode-cn.com/problems/lfu-cache/solution/c-shuang-xiang-lian-biao-ha-xi-by-da-li-wang/
```

#### 2.哈希表 + 二叉树

```c++
struct Node {
    int cnt, time, key, value;

    Node(int _cnt, int _time, int _key, int _value):cnt(_cnt), time(_time), key(_key), value(_value){}
    
    bool operator < (const Node& rhs) const {
        return cnt == rhs.cnt ? time < rhs.time : cnt < rhs.cnt;
    }
};
class LFUCache {
    // 缓存容量，时间戳
    int capacity, time;
    unordered_map<int, Node> key_table;
    set<Node> S;
public:
    LFUCache(int _capacity) {
        capacity = _capacity;
        time = 0;
        key_table.clear();
        S.clear();
    }
    
    int get(int key) {
        if (capacity == 0) return -1;
        auto it = key_table.find(key);
        // 如果哈希表中没有键 key，返回 -1
        if (it == key_table.end()) return -1;
        // 从哈希表中得到旧的缓存
        Node cache = it -> second;
        // 从平衡二叉树中删除旧的缓存
        S.erase(cache);
        // 将旧缓存更新
        cache.cnt += 1;
        cache.time = ++time;
        // 将新缓存重新放入哈希表和平衡二叉树中
        S.insert(cache);
        it -> second = cache;
        return cache.value;
    }
    
    void put(int key, int value) {
        if (capacity == 0) return;
        auto it = key_table.find(key);
        if (it == key_table.end()) {
            // 如果到达缓存容量上限
            if (key_table.size() == capacity) {
                // 从哈希表和平衡二叉树中删除最近最少使用的缓存
                key_table.erase(S.begin() -> key);
                S.erase(S.begin());
            }
            // 创建新的缓存
            Node cache = Node(1, ++time, key, value);
            // 将新缓存放入哈希表和平衡二叉树中
            key_table.insert(make_pair(key, cache));
            S.insert(cache);
        }
        else {
            // 这里和 get() 函数类似
            Node cache = it -> second;
            S.erase(cache);
            cache.cnt += 1;
            cache.time = ++time;
            cache.value = value;
            S.insert(cache);
            it -> second = cache;
        }
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/lfu-cache/solution/lfuhuan-cun-by-leetcode-solution/
```

<div align="center">
    <hr style="height:1px;"/>
    <br>
    <img width="200px" src="https://github.com/RunCoderHang/LeetCode-Notes/blob/master/image/wxgzh-hang.png"></img>
</div>