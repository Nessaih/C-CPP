
## 设计哈希集合
不使用任何内建的哈希表库设计一个哈希集合

具体地说，你的设计应该包含以下的功能

+ add(value)：向哈希集合中插入一个值。
+ contains(value) ：返回哈希集合中是否存在这个值。
+ remove(value)：将给定值从哈希集合中删除。如果哈希集合中没有这个值，什么也不做。

示例:
```C++
MyHashSet hashSet = new MyHashSet();
hashSet.add(1);         
hashSet.add(2);         
hashSet.contains(1);    // 返回 true
hashSet.contains(3);    // 返回 false (未找到)
hashSet.add(2);          
hashSet.contains(2);    // 返回 true
hashSet.remove(2);          
hashSet.contains(2);    // 返回  false (已经被删除)
```

**注意：**

+ 所有的值都在 [0, 1000000]的范围内。
+ 操作的总数目在[1, 10000]范围内。
+ 不要使用内建的哈希集合库。

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
