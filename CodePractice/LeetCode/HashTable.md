
## 设计哈希集合
不使用任何内建的哈希表库设计一个哈希集合

实现 MyHashSet 类：

+ add(value)：向哈希集合中插入一个值。
+ contains(value) ：返回哈希集合中是否存在这个值。
+ remove(value)：将给定值从哈希集合中删除。如果哈希集合中没有这个值，什么也不做。

示例:
```C++
输入：
["MyHashSet", "add", "add", "contains", "contains", "add", "contains", "remove", "contains"]
[[], [1], [2], [1], [3], [2], [2], [2], [2]]
输出：
[null, null, null, true, false, null, true, null, false]

解释：
MyHashSet myHashSet = new MyHashSet();
myHashSet.add(1);      // set = [1]
myHashSet.add(2);      // set = [1, 2]
myHashSet.contains(1); // 返回 True
myHashSet.contains(3); // 返回 False ，（未找到）
myHashSet.add(2);      // set = [1, 2]
myHashSet.contains(2); // 返回 True
myHashSet.remove(2);   // set = [1]
myHashSet.contains(2); // 返回 False ，（已移除）

```

**提示：**

+ 0 <= key <= 106
+ 最多调用 104 次 add、remove 和 contains 。

### 题解：
```C++

#include <iostream>


using namespace std;

class MyHashSet {
private:
#define HASHSIZE  101

	typedef struct hashnode {
		int key;
		struct hashnode *next;
	}HashNode;

	HashNode *hashtab[HASHSIZE];

	int hash(int  key)
	{
		key = key % HASHSIZE;
		return key;
	}

public:
	/** Initialize your data structure here. */
	MyHashSet() {
		for (int i = 0; i < HASHSIZE; ++i)
		{
			hashtab[i] = NULL;
		}
	}

	void add(int key)
	{
		int hashkey = hash(key);
		HashNode *p = hashtab[hashkey];

		while (p)
		{
			if (key == p->key)
				return;         //the key already exist
			else
				p = p->next;
		}
		if (p = (HashNode *)malloc(sizeof(HashNode)))
		{
			p->key = key;
			p->next = hashtab[hashkey];
			hashtab[hashkey] = p;
		}
	}

	void remove(int key)
	{
		int hashkey = hash(key);
		HashNode *p = hashtab[hashkey];
		HashNode *h = NULL;

		if (p)
		{
			if (key == p->key)
			{
				hashtab[hashkey] = p->next;
				free(p);
				p = NULL;			//Preventing the execution of while(p)
			}
			else
			{
				h = p;
				p = p->next;
			}
		}
		while (p)
		{
			if (key == p->key)
			{
				h->next = p->next;
				free(p);
				p = NULL;
			}
			else
			{
				h = p;
				p = p->next;
			}
		}
	}

	/** Returns true if this set contains the specified element */
	bool contains(int key)
	{
		int hashkey = hash(key);
		HashNode *p = hashtab[hashkey];

		while (p)
		{
			if (key == p->key)
				return true;
			else
				p = p->next;
		}
		return false;
	}
};

/**
 * Your MyHashSet object will be instantiated and called as such:
 * MyHashSet* obj = new MyHashSet();
 * obj->add(key);
 * obj->remove(key);
 * bool param_3 = obj->contains(key);
 */

int main()
{

	MyHashSet hashSet = MyHashSet();
	hashSet.add(1);
	hashSet.add(2);
	cout << hashSet.contains(1);  // 返回 true
	cout << hashSet.contains(3);  // 返回 false (未找到)
	hashSet.add(2);
	cout << hashSet.contains(2);  // 返回 true
	hashSet.remove(2);
	cout << hashSet.contains(2);  // 返回  false (已经被删除)
}
 
```


## 设计哈希映射

不使用任何内建的哈希表库设计一个哈希映射（HashMap）。

实现 MyHashMap 类：

+ MyHashMap() 用空映射初始化对象
+ void put(int key, int value) 向 HashMap 插入一个键值对 (key, value) 。如果 key 已经存在于映射中，则更新其对应的值 value 。
+ int get(int key) 返回特定的 key 所映射的 value ；如果映射中不包含 key 的映射，返回 -1 。
+ void remove(key) 如果映射中存在 key 的映射，则移除 key 和它所对应的 value 。

**示例：**
```C++
输入：
["MyHashMap", "put", "put", "get", "get", "put", "get", "remove", "get"]
[[], [1, 1], [2, 2], [1], [3], [2, 1], [2], [2], [2]]
输出：
[null, null, null, 1, -1, null, 1, null, -1]

解释：
MyHashMap myHashMap = new MyHashMap();
myHashMap.put(1, 1); // myHashMap 现在为 [[1,1]]
myHashMap.put(2, 2); // myHashMap 现在为 [[1,1], [2,2]]
myHashMap.get(1);    // 返回 1 ，myHashMap 现在为 [[1,1], [2,2]]
myHashMap.get(3);    // 返回 -1（未找到），myHashMap 现在为 [[1,1], [2,2]]
myHashMap.put(2, 1); // myHashMap 现在为 [[1,1], [2,1]]（更新已有的值）
myHashMap.get(2);    // 返回 1 ，myHashMap 现在为 [[1,1], [2,1]]
myHashMap.remove(2); // 删除键为 2 的数据，myHashMap 现在为 [[1,1]]
myHashMap.get(2);    // 返回 -1（未找到），myHashMap 现在为 [[1,1]]
```

**提示：**
+ 0 <= key, value <= 106
+ 最多调用 104 次 put、get 和 remove 方法
 
### 题解：
```C++

class MyHashMap {

private:
    #define HASHSIZE  101
    typedef struct hashnode {
        int key;
        int value;
        struct hashnode *next;
    }HashNode;

    HashNode *hashtab[HASHSIZE];

    int hash(int  key)
    {
        key = key % HASHSIZE;
        return key;
    }

public:
    /** Initialize your data structure here. */
    MyHashMap() {
        for (int i = 0; i < HASHSIZE; ++i)
        {
            hashtab[i] = NULL;
        }
    }
    
    /** value will always be non-negative. */
    void put(int key, int value) {
        int hashkey = hash(key);
        HashNode *p = hashtab[hashkey];

        while (p)
        {
            if (key == p->key)  //the key already exist,update the value
            {
                p->value = value;
                return ;
            }         
            else
            {
                p = p->next;
            }
        }
        //the key not exist,create a new node
        if (p = (HashNode *)malloc(sizeof(HashNode)))
        {
            p->key = key;
            p->value = value;
            p->next = hashtab[hashkey];
            hashtab[hashkey] = p;
        }
    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        int hashkey = hash(key);
        HashNode *p = hashtab[hashkey];

        while (p)
        {
            if (key == p->key)
                return p->value;
            else
                p = p->next;
        }
        return -1;
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        int hashkey = hash(key);
        HashNode *p = hashtab[hashkey];
        HashNode *h = NULL;

        if (p)
        {
            if (key == p->key)
            {
                hashtab[hashkey] = p->next;
                free(p);
                p = NULL;           //Preventing the execution of while(p)
            }
            else
            {
                h = p;
                p = p->next;
            }
        }
        while (p)
        {
            if (key == p->key)
            {
                h->next = p->next;
                free(p);
                p = NULL;
            }
            else
            {
                h = p;
                p = p->next;
            }
        }
    }
};



```
