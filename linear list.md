### 线性表

|      Part I       |     Part II     |    Part III     |     Part IV     |       Part V        |
| :---------------: | :-------------: | :-------------: | :-------------: | :-----------------: |
| [顺序线性表](#p1) | [单向链表](#p2) | [双向链表](#p3) | [循环链表](#p4) | [三个案例分析](#p5) |

|            |                            顺序表                            |                             链表                             |
| :--------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|  存储空间  |              预先分配，可能会导致空间闲置或溢出              |              动态分配，不会出现空间闲置或者溢出              |
|  存储密度  |       存储密度为1，逻辑关系等于存储关系，没有额外开销        |     存储密度小于1，要借助指针域来表示元素之间的逻辑关系      |
|  存取元素  |           随机存取，按位置访问元素的时间复杂度O(1)           |         顺序存取，访问某位置的元素的时间复杂度为O(n)         |
| 插入、删除 | 插入和删除都要移动大量的元素。平均移动元素约为表的一半。时间复杂度O(n) | 不需要移动元素，只需要改变指针位置，继而改变结点之间的链接关系。时间复杂度O(1) |
|  适用情况  | 1.表长变化不大，或者事先就能确定变化的范围<br />2.很少进行插入和删除，需要下标访问元素 |            1.长度变化较大<br />2.频繁的插入和删除            |
|            |                 vector               |                  list                |

<span id="p1">**==1. 顺序线性表==**</span>

*线性表的定义*
~~~cpp
struct Sqlist {
	int* data;
	int last;
};
~~~

*线性表的初始化*
~~~cpp
void InitList(Sqlist L) {
	L.data = new int[KMaxSize];
	L.last = -1;
}
~~~
*线性表的查找*
~~~cpp
nt Locate(Sqlist L, int e) {
	for (int i = 0; i <= L.last; i++) {
		if (L.data[i] == e) {
			return i;
		}
	}
	return -1;
}
~~~
*线性表的插入*
~~~cpp
bool Insert(Sqlist L, int i, int e) {
	if (i<1 || i>L.last + 2) {
		return false;
	}
	if (L.last + 1 > KMaxSize) {
		return false;
	}
	for (int j = L.last; j >= i - 1; j--) {
		L.data[j + 1] = L.data[j];
	}
	L.data[i - 1] = e;
	L.last++;
	return true;
}
~~~
*线性表的删除*
~~~cpp
bool Delete(Sqlist L, int i) {
	if (i<1 || i>L.last + 1) {
		return false;
		}
	for (int j = i; j < L.last; j++) {
		L.data[j - 1] = L.data[j];
	}
	L.last--;
	return true;
}
~~~
*线性表的取值*
~~~cpp
bool Getdata(const Sqlist& L, int i, int& e) {
	if (i<1 || i>L.last + 1) {
		return false;
	}
	e = L.data[i - 1];
	return true;
}
~~~
*线性表的清空*
~~~cpp
void Clear(Sqlist &L) {
	L.last = -1;
}
~~~
*线性表的销毁*
~~~cpp
void Destroy(Sqlist &L) {
	if (L.data != nullptr) {
		delete[]L.data;
		L.data = nullptr;
	}
	L.last = -1;
}
~~~



<span id="p2">**==单向链表==**</span>

*链表的定义与初始化*

~~~cpp
struct ListNode {
	int data;
	ListNode* next;
ListNode(int val): data(val), next(nullptr) {}
};
class LinkedList {
private:ListNode* head;

public: LinkedList() {
	head = nullptr;
}
	  ~LinkedList() {
		  ListNode* p = head;
		  while (p != nullptr) {
			  ListNode* temp = p;
			  p = p->next;
			  delete temp;
		  }// 析构函数：释放内存，防止内存泄漏
	  }//计算表长
	  int getLength() {
		  int counter = 0;        // 计数器
		  ListNode* p = head;     // 移动指针 p，初始化指向头指针

		  while (p != nullptr) {  // 当 p 不为空时
			  counter++;          // 计数器加 1
			  p = p->next;        // p 逐步往后移
		  }
		  return counter;         // 返回表长
	  }//按位查找
	  int getElem(int i) {
		  if (i < 1 || head == nullptr) {
			  return -1;
		  }
		  ListNode* p = head;
		  int counter = 1;
		  while (p != nullptr && counter < i) {
			  p = p->next;
			  counter++;
		  }
		  if (p != nullptr) {
			  return p->data;
		  }
		  else {
			  return -1; // 不存在第 i 个元素
		  }

	  }//按值查找
	  ListNode* locateElem(int x) {
		  ListNode* p = head;

		  // 遍历查找
		  while (p != nullptr && p->data != x) {
			  p = p->next;
		  }
		  return p;
	  }//插入
	  bool insert(int i, int x) {
		  // 1. 插入位置不合法
		  if (i < 1) {
			  return false;
		  }

		  // 2. 插入第 1 个结点（特殊情况：在头部插入）
		  if (i == 1) {
			  ListNode* newNode = new ListNode(x);
			  newNode->next = head; // 新节点的 next 指向原 head
			  head = newNode;       // head 更新为新节点
			  return true;
		  }

		  // 3. 查找第 i-1 个结点并插入其后 (i > 1 的情况)
		  ListNode* p = head;
		  int counter = 1;

		  // 教材逻辑：寻找第 i-1 个节点
		  while (p != nullptr && counter < (i - 1)) {
			  p = p->next;
			  counter++;
		  }

		  // 如果 p 指向第 i-1 个结点
		  if (p != nullptr) {
			  ListNode* newNode = new ListNode(x);
			  newNode->next = p->next; // 新节点的 next 指向原第 i 个节点
			  p->next = newNode;       // 第 i-1 个节点的 next 指向新节点
			  return true;
		  }
		  else {
			  return false;
		  }
	  }//删除
	  bool remove(int i) {
		  // 1. 删除位置不合法
		  if (i < 1) {
			  return false;
		  }

		  ListNode* p = head;

		  // 2. 删除第 1 个结点（特殊情况）
		  if (p != nullptr && i == 1) {
			  head = p->next; // head 指向第二个节点
			  delete p;       // 释放原头节点
			  return true;
		  }

		  // 3. 查找第 i-1 个结点 (i > 1 的情况)
		  int counter = 1;
		  while (p != nullptr && counter < (i - 1)) {
			  p = p->next;
			  counter++;
		  }

		  // 4. 执行删除操作
		  // 如果 p 指向第 i-1 个结点，且待删除结点存在 (p->next != nullptr)
		  if (p != nullptr && p->next != nullptr) {
			  ListNode* deleted_node = p->next; // 待删除节点
			  p->next = deleted_node->next;     // 跨过待删除节点
			  delete deleted_node;              // 释放内存
			  return true;
		  }
		  else {
			  return false;
		  }
	  }
};
~~~
<span id="p4">**双向链表**</span>

*双向链表的定义*


~~~cpp
// 1. 定义双向链表结点结构
struct DNode {
    int data;           // 数据域
    DNode* prior;       // 前驱指针 (指向前一个结点)
    DNode* next;        // 后继指针 (指向后一个结点)
};

// 2. 类型别名，方便使用
typedef DNode* DLinkList;

// 3. 初始化带头结点的空双向链表
bool InitDLinkList(DLinkList &L) {
    L = new DNode;      // 分配头结点内存
    if (L == nullptr) return false; // 内存分配失败
    
    // 关键：空双向链表的头结点的两个指针初始状态
    L->prior = nullptr; // 前驱置空
    L->next = nullptr;  // 后继置空
    return true;
}
<span id="p3">**循环链表**</span>

*循环链表的定义*

~~~cpp
typedef struct CLnode
{
    ElemType data;
    CLnode *next;
}*CircList;
~~~

*循环链表的初始化*

~~~cpp
void InitList(CircList &L)
{
    L = new CLnode;
    L->next = L;
}
~~~

==循环链表的基本操作和单链表基本上相同，唯一不同的是，由于循环链表的最后一个结点的next不再是空指针，而是指向头结点，因此，循环中的结束条件要发生变化==

~~~cpp
单链表--------------循环链表
while(p)--------->while(p!=L)
while(p->next)--->while(p->next!=L)
~~~
















































































































































































































































































