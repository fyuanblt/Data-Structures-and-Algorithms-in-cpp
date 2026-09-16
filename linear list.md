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























































































































































































































































































