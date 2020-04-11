## LeetCode : Min Stack

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

* push(x) -- Push element x onto stack.
* pop() -- Removes the element on top of the stack.
* top() -- Get the top element.
* getMin() -- Retrieve the minimum element in the stack.
 

**Example:**

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
```


## Solve

定义两个数组分别作为：正常的栈以及递减排序的栈。

递减排序的栈：仅将最小的元素入栈，而栈的尾部一定是占中的最小的元素。

```c++
class MinStack {
    vector<int> min;
    vector<int> stack;
public:
    MinStack() {

    }

    void push(int x) {
        stack.push_back(x);
        if(min.empty() || min.back() >= x) {
            min.push_back(x);
        }
    }

    void pop() {
        if(stack.back() == min.back())
            min.pop_back();

        stack.pop_back();
    }

    int top() {
        if(!stack.empty()) {
            return stack.back();
        }
        return -1;
    }

    int getMin() {
        if(!min.empty()) {
            return min.back();
        }
        return -1;
    }
};
```


