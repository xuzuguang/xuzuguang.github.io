# LeetCode 4-24

#### [748. 最短完整词](https://leetcode-cn.com/problems/shortest-completing-word/)

如果单词列表（`words`）中的一个单词包含牌照（`licensePlate`）中所有的字母，那么我们称之为完整词。在所有完整词中，最短的单词我们称之为最短完整词。

单词在匹配牌照中的字母时不区分大小写，比如牌照中的 `"P"` 依然可以匹配单词中的 `"p"` 字母。

我们保证一定存在一个最短完整词。当有多个单词都符合最短完整词的匹配条件时取单词列表中最靠前的一个。

牌照中可能包含多个相同的字符，比如说：对于牌照 `"PP"`，单词 `"pair"` 无法匹配，但是 `"supper"` 可以匹配。

 

**示例 1：**

```
输入：licensePlate = "1s3 PSt", words = ["step", "steps", "stripe", "stepple"]
输出："steps"
说明：最短完整词应该包括 "s"、"p"、"s" 以及 "t"。对于 "step" 它只包含一个 "s" 所以它不符合条件。同时在匹配过程中我们忽略牌照中的大小写。
```

 

**示例 2：**

```
输入：licensePlate = "1s3 456", words = ["looks", "pest", "stew", "show"]
输出："pest"
说明：存在 3 个包含字母 "s" 且有着最短长度的完整词，但我们返回最先出现的完整词。
```



**代码：**

```c++
bool bComplete(string word , unordered_map<char , int> unMap){
    // 判断单个字符串是否包含hashmap中所有数据
    // 遍历数组 ，如果字符在hashmap中， --
    for(auto i : word){
        if(i >= 'A' &&  i <= 'Z' && unMap.find(tolower(i)) != unMap.end())
            unMap[tolower(i)] -= 1;
        else if(i >= 'a' &&  i <= 'z' && unMap.find(i) != unMap.end())
            unMap[i] -= 1;
    }
    // 遍历hashmap ， map中值 如果 >0 则false
    for(auto i:unMap){
        if(i.second > 0 )
            return false;
    }
    return true;
}
string shortestCompletingWord(string licensePlate, vector<string>& words) {
    unordered_map<char , int> unMap;
    string res ;
    for(char i :licensePlate){
        if(i >= 'A' &&  i <= 'Z')
            unMap[tolower(i)] += 1;
        else if(i >= 'a' && i <= 'z')
            unMap[i] += 1;
    }
    for(auto i :words){
        if(!bComplete(i , unMap)){
            continue;
        }
        if(res.empty() || res.size() > i.size())
            res = i;
    }
    return res;
}
```

#### [771. 宝石与石头](https://leetcode-cn.com/problems/jewels-and-stones/)

 给定字符串`J` 代表石头中宝石的类型，和字符串 `S`代表你拥有的石头。 `S` 中每个字符代表了一种你拥有的石头的类型，你想知道你拥有的石头中有多少是宝石。

`J` 中的字母不重复，`J` 和 `S`中的所有字符都是字母。字母区分大小写，因此`"a"`和`"A"`是不同类型的石头。

**示例 1:**

```
输入: J = "aA", S = "aAAbbbb"
输出: 3
```

**示例 2:**

```
输入: J = "z", S = "ZZ"
输出: 0
```

**代码:**

```c++
int numJewelsInStones(string J, string S) {
    int res = 0 ; 
    unordered_map<char , int> unMap;
    for(char i : J){
        unMap[i] = 1;
    }
    for(char i : S){
        if(unMap.find(i) != unMap.end())
            res++;
    }
    return res;
}
```

#### [705. 设计哈希集合](https://leetcode-cn.com/problems/design-hashset/)

不使用任何内建的哈希表库设计一个哈希集合

具体地说，你的设计应该包含以下的功能

- `add(value)`：向哈希集合中插入一个值。
- `contains(value)` ：返回哈希集合中是否存在这个值。
- `remove(value)`：将给定值从哈希集合中删除。如果哈希集合中没有这个值，什么也不做。


**示例:**

```
MyHashSet hashSet = new MyHashSet();
hashSet.add(1);         
hashSet.add(2);         
hashSet.contains(1);    // 返回 true
hashSet.contains(3);    // 返回 false (未找到)
hashSet.add(2);          
hashSet.contains(2);    // 返回 true
hashSet.remove(2);          
hashSet.contains(2);    // 返回  false (已经被删除)
```


**注意：**

- 所有的值都在 `[0, 1000000]`的范围内。
- 操作的总数目在`[1, 10000]`范围内。
- 不要使用内建的哈希集合库。

**代码:**

