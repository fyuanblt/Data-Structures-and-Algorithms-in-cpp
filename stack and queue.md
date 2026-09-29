### 栈和队列

|  Part I   |   Part II   |
| :-------: | :---------: |
| [栈](#p1) | [队列](#p2) |

<span id="p1">**栈**</span>
入栈push（压栈）
出栈pop（弹栈）
上溢overflow（栈满的时候入栈）
下溢（对空栈出栈）
**1-顺序栈**
~~~cpp
#include <iostream>
#include <stdexcept>

// 代码3-2: 顺序栈的结构定义与初始化
template <typename T>
class SeqStack {
private:
    T* elem;       // 栈元素存储空间
    int top;       // 栈顶指针（实指，指向栈顶元素，初始为-1）
    int capacity;  // 栈的最大容量

public:
    // 对应 代码3-2: InitStack(stack, kSize)
    SeqStack(int kSize) {
        capacity = kSize;
        top = -1;
        elem = new T[capacity];
    }

    // 析构函数（对应 DestroyStack 抽象操作）
    ~SeqStack() {
        delete[] elem;
    }

    // 清空栈（对应 Clear 抽象操作）
    void Clear() {
        top = -1;
    }

    // 判空（对应 IsEmpty 抽象操作）
    bool IsEmpty() const {
        return top == -1;
    }

    // 判满（对应 IsFull 抽象操作）
    bool IsFull() const {
        return top == capacity - 1;
    }

    // 算法3-1: 顺序栈的入栈操作 Push(stack, x)
    void Push(const T& x) {
        if (IsFull()) {
            std::cerr << "栈满，入栈失败！" << std::endl;
            return; // 退出
        }
        top = top + 1;
        elem[top] = x;
    }

    // 算法3-2: 顺序栈的取顶操作 Top(stack)
    T Top() const {
        if (IsEmpty()) {
            std::cerr << "栈空，返回NIL" << std::endl;
            throw std::runtime_error("Stack is empty"); // 代替伪代码中的 NIL
        }
        return elem[top];
    }

    // 算法3-3: 顺序栈的出栈操作 Pop(stack)
    void Pop() {
        if (IsEmpty()) {
            std::cerr << "栈空，出栈失败！" << std::endl;
            return; // 退出
        }
        top = top - 1;
    }

    int Size() const { return top + 1; }
};
~~~