```c++
class MyHashSet {
    const int HASH_LEN = 100;
    struct ListNode{ // 链表
        int val;
        ListNode *next;
        ListNode(int x) : val(x), next(NULL) {}
    };
public:
    vector<ListNode*> m_List;
    /** Initialize your data structure here. */
    MyHashSet() {
        m_List.resize(HASH_LEN);
        for(int i = 0 ; i < HASH_LEN ; i++ )
            m_List[i] = new ListNode(-1);
    }
    
    void add(int key) {
        int haval = key % HASH_LEN;
        ListNode *tmp = m_List[haval];
        if(tmp->val == -1){// 空的
            tmp->val = key;
            return;
        }
        while(tmp){
            if(tmp->val == key)
                return;
            if(!(tmp->next)){
                ListNode * node = new ListNode(key);
                tmp->next = node;
                return;
            }
            tmp = tmp->next;
        }
    }
    
    void remove(int key) {
        int haval = key % HASH_LEN;
        ListNode *tmp = m_List[haval];
        if(tmp->val == -1)
            return;
        ListNode* preTmp = NULL;
        while(tmp){
            if(tmp->val == key){
                if(preTmp != NULL)
                    preTmp->next = tmp->next;
                else{
                    if(tmp->next == NULL){
                        ListNode * node = new ListNode(-1);
                        m_List[haval] = node;
                    }else
                        m_List[haval] = tmp->next;
                }
                delete tmp;
                tmp = NULL;
                return;
            }
            preTmp = tmp;
            tmp = tmp->next;
        }
    }
    
    /** Returns true if this set contains the specified element */
    bool contains(int key) {
        int haval = key % HASH_LEN;
        ListNode *tmp = m_List[haval];
        if(tmp->val == -1){
            return false;
        }
        while(tmp){
            if(tmp->val == key) 
                return true;
            tmp = tmp->next;
        }
        return false;
    }
};
```

#### [706. 设计哈希映射](https://leetcode-cn.com/problems/design-hashmap/)

不使用任何内建的哈希表库设计一个哈希映射

具体地说，你的设计应该包含以下的功能

- `put(key, value)`：向哈希映射中插入(键,值)的数值对。如果键对应的值已经存在，更新这个值。
- `get(key)`：返回给定的键所对应的值，如果映射中不包含这个键，返回-1。
- `remove(key)`：如果映射中存在这个键，删除这个数值对。


**示例：**

```
MyHashMap hashMap = new MyHashMap();
hashMap.put(1, 1);          
hashMap.put(2, 2);         
hashMap.get(1);            // 返回 1
hashMap.get(3);            // 返回 -1 (未找到)
hashMap.put(2, 1);         // 更新已有的值
hashMap.get(2);            // 返回 1 
hashMap.remove(2);         // 删除键为2的数据
hashMap.get(2);            // 返回 -1 (未找到) 
```

**代码：**

```c++


class MyHashMap {
    struct ListNode{
        int key;
        int value ;
        ListNode* next;
        ListNode(int n , int m):key(n),value(m),next(NULL){}
    };
    const int HASH_LEN = 100;
public:
    vector<ListNode*> m_vec;
    /** Initialize your data structure here. */
    MyHashMap() {
        m_vec.resize(HASH_LEN);
        for(int i = 0 ; i < HASH_LEN ; i++)
            m_vec[i] = new ListNode(-1 , -1);    
    }
    
    /** value will always be non-negative. */
    void put(int key, int value) {
        int n = key%HASH_LEN;
        ListNode* tmp = m_vec[n];
        if(tmp->key == -1){
            tmp->key = key ;
            tmp->value = value;
            return;
        }
        while(tmp){
            if(tmp->key == key){
                tmp->value = value;
                return;
            }
            if(tmp->next == NULL){
                ListNode* node = new ListNode(key ,value);
                tmp->next = node;
                return;
            }
            tmp = tmp->next;
        }
    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int n = key%HASH_LEN;
        ListNode* tmp = m_vec[n];
        if(tmp->key == -1)
            return -1;
        while(tmp){
            if(tmp->key == key)
                return tmp->value;
            tmp = tmp->next;
        }
        return -1;
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int n = key%HASH_LEN;
        ListNode* tmp = m_vec[n];
        if(tmp->key == -1)
            return;
        ListNode* preNode = NULL;
        while(tmp){
            if(tmp->key == key){
                if(preNode == NULL){
                    if(tmp->next == NULL)
                        m_vec[n] = new ListNode(-1 , -1);
                    else
                        m_vec[n] = tmp->next;
                }else{
                    preNode->next = tmp->next;
                }
                delete tmp;
                tmp = NULL;
                return;
            }
            preNode = tmp;
            tmp = tmp->next;
        }
    }
};
```