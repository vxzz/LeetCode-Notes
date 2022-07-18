# 数组

## 数组理论基础

数组是存放在连续内存空间上的相同类型数据的集合。

## 数组经典题目

### 1、**二分法**

 [704.二分查找](https://leetcode-cn.com/problems/binary-search/)（2022.5.25）

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

示例 1:

```
输入: nums = [-1,0,3,5,9,12], target = 9     
输出: 4       
解释: 9 出现在 nums 中并且下标为 4     
```

示例 2:

```
输入: nums = [-1,0,3,5,9,12], target = 2     
输出: -1        
解释: 2 不存在 nums 中因此返回 -1        
```

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int begin = 0;
        int end = nums.size() - 1;
        while(begin <= end){
            int middle = begin + (end - begin) / 2;
            if(nums[middle] > target){
                end = middle - 1;
            }else if(nums[middle] < target){
                begin = middle + 1;
            }else{
                return middle;
            }   
        }
        return -1;
    }
};
```

- 暴力解法时间复杂度：O(n)
- 二分法时间复杂度：O(logn)

二分法是算法面试中的常考题，在这道题目中我们使用**循环不变量原则**，只有在循环中坚持对区间的定义，才能清楚的把握循环中的各种细节。



### 2、双指针法

#### 2.1、移除元素

[27. 移除元素](https://leetcode.cn/problems/remove-element/)（2022.5.25）

给你一个数组nums和一个值val，你需要 原地 移除所有数值等于val的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并原地修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

示例 1：

```
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2]
解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = 		[2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。
```

示例 2：

```
输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,4,0,3]
解释：函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。注意这五个元素可为任意顺序。你不需要考虑数组中超出新长度后面的元素。
```

```cpp
//双指针法
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slowIndex = 0;
        for(int fastIndex = 0; fastIndexfastIndex < nums.size(); ++fastIndex){
            if(nums[fastIndex] != val){
                nums[slowIndex++] = nums[fastIndex];
            }
        }
        return slowIndex;
    }
};
```

#### 2.2、有序数组

[977.有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array)（2022.5.25）

给你一个按非递减顺序排序的整数数组nums，返回每个数字的平方组成的新数组，要求也按非递减顺序排序。

示例 1：

```
输入：nums = [-4,-1,0,3,10]
输出：[0,1,9,16,100]
解释：平方后，数组变为 [16,1,0,9,100]
排序后，数组变为 [0,1,9,16,100]
```

示例 2：

```
输入：nums = [-7,-3,2,3,11]
输出：[4,9,9,49,121]
```

```cpp
//双指针法
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int k = nums.size() - 1;
        vector<int> result(nums.size(), 0);
        for(int i = 0, j = k; i <= j;){
            if((nums[i] * nums[i]) < (nums[j] * nums[j])){
                result[k--] = nums[j] * nums[j];
                j--;
            }else{
                result[k--] = nums[i] * nums[i];
                i++;
            }
        }
        return result;
    }
};
```

- 暴力解法时间复杂度：O(n^2)
- 双指针时间复杂度：O(n)

数组中的元素不能删除的原因：

- 数组在内存中是连续的地址空间，不能释放单一元素，如果要释放，就是全释放（程序运行结束，回收内存栈空间）。
- C++中vector和array的区别一定要弄清楚，vector的底层实现是array，封装后使用更友好。

**双指针法（快慢指针法）在数组和链表的操作中是非常常见的，很多考察数组和链表操作的面试题，都使用双指针法。**



### 3、滑动窗口

[209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)（2022.5.27）

给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

示例 1：

```
输入：target = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```

示例 2：

```
输入：target = 4, nums = [1,4,4]
输出：1
```


示例 3：

```
输入：target = 11, nums = [1,1,1,1,1,1,1,1]
输出：0
```

```cpp
//滑动窗口法
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int i = 0;
        int result = INT_MAX;
        int len = 0;
        int sum = 0;
        for(int j = 0; j < nums.size(); ++j){
            sum += nums[j];
            while(sum >= target){
                len = j - i + 1;
                result = result < len ? result : len;
                sum -= nums[i++];
            }
        }
        return result == INT_MAX ? 0 : result;
    }
};
```

- 暴力解法时间复杂度：O(n^2)
- 滑动窗口时间复杂度：O(n)

本题中，主要要理解滑动窗口如何移动窗口起始位置，达到动态更新窗口大小的，从而得出长度最小的符合条件的长度。

**滑动窗口的精妙之处在于根据当前子序列和大小的情况，不断调节子序列的起始位置。从而将O(n^2)的暴力解法降为O(n)。**



### 4、模拟行为

[59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)（2022.5.27）

给你一个正整数 n ，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的 n x n 正方形矩阵 matrix 。

示例 1：

![螺旋矩阵](D:\xzz\LeetCode\Array\螺旋矩阵.jpg)

```
输入：n = 3
输出：[[1,2,3],[8,9,4],[7,6,5]]
```


示例 2：

```
输入：n = 1
输出：[[1]]
```

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        int t = 0;      // top
        int b = n - 1;    // bottom
        int l = 0;      // left
        int r = n - 1;    // right
        vector<vector<int>> ans(n, vector<int>(n));
        int k = 1;
        while(k <= n * n){
            for(int i = l; i <= r; ++i) ans[t][i] = k++;
            ++t;
            for(int i = t; i <= b; ++i) ans[i][r] = k++;
            --r;
            for(int i = r; i >= l; --i) ans[b][i] = k++;
            --b;
            for(int i = b; i >= t; --i) ans[i][l] = k++;
            ++l;
        }
        return ans;
    }
};
```

模拟类的题目在数组中很常见，不涉及到什么算法，就是单纯的模拟，十分考察大家对代码的掌控能力。

在这道题目中，我们再一次用到了**循环不变量原则**，其实这也是写程序中的重要原则。

相信大家有遇到过这种情况： 感觉题目的边界条件超多，一波接着一波的判断，找边界，踩了东墙补西墙，好不容易运行通过了，代码写的十分冗余，毫无章法，其实**真正解决题目的代码都是简洁的，或者有原则性的。**



![数组总结](D:\xzz\LeetCode\Array\数组总结.png)



# 链表

## 链表理论基础

（2022.5.28）

链表是一种通过指针串联在一起的线性结构，每一个节点由两部分组成，一个是数据域一个是指针域（存放指向下一个节点的指针），最后一个节点的指针域指向null（空指针）。

链表的入口节点称为链表的头结点也就是head。

### 链表的类型

#### 单链表

单链表中的节点只能指向节点的下一个节点。

#### 双链表

每一个节点有两个指针域，一个指向下一个节点，一个指向上一个节点。

双链表既可以向前查询也可以向后查询。

#### 循环链表

链表首尾相连，可以用来解决约瑟夫环问题。

### 链表的存储方式

和数组不同，链表是通过指针域的指针链接在内存中各个节点。

所以链表中的节点在内存中不是连续分布的 ，而是散乱分布在内存中的某地址上，分配机制取决于操作系统的内存管理。

### 链表的定义

如下所示：

```cpp
// 单链表
struct ListNode {
    int val;  // 节点上存储的元素
    ListNode *next;  // 指向下一个节点的指针
    ListNode(int x) : val(x), next(NULL) {}  // 节点的构造函数
};
```

不定义构造函数也可以，**C++默认生成一个构造函数。**

但是这个构造函数不会初始化任何成员变量，下面举两个例子：

通过自己定义构造函数初始化节点：

```cpp
ListNode* head = new ListNode(5);
```

使用默认构造函数初始化节点：

```cpp
ListNode* head = new ListNode();
head->val = 5;
```

所以如果不定义构造函数而使用默认构造函数的话，**在初始化的时候就不能直接给变量赋值！**

### 链表的操作

#### 删除节点

![](D:\xzz\LeetCode\List\删除节点.png)

只要将C节点的next指针指向E节点就可以了。

在C++里最好再手动释放D节点，释放这块内存。

其他语言例如Java、Python，有自己的内存回收机制，就不用自己手动释放了。

#### 添加节点

![](D:\xzz\LeetCode\List\添加结点.png)

链表的增添和删除都是O(1)操作，也不会影响到其他节点。

但是要注意，要是删除第五个节点，需要从头节点查找到第四个节点通过next指针进行删除操作，查找的时间复杂度是O(n)。

### 性能分析

把链表的特性和数组的特性进行一个对比，如图所示：

![List](D:\xzz\LeetCode\List\List.png)

数组在定义的时候，长度就是固定的，如果想改动数组的长度，就需要重新定义一个新的数组。

链表的长度可以是不固定的，并且可以动态增删， 适合数据量不固定，频繁增删，较少查询的场景。

## 链表经典题目

### 1、虚拟头结点

[203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)（2022.5.28）

```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* pre = new ListNode(0);
        pre->next = head;
        ListNode* cur = pre;
        while(cur->next != nullptr){
            if(cur->next->val ==  val){
                ListNode* node = cur->next;
                cur->next = node->next;
                delete node;
            }else cur = cur->next;
        }
        head = pre->next;
        delete pre;
        return head;
    }
};
```

链表的一大问题就是操作当前节点必须要找前一个节点才能操作。这就造成了头结点的尴尬，因为头结点没有前一个节点了。

**每次对应头结点的情况都要单独处理，所以使用虚拟头结点的技巧，就可以解决这个问题**。



### 2、链表的基本操作

[707. 设计链表](https://leetcode-cn.com/problems/design-linked-list/)（2022.5.28）

```cpp
class MyLinkedList {
public:
    // 定义链表节点结构体
    struct LinkedNode {
        int val;
        LinkedNode* next;
        LinkedNode(int val): val(val), next(nullptr){}
    };

    // 初始化链表
    MyLinkedList() {
        _dummyHead = new LinkedNode(0); // 这里定义的头结点 是一个虚拟头结点，而不是真正的链表头结点
        _size = 0;
    }

    // 获取到第index个节点数值，如果index是非法数值直接返回-1， 注意index是从0开始的，第0个节点就是头结点
    int get(int index) {
        if (index > (_size - 1) || index < 0) {
            return -1;
        }
        LinkedNode* cur = _dummyHead->next;
        while(index--){ // 如果--index 就会陷入死循环
            cur = cur->next;
        }
        return cur->val;
    }

    // 在链表最前面插入一个节点，插入完成后，新插入的节点为链表的新的头结点
    void addAtHead(int val) {
        LinkedNode* newNode = new LinkedNode(val);
        newNode->next = _dummyHead->next;
        _dummyHead->next = newNode;
        _size++;
    }

    // 在链表最后面添加一个节点
    void addAtTail(int val) {
        LinkedNode* newNode = new LinkedNode(val);
        LinkedNode* cur = _dummyHead;
        while(cur->next != nullptr){
            cur = cur->next;
        }
        cur->next = newNode;
        _size++;
    }

    // 在第index个节点之前插入一个新节点，例如index为0，那么新插入的节点为链表的新头节点。
    // 如果index 等于链表的长度，则说明是新插入的节点为链表的尾结点
    // 如果index大于链表的长度，则返回空
    void addAtIndex(int index, int val) {
        if (index > _size) {
            return;
        }
        LinkedNode* newNode = new LinkedNode(val);
        LinkedNode* cur = _dummyHead;
        while(index--) {
            cur = cur->next;
        }
        newNode->next = cur->next;
        cur->next = newNode;
        _size++;
    }

    // 删除第index个节点，如果index 大于等于链表的长度，直接return，注意index是从0开始的
    void deleteAtIndex(int index) {
        if (index >= _size || index < 0) {
            return;
        }
        LinkedNode* cur = _dummyHead;
        while(index--) {
            cur = cur ->next;
        }
        LinkedNode* tmp = cur->next;
        cur->next = cur->next->next;
        delete tmp;
        _size--;
    }

    // 打印链表
    void printLinkedList() {
        LinkedNode* cur = _dummyHead;
        while (cur->next != nullptr) {
            cout << cur->next->val << " ";
            cur = cur->next;
        }
        cout << endl;
    }
private:
    int _size;
    LinkedNode* _dummyHead;
};
```

把链表常见的五个操作练习了一遍，考察了：

- 获取链表第index个节点的数值

- 在链表的最前面插入一个节点

- 在链表的最后面插入一个节点

- 在链表第index个节点前面插入一个节点

- 删除链表的第index个节点的数值

  

### 3、反转链表

[206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)（2022.5.28）

```cpp
//迭代法
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* temp; // 保存cur的下一个节点
        ListNode* cur = head;
        ListNode* pre = NULL;
        while(cur) {
            temp = cur->next;  // 保存一下 cur的下一个节点，因为接下来要改变cur->next
            cur->next = pre; // 翻转操作
            // 更新pre 和 cur指针
            pre = cur;
            cur = temp;
        }
        return pre;
    }
};
```

```cpp
//递归法
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        // 边缘条件判断
        if(!head) return nullptr;
        if(head->next == nullptr) return head;
         // 递归调用，翻转第二个节点开始往后的链表
        ListNode *last = reverseList(head->next);
        // 翻转头节点与第二个节点的指向
        head->next->next = head;
        // 此时的 head 节点为尾节点，next 需要指向 NULL
        head->next = nullptr;
        return last;
    }
};
```

反转链表是面试中高频题目，很考察面试者对链表操作的熟练程度。

两种反转的方式，迭代法和递归法。

先学透迭代法，然后再看递归法，因为递归法比较绕，如果迭代还写不明白，递归基本也写不明白了。

**可以先通过迭代法，彻底弄清楚链表反转的过程！**



### 4、交换节点

[24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)（2022.5.28）

```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummyHead = new ListNode(0); // 设置一个虚拟头结点
        dummyHead->next = head; // 将虚拟头结点指向head，这样方面后面做删除操作
        ListNode* cur = dummyHead;
        while(cur->next != nullptr && cur->next->next != nullptr) {
            ListNode* tmp = cur->next; // 记录临时节点
            ListNode* tmp1 = cur->next->next->next; // 记录临时节点

            cur->next = cur->next->next;    // 步骤一
            cur->next->next = tmp;          // 步骤二
            cur->next->next->next = tmp1;   // 步骤三

            cur = cur->next->next; // cur移动两位，准备下一轮交换
        }
        return dummyHead->next;
    }
};
```

- 时间复杂度：O(n)

- 空间复杂度：O(1)

  

### 5、删除倒数第N个节点

[19. 删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)（2022.5.30）

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* slow = dummyHead;
        ListNode* fast = dummyHead;
        while(n-- && fast != NULL) {
            fast = fast->next;
        }
        fast = fast->next; // fast再提前走一步，因为需要让slow指向删除节点的上一个节点
        while (fast != NULL) {
            fast = fast->next;
            slow = slow->next;
        }
        slow->next = slow->next->next;
        return dummyHead->next;
    }
};
```

结合虚拟头结点和双指针法来移除链表倒数第N个节点。

### 6、链表相交

[面试题 02.07. 链表相交](https://leetcode-cn.com/problems/intersection-of-two-linked-lists-lcci/)（2022.5.30）

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* curA = headA;
        ListNode* curB = headB;
        int lenA = 0, lenB = 0;
        while (curA != NULL) { // 求链表A的长度
            lenA++;
            curA = curA->next;
        }
        while (curB != NULL) { // 求链表B的长度
            lenB++;
            curB = curB->next;
        }
        curA = headA;
        curB = headB;
        // 让curA为最长链表的头，lenA为其长度
        if (lenB > lenA) {
            swap (lenA, lenB);
            swap (curA, curB);
        }
        // 求长度差
        int gap = lenA - lenB;
        // 让curA和curB在同一起点上（末尾位置对齐）
        while (gap--) {
            curA = curA->next;
        }
        // 遍历curA 和 curB，遇到相同则直接返回
        while (curA != NULL) {
            if (curA == curB) {
                return curA;
            }
            curA = curA->next;
            curB = curB->next;
        }
        return NULL;
    }
};
```

- 时间复杂度：O(n+m)
- 空间复杂度：O(1)

使用双指针来找到两个链表的交点（引用完全相同，即：内存地址完全相同的交点）



### 7、环形链表

[142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)（2022.5.30）

```cpp
//哈希表
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        unordered_set<ListNode *> visited;
        while (head != nullptr) {
            if (visited.count(head)) {
                return head;
            }
            visited.insert(head);
            head = head->next;
        }
        return nullptr;
    }
};
```

```cpp
//双指针法
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while(fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
            // 快慢指针相遇，此时从head和相遇点，同时查找直至相遇
            if (slow == fast) {
                ListNode* index1 = fast;
                ListNode* index2 = head;
                while (index1 != index2) {
                    index1 = index1->next;
                    index2 = index2->next;
                }
                return index2; // 返回环的入口
            }
        }
        return NULL;
    }
};
```

在链表如何找环，以及如何找环的入口位置。主要在于一些数学证明。



## 总结

考察链表的操作其实就是考察指针的操作，是面试中的常见类型。

<img src="D:\xzz\LeetCode\List\链表总结.png" alt="链表总结"  />





# 哈希表

## 哈希表理论基础

（2022.5.31）

### 哈希表

哈希表（Hash table）国内也有一些算法书籍翻译为散列表。

> 哈希表是根据关键码的值而直接进行访问的数据结构。

**一般来说哈希表都是用来快速判断一个元素是否出现集合里**。

### **哈希函数**

把传入的key映射到符号表的索引上。

### **哈希碰撞**

处理有多个key映射到相同索引上时的情景，处理碰撞的普遍方式是拉链法和线性探测法。

### 常见的三种哈希结构

- 数组
- set（集合）
- map（映射）

在C++中，set 和 map 分别提供以下三种数据结构，其底层实现以及优劣如下表所示：

| 集合               | 底层实现 | 是否有序 | 数值是否可以重复 | 能否更改数值 | 查询效率 | 增删效率 |
| ------------------ | -------- | -------- | ---------------- | ------------ | -------- | -------- |
| std::set           | 红黑树   | 有序     | 否               | 否           | O(logn)  | O(logn)  |
| std::multiset      | 红黑树   | 有序     | 是               | 否           | O(logn)  | O(logn)  |
| std::unordered_set | 哈希表   | 无序     | 否               | 否           | O(1)     | O(1)     |

std::unordered_set底层实现为哈希表，std::set 和std::multiset 的底层实现是红黑树，红黑树是一种平衡二叉搜索树，所以key值是有序的，但key不可以修改，改动key值会导致整棵树的错乱，所以只能删除和增加。

当我们要使用集合来解决哈希问题的时候，优先使用unordered_set，因为它的查询和增删效率是最优的，如果需要集合是有序的，那么就用set，如果要求不仅有序还要有重复数据的话，那么就用multiset。

| 映射               | 底层实现 | 是否有序 | 数值是否可以重复 | 能否更改数值 | 查询效率 | 增删效率 |
| ------------------ | -------- | -------- | ---------------- | ------------ | -------- | -------- |
| std::map           | 红黑树   | key有序  | key不可重复      | key不可修改  | O(logn)  | O(logn)  |
| std::multimap      | 红黑树   | key有序  | key可重复        | key不可修改  | O(logn)  | O(logn)  |
| std::unordered_map | 哈希表   | key无序  | key不可重复      | key不可修改  | O(1)     | O(1)     |

std::unordered_map 底层实现为哈希表，std::map 和std::multimap 的底层实现是红黑树。同理，std::map 和std::multimap 的key也是有序的（这个问题也经常作为面试题，考察对语言容器底层的理解）。

map 是一个key-value 的数据结构，map中，对key是有限制，对value没有限制的，因为key的存储方式使用红黑树实现的。

**当我们遇到了要快速判断一个元素是否出现集合里的时候，就要考虑哈希法**。



## 哈希表经典题目

### 1、数组作为哈希表

一些应用场景就是为数组量身定做的。

#### 1.1、有效的字母异位词

[242.有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)（2022.5.31）

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        int record[26] = {0};
        for (int i = 0; i < s.size(); i++) {
            // 并不需要记住字符a的ASCII，只要求出一个相对数值就可以了
            record[s[i] - 'a']++;
        }
        for (int i = 0; i < t.size(); i++) {
            record[t[i] - 'a']--;
        }
        for (int i = 0; i < 26; i++) {
            if (record[i] != 0) {
                // record数组如果有的元素不为零0，说明字符串s和t 一定是谁多了字符或者谁少了字符。
                return false;
            }
        }
        // record数组所有元素都为零0，说明字符串s和t是字母异位词
        return true;
    }
};
```

- 时间复杂度：O(n)
- 空间复杂度：O(1)

数组就是简单的哈希表，但是数组的大小是受限的！这道题目包含小写字母，那么使用数组来做哈希最合适不过。

#### 1.2、查找共用字符

[1002. 查找共用字符](https://leetcode.cn/problems/find-common-characters/)（2022.5.31）

```cpp
class Solution {
public:
    vector<string> commonChars(vector<string>& A) {
        vector<string> result;
        if (A.size() == 0) return result;
        int hash[26] = {0}; // 用来统计所有字符串里字符出现的最小频率
        for (int i = 0; i < A[0].size(); i++) { // 用第一个字符串给hash初始化
            hash[A[0][i] - 'a']++;
        }

        int hashOtherStr[26] = {0}; // 统计除第一个字符串外字符的出现频率
        for (int i = 1; i < A.size(); i++) {
            memset(hashOtherStr, 0, 26 * sizeof(int));
            for (int j = 0; j < A[i].size(); j++) {
                hashOtherStr[A[i][j] - 'a']++;
            }
            // 更新hash，保证hash里统计26个字符在所有字符串里出现的最小次数
            for (int k = 0; k < 26; k++) {
                hash[k] = min(hash[k], hashOtherStr[k]);
            }
        }
        // 将hash统计的字符次数，转成输出形式
        for (int i = 0; i < 26; i++) {
            while (hash[i] != 0) { // 注意这里是while，多个重复的字符
                string s(1, i + 'a'); // char -> string
                result.push_back(s);
                hash[i]--;
            }
        }

        return result;
    }
};
```

#### 1.3、赎金信

[383.赎金信](https://leetcode-cn.com/problems/ransom-note/)

```cpp
// 时间复杂度: O(n)
// 空间复杂度：O(1)
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int record[26] = {0};
        for (int i = 0; i < magazine.length(); i++) {
            // 通过recode数据记录 magazine里各个字符出现次数
            record[magazine[i]-'a'] ++;
        }
        for (int j = 0; j < ransomNote.length(); j++) {
            // 遍历ransomNote，在record里对应的字符个数做--操作
            record[ransomNote[j]-'a']--;
            // 如果小于零说明ransomNote里出现的字符，magazine没有
            if(record[ransomNote[j]-'a'] < 0) {
                return false;
            }
        }
        return true;
    }
};
```

同样要求只有小写字母，那么就给我们浓浓的暗示：用数组！

本题和[242.有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)很像，[242.有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)是求字符串a和字符串b是否可以相互组成，在[383.赎金信](https://leetcode-cn.com/problems/ransom-note/)中是求字符串a能否组成字符串b，而不用管字符串b 能不能组成字符串a。

**上面两道题目用map确实可以，但使用map的空间消耗要比数组大一些，因为map要维护红黑树或者符号表，而且还要做哈希函数的运算。所以数组更加简单直接有效！**

### 2、set作为哈希表

#### 2.1、两个数组的交集

[349. 两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays/)（2022.5.31）

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result_set; // 存放结果
        unordered_set<int> nums_set(nums1.begin(), nums1.end());
        for (int num : nums2) {
            // 发现nums2的元素 在nums_set里有出现过
            if (nums_set.find(num) != nums_set.end()) {
                result_set.insert(num);
            }
        }
        return vector<int>(result_set.begin(), result_set.end());
    }
};
```

什么时候不能用数组，需要用set。

这道题目没有限制数值的大小，就无法使用数组来做哈希表了。

**主要因为如下两点：**

- 数组的大小是有限的，受到系统栈空间（不是数据结构的栈）的限制。
- 如果数组空间够大，但哈希值比较少、特别分散、跨度非常大，使用数组就造成空间的极大浪费。

所以此时一样的做映射的话，就可以使用set了。

#### 2.2、快乐数

[202.快乐数](https://leetcode-cn.com/problems/happy-number/)（2022.5.31）

我们再次使用了unordered_set来判断一个数是否重复出现过。

```cpp
class Solution {
public:
    // 取数值各个位上的单数之和
    int getSum(int n) {
        int sum = 0;
        while (n) {
            sum += (n % 10) * (n % 10);
            n /= 10;
        }
        return sum;
    }
    bool isHappy(int n) {
        unordered_set<int> set;
        while(1) {
            int sum = getSum(n);
            if (sum == 1) {
                return true;
            }
            // 如果这个sum曾经出现过，说明已经陷入了无限循环了，立刻return false
            if (set.find(sum) != set.end()) {
                return false;
            } else {
                set.insert(sum);
            }
            n = sum;
        }
    }
};
```



### 3、map作为哈希表

#### 3.1、两数之和

[1.两数之和](https://leetcode-cn.com/problems/two-sum/)（2022.6.2）

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        std::unordered_map <int,int> map;
        for(int i = 0; i < nums.size(); i++) {
            auto iter = map.find(target - nums[i]);
            if(iter != map.end()) {
                return {iter->second, i};
            }
            map.insert(pair<int, int>(nums[i], i));
        }
        return {};
    }
};
```

使用数组和set来做哈希法的局限。

- 数组的大小是受限制的，而且如果元素很少，而哈希值太大会造成内存空间的浪费。
- set是一个集合，里面放的元素只能是一个key，而两数之和这道题目，不仅要判断y是否存在而且还要记录y的下标位置，因为要返回x 和 y的下标。所以set 也不能用。

map是一种`<key, value>`的结构，本题可以用key保存数值，用value在保存数值所在的下标。所以使用map最为合适。

[1.两数之和](https://leetcode-cn.com/problems/two-sum/)中并不需要key有序，选择std::unordered_map 效率更高！

#### 3.2、四数相加

[454.四数相加](https://leetcode-cn.com/problems/4sum-ii/)（2022.6.2）

```cpp
class Solution {
public:
    int fourSumCount(vector<int>& A, vector<int>& B, vector<int>& C, vector<int>& D) {
        unordered_map<int, int> umap; //key:a+b的数值，value:a+b数值出现的次数
        // 遍历大A和大B数组，统计两个数组元素之和，和出现的次数，放到map中
        for (int a : A) {
            for (int b : B) {
                umap[a + b]++;
            }
        }
        int count = 0; // 统计a+b+c+d = 0 出现的次数
        // 在遍历大C和大D数组，找到如果 0-(c+d) 在map中出现过的话，就把map中key对应的value也就是出现次数统计出来。
        for (int c : C) {
            for (int d : D) {
                if (umap.find(0 - (c + d)) != umap.end()) {
                    count += umap[0 - (c + d)];
                }
            }
        }
        return count;
    }
};
```

本题乍眼一看好像和[18. 四数之和](https://leetcode-cn.com/problems/4sum/)，[15.三数之和](https://leetcode-cn.com/problems/3sum/)差不多，其实差很多！

关键差别是本题为四个独立的数组，只要找到A[i] + B[j] + C[k] + D[l] = 0就可以，不用考虑重复问题，而[18. 四数之和](https://leetcode-cn.com/problems/4sum/)，[15.三数之和](https://leetcode-cn.com/problems/3sum/)是一个数组（集合）里找到和为0的组合，可就难很多了！

#### 3.3、三数之和

[15.三数之和](https://leetcode-cn.com/problems/3sum/)（2022.6.2）

用哈希法解决了两数之和，很多同学会感觉用哈希法也可以解决三数之和，四数之和。

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        // 找出a + b + c = 0
        // a = nums[i], b = nums[left], c = nums[right]
        for (int i = 0; i < nums.size(); i++) {
            // 排序之后如果第一个元素已经大于零，那么无论如何组合都不可能凑成三元组，直接返回结果就可以了
            if (nums[i] > 0) {
                return result;
            }
            // 错误去重方法，将会漏掉-1,-1,2 这种情况
            /*
            if (nums[i] == nums[i + 1]) {
                continue;
            }
            */
            // 正确去重方法
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int left = i + 1;
            int right = nums.size() - 1;
            while (right > left) {
                // 去重复逻辑如果放在这里，0，0，0 的情况，可能直接导致 right<=left 了，从而漏掉了 0,0,0 这种三元组
                /*
                while (right > left && nums[right] == nums[right - 1]) right--;
                while (right > left && nums[left] == nums[left + 1]) left++;
                */
                if (nums[i] + nums[left] + nums[right] > 0) {
                    right--;
                    // 当前元素不合适了，可以去重
                    while (left < right && nums[right] == nums[right + 1]) right--;
                } else if (nums[i] + nums[left] + nums[right] < 0) {
                    left++;
                    // 不合适，去重
                    while (left < right && nums[left] == nums[left - 1]) left++;
                } else {
                    result.push_back(vector<int>{nums[i], nums[left], nums[right]});
                    // 去重逻辑应该放在找到一个三元组之后
                    while (right > left && nums[right] == nums[right - 1]) right--;
                    while (right > left && nums[left] == nums[left + 1]) left++;

                    // 找到答案时，双指针同时收缩
                    right--;
                    left++;
                }
            }

        }
        return result;
    }
};
```

其实是可以解决，但是非常麻烦，需要去重导致代码效率很低。

在[15.三数之和](https://leetcode-cn.com/problems/3sum/)中使用哈希法和双指针两个解法，可以体会到只使用哈希法是比较麻烦的。

#### 3.4、四数之和

[18. 四数之和](https://leetcode-cn.com/problems/4sum/)（2022.6.2）

所以[18. 四数之和](https://leetcode-cn.com/problems/4sum/)，[15.三数之和](https://leetcode-cn.com/problems/3sum/)都推荐使用双指针法。

```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int k = 0; k < nums.size(); k++) {
            // 这种剪枝是错误的，这道题目target 是任意值
            // if (nums[k] > target) {
            //     return result;
            // }
            // 去重
            if (k > 0 && nums[k] == nums[k - 1]) {
                continue;
            }
            for (int i = k + 1; i < nums.size(); i++) {
                // 正确去重方法
                if (i > k + 1 && nums[i] == nums[i - 1]) {
                    continue;
                }
                int left = i + 1;
                int right = nums.size() - 1;
                while (right > left) {
                    // nums[k] + nums[i] + nums[left] + nums[right] > target 会溢出
                    if (nums[k] + nums[i] > target - (nums[left] + nums[right])) {
                        right--;
                        // 当前元素不合适了，可以去重
                        while (left < right && nums[right] == nums[right + 1]) right--;
                    // nums[k] + nums[i] + nums[left] + nums[right] < target 会溢出
                    } else if (nums[k] + nums[i]  < target - (nums[left] + nums[right])) {
                        left++;
                        // 不合适，去重
                        while (left < right && nums[left] == nums[left - 1]) left++;
                    } else {
                        result.push_back(vector<int>{nums[k], nums[i], nums[left], nums[right]});
                        // 去重逻辑应该放在找到一个四元组之后
                        while (right > left && nums[right] == nums[right - 1]) right--;
                        while (right > left && nums[left] == nums[left + 1]) left++;

                        // 找到答案时，双指针同时收缩
                        right--;
                        left++;
                    }
                }
            }
        }
        return result;
    }
};
```



# 字符串

## 字符串理论基础

### 什么是字符串

字符串是若干字符组成的有限序列，也可以理解为是一个字符数组，但是很多语言对字符串做了特殊的规定，接下来我来说一说C/C++中的字符串。

在C语言中，把一个字符串存入一个数组时，也把结束符 '\0'存入数组，并以此作为该字符串是否结束的标志。

例如这段代码：

```
char a[5] = "asd";
for (int i = 0; a[i] != '\0'; i++) {
}
```

在C++中，提供一个string类，string类会提供size接口，可以用来判断string类字符串是否结束，就不用'\0'来判断是否结束。

例如这段代码:

```
string a = "asd";
for (int i = 0; i < a.size(); i++) {
}
```

那么vector< char > 和 string 又有什么区别呢？

其实在基本操作上没有区别，但是 string提供更多的字符串处理的相关接口，例如string 重载了+，而vector却没有。

所以想处理字符串，我们还是会定义一个string类型。

### 要不要使用库函数

**打基础的时候，不要太迷恋于库函数。**

甚至一些同学习惯于调用substr，split，reverse之类的库函数，却不知道其实现原理，也不知道其时间复杂度，这样实现出来的代码，如果在面试现场，面试官问：“分析其时间复杂度”的话，一定会一脸懵逼！

所以建议**如果题目关键的部分直接用库函数就可以解决，建议不要使用库函数。**

**如果库函数仅仅是 解题过程中的一小部分，并且你已经很清楚这个库函数的内部实现原理的话，可以考虑使用库函数。**

## 字符串经典题目

### 1、双指针法

#### 1.1、反转字符串

[344. 反转字符串](https://leetcode.cn/problems/reverse-string/)（2022.6.4）

在[344. 反转字符串](https://leetcode.cn/problems/reverse-string/)，我们使用双指针法实现了反转字符串的操作，**双指针法在数组，链表和字符串中很常用。**

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        for (int i = 0, j = s.size() - 1; i < s.size()/2; i++, j--) {
            swap(s[i],s[j]);
        }
    }
};
```

#### 1.2、替换空格

[剑指 Offer 05. 替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)（2022.6.4）

```cpp
class Solution {
public:
    string replaceSpace(string s) {
        int count = 0; // 统计空格的个数
        int sOldSize = s.size();
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == ' ') {
                count++;
            }
        }
        // 扩充字符串s的大小，也就是每个空格替换成"%20"之后的大小
        s.resize(s.size() + count * 2);
        int sNewSize = s.size();
        // 从后先前将空格替换为"%20"
        for (int i = sNewSize - 1, j = sOldSize - 1; j < i; i--, j--) {
            if (s[j] != ' ') {
                s[i] = s[j];
            } else {
                s[i] = '0';
                s[i - 1] = '2';
                s[i - 2] = '%';
                i -= 2;
            }
        }
        return s;
    }
};
```

- 时间复杂度：O(n)
- 空间复杂度：O(1)

**其实很多数组填充类的问题，都可以先预先给数组扩容带填充后的大小，然后在从后向前进行操作。**

那么针对数组删除操作的问题，其实在[27. 移除元素](https://gitee.com/link?target=https%3A%2F%2Fprogrammercarl.com%2F0027.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0.html)中就已经提到了使用双指针法进行移除操作。

同样的道理在[151.翻转字符串里的单词](https://gitee.com/link?target=https%3A%2F%2Fprogrammercarl.com%2F0151.%E7%BF%BB%E8%BD%AC%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%87%8C%E7%9A%84%E5%8D%95%E8%AF%8D.html)中我们使用O(n)的时间复杂度，完成了删除冗余空格。

一些同学会使用for循环里调用库函数erase来移除元素，这其实是O(n^2)的操作，因为erase就是O(n)的操作，所以这也是典型的不知道库函数的时间复杂度，上来就用的案例了。

### 2、反转系列

在反转上还可以在加一些玩法，其实考察的是对代码的掌控能力。

#### 2.1、反转字符串 II

[541. 反转字符串 II](https://leetcode.cn/problems/reverse-string-ii/)（2022.6.4）

```cpp
//左闭右开
class Solution {
public:
    string reverseStr(string s, int k) {
        for (int i = 0; i < s.size(); i += (2 * k)) {
            // 1. 每隔 2k 个字符的前 k 个字符进行反转
            // 2. 剩余字符小于 2k 但大于或等于 k 个，则反转前 k 个字符
            if (i + k <= s.size()) {
                reverse(s.begin() + i, s.begin() + i + k );
                continue;
            }
            // 3. 剩余字符少于 k 个，则将剩余字符全部反转。
            reverse(s.begin() + i, s.begin() + s.size());
        }
        return s;
    }
};
```

一些同学可能为了处理逻辑：每隔2k个字符的前k的字符，写了一堆逻辑代码或者再搞一个计数器，来统计2k，再统计前k个字符。

其实**当需要固定规律一段一段去处理字符串的时候，要想想在在for循环的表达式上做做文章**。

只要让 i += (2 * k)，i 每次移动 2 * k 就可以了，然后判断是否需要有反转的区间。

因为要找的也就是每2 * k 区间的起点，这样写程序会高效很多。

#### 2.2、翻转字符串里的单词

[151. 颠倒字符串中的单词 ](https://leetcode.cn/problems/reverse-words-in-a-string/)（2022.6.4）

```cpp
class Solution {
public:
    void reverse(string& s, int start, int end){ //翻转，区间写法：闭区间 []
        for (int i = start, j = end; i < j; i++, j--) {
            swap(s[i], s[j]);
        }
    }

    void removeExtraSpaces(string& s) {//去除所有空格并在相邻单词之间添加空格, 快慢指针。
        int slow = 0;   //整体思想参考Leetcode: 27. 移除元素：https://leetcode-cn.com/problems/remove-element/
        for (int i = 0; i < s.size(); ++i) { //
            if (s[i] != ' ') { //遇到非空格就处理，即删除所有空格。
                if (slow != 0) s[slow++] = ' '; //手动控制空格，给单词之间添加空格。slow != 0说明不是第一个单词，需要在单词前添加空格。
                while (i < s.size() && s[i] != ' ') { //补上该单词，遇到空格说明单词结束。
                    s[slow++] = s[i++];
                }
            }
        }
        s.resize(slow); //slow的大小即为去除多余空格后的大小。
    }

    string reverseWords(string s) {
        removeExtraSpaces(s); //去除多余空格，保证单词之间之只有一个空格，且字符串首尾没空格。
        reverse(s, 0, s.size() - 1);
        int start = 0; //removeExtraSpaces后保证第一个单词的开始下标一定是0。
        for (int i = 0; i <= s.size(); ++i) {
            if (i == s.size() || s[i] == ' ') { //到达空格或者串尾，说明一个单词结束。进行翻转。
                reverse(s, start, i - 1); //翻转，注意是左闭右闭 []的翻转。
                start = i + 1; //更新下一个单词的开始下标start
            }
        }
        return s;
    }
};
```

在[151. 颠倒字符串中的单词 ](https://leetcode.cn/problems/reverse-words-in-a-string/)中要求翻转字符串里的单词，这道题目可以说是综合考察了字符串的多种操作。是考察字符串的好题。

这道题目通过 **先整体反转再局部反转**，实现了反转字符串里的单词。

后来发现反转字符串还有一个牛逼的用处，就是达到左旋的效果。

#### 2.3、翻转字符串

[剑指 Offer 58 - II. 左旋转字符串](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)（2022.6.5）

```cpp
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        reverse(s.begin(), s.begin() + n);
        reverse(s.begin() + n, s.end());
        reverse(s.begin(), s.end());
        return s;
    }
};
```

- 时间复杂度：O(n)
- 空间复杂度：O(1)

我们通过**先局部反转再整体反转**达到了左旋的效果。

### 3、KMP

KMP的主要思想是**当出现字符串不匹配时，可以知道一部分之前已经匹配的文本内容，可以利用这些信息避免从头再去做匹配了。**

KMP的精髓所在就是前缀表：起始位置到下标i之前（包括i）的子串中，有多大长度的相同前缀后缀。

#### 3.1、实现strStr()

[28. 实现 strStr()](https://leetcode.cn/problems/implement-strstr/)（2022.6.5）

```cpp
class Solution {
public:
    void getNext(int* next, const string& s) {
        int j = 0;
        next[0] = 0;
        for(int i = 1; i < s.size(); i++) {
            while (j > 0 && s[i] != s[j]) {
                j = next[j - 1];
            }
            if (s[i] == s[j]) {
                j++;
            }
            next[i] = j;
        }
    }
    int strStr(string haystack, string needle) {
        if (needle.size() == 0) {
            return 0;
        }
        int next[needle.size()];
        getNext(next, needle);
        int j = 0;
        for (int i = 0; i < haystack.size(); i++) {
            while(j > 0 && haystack[i] != needle[j]) {
                j = next[j - 1];
            }
            if (haystack[i] == needle[j]) {
                j++;
            }
            if (j == needle.size() ) {
                return (i - needle.size() + 1);
            }
        }
        return -1;
    }
};
```

#### 3.2、重复的子字符串

[459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/)（2022.6.5）

```cpp
class Solution {
public:
    void getNext (int* next, const string& s){
        next[0] = 0;
        int j = 0;
        for(int i = 1;i < s.size(); i++){
            while(j > 0 && s[i] != s[j]) {
                j = next[j - 1];
            }
            if(s[i] == s[j]) {
                j++;
            }
            next[i] = j;
        }
    }
    bool repeatedSubstringPattern (string s) {
        if (s.size() == 0) {
            return false;
        }
        int next[s.size()];
        getNext(next, s);
        int len = s.size();
        if (next[len - 1] != 0 && len % (len - (next[len - 1] )) == 0) {
            return true;
        }
        return false;
    }
};
```

再一次强调了什么是前缀，什么是后缀，什么又是最长相等前后缀。

前缀：指不包含最后一个字符的所有以第一个字符开头的连续子串。

后缀：指不包含第一个字符的所有以最后一个字符结尾的连续子串。

然后**针对前缀表到底要不要减一，这其实是不同KMP实现的方式**，其中主要**理解j=next[x]这一步最为关键！**

### 总结

字符串类类型的题目，往往想法比较简单，但是实现起来并不容易，复杂的字符串题目非常考验对代码的掌控能力。

双指针法是字符串处理的常客。

KMP算法是字符串查找最重要的算法。



# 双指针法

双指针法并不隶属于某一种数据结构，我们在数组，链表，字符串都用到了双指针法，所以有必要针对双指针法做一个总结。（2022.6.6）

## 1、数组篇

在[27. 移除元素](https://leetcode.cn/problems/remove-element/)中，原地移除数组上的元素，我们说到了数组上的元素，不能真正的删除，只能覆盖。

一些同学可能会写出如下代码（伪代码）：

```cpp
for (int i = 0; i < array.size(); i++) {
    if (array[i] == target) {
        array.erase(i);
    }
}
```

这个代码看上去好像是O(n)的时间复杂度，其实是O(n^2)的时间复杂度，因为erase操作也是O(n)的操作。

所以此时使用双指针法才展现出效率的优势：**通过两个指针在一个for循环下完成两个for循环的工作。**

## 2、字符串篇

在[344. 反转字符串](https://leetcode.cn/problems/reverse-string/)中讲解了反转字符串，注意这里强调要原地反转，要不然就失去了题目的意义。

使用双指针法，**定义两个指针（也可以说是索引下标），一个从字符串前面，一个从字符串后面，两个指针同时向中间移动，并交换元素。**，时间复杂度是O(n)。

在[剑指 Offer 05. 替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)中介绍使用双指针填充字符串的方法，如果想把这道题目做到极致，就不要只用额外的辅助空间了！

思路就是**首先扩充数组到每个空格替换成"%20"之后的大小。然后双指针从后向前替换空格。**

有同学问了，为什么要从后向前填充，从前向后填充不行么？

从前向后填充就是O(n^2)的算法了，因为每次添加元素都要将添加元素之后的所有元素向后移动。

**其实很多数组（字符串）填充类的问题，都可以先预先给数组扩容带填充后的大小，然后在从后向前进行操作。**

那么在[151. 颠倒字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)中，我们使用双指针法，用O(n)的时间复杂度完成字符串删除类的操作，因为题目要产出冗余空格。

**在删除冗余空格的过程中，如果不注意代码效率，很容易写成了O(n^2)的时间复杂度。其实使用双指针法O(n)就可以搞定。**

**主要还是大家用erase用的比较随意，一定要注意for循环下用erase的情况，一般可以用双指针写效率更高！**

## 3、链表篇

翻转链表是现场面试，白纸写代码的好题，考察了候选者对链表以及指针的熟悉程度，而且代码也不长，适合在白纸上写。

在[206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)中，讲如何使用双指针法来翻转链表，**只需要改变链表的next指针的指向，直接将链表反转 ，而不用重新定义一个新的链表。**

思路还是很简单的，代码也不长，但是想在白纸上一次性写出bugfree的代码，并不是容易的事情。

在链表中求环，应该是双指针在链表里最经典的应用，在[142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)中讲解了如何通过双指针判断是否有环，而且还要找到环的入口。

**使用快慢指针（双指针法），分别定义 fast 和 slow指针，从头结点出发，fast指针每次移动两个节点，slow指针每次移动一个节点，如果 fast 和 slow指针在途中相遇 ，说明这个链表有环。**

那么找到环的入口，其实需要点简单的数学推理，我在文章中把找环的入口清清楚楚的推理的一遍，如果对找环入口不够清楚的同学建议自己看一看[142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)。

## 4、N数之和篇

在[15.三数之和](https://leetcode-cn.com/problems/3sum/)中，讲到使用哈希法可以解决1.两数之和的问题

其实使用双指针也可以解决1.两数之和的问题，只不过1.两数之和求的是两个元素的下标，没法用双指针，如果改成求具体两个元素的数值就可以了，大家可以尝试用双指针做一个leetcode上两数之和的题目，就可以体会到我说的意思了。

使用了哈希法解决了两数之和，但是哈希法并不使用于三数之和！

使用哈希法的过程中要把符合条件的三元组放进vector中，然后在去去重，这样是非常费时的，很容易超时，也是三数之和通过率如此之低的根源所在。

去重的过程不好处理，有很多小细节，如果在面试中很难想到位。

时间复杂度可以做到O(n^2)，但还是比较费时的，因为不好做剪枝操作。

所以这道题目使用双指针法才是最为合适的，用双指针做这道题目才能就能真正体会到，**通过前后两个指针不算向中间逼近，在一个for循环下完成两个for循环的工作。**

只用双指针法时间复杂度为O(n^2)，但比哈希法的O(n^2)效率高得多，哈希法在使用两层for循环的时候，能做的剪枝操作很有限。

在[18. 四数之和](https://leetcode-cn.com/problems/4sum/)中，讲到了四数之和，其实思路是一样的，**在三数之和的基础上再套一层for循环，依然是使用双指针法。**

对于三数之和使用双指针法就是将原本暴力O(n^3)的解法，降为O(n^2)的解法，四数之和的双指针解法就是将原本暴力O(n^4)的解法，降为O(n^3)的解法。

同样的道理，五数之和，n数之和都是在这个基础上累加。

## 5、总结

本文中一共介绍了九道使用双指针解决问题的经典题目，除了链表一些题目一定要使用双指针，其他题目都是使用双指针来提高效率，一般是将O(n^2)的时间复杂度，降为O(n)。



# 栈与队列

## 栈与队列理论基础

队列是先进先出，栈是先进后出。

如图所示：

![栈与队列](D:\xzz\LeetCode\Stack\栈与队列.png)

列出四个关于栈的问题：

1. C++中stack 是容器么？
2. 我们使用的stack是属于哪个版本的STL？
3. 我们使用的STL中stack是如何实现的？
4. stack 提供迭代器来遍历stack空间么？

首先大家要知道 栈和队列是STL（C++标准库）里面的两个数据结构。

C++标准库是有多个版本的，要知道我们使用的STL是哪个版本，才能知道对应的栈和队列的实现原理。

那么来介绍一下，三个最为普遍的STL版本：

1. HP STL 其他版本的C++ STL，一般是以HP STL为蓝本实现出来的，HP STL是C++ STL的第一个实现版本，而且开放源代码。
2. P.J.Plauger STL 由P.J.Plauger参照HP STL实现出来的，被Visual C++编译器所采用，不是开源的。
3. SGI STL 由Silicon Graphics Computer Systems公司参照HP STL实现，被Linux的C++编译器GCC所采用，SGI STL是开源软件，源码可读性甚高。

接下来介绍的栈和队列也是SGI STL里面的数据结构， 知道了使用版本，才知道对应的底层实现。

来说一说栈，栈先进后出，如图所示：

![栈](D:\xzz\LeetCode\Stack\栈.png)

栈提供push 和 pop 等等接口，所有元素必须符合先进后出规则，所以栈不提供走访功能，也不提供迭代器(iterator)。 不像是set 或者map 提供迭代器iterator来遍历所有元素。

**栈是以底层容器完成其所有的工作，对外提供统一的接口，底层容器是可插拔的（也就是说我们可以控制使用哪种容器来实现栈的功能）。**

所以STL中栈往往不被归类为容器，而被归类为container adapter（容器适配器）。

那么问题来了，STL 中栈是用什么容器实现的？

从下图中可以看出，栈的内部结构，栈的底层实现可以是vector，deque，list 都是可以的， 主要就是数组和链表的底层实现。

![队列](D:\xzz\LeetCode\Stack\队列.png)

**我们常用的SGI STL，如果没有指定底层实现的话，默认是以deque为缺省情况下栈的低层结构。**

deque是一个双向队列，只要封住一段，只开通另一端就可以实现栈的逻辑了。

**SGI STL中 队列底层实现缺省情况下一样使用deque实现的。**

我们也可以指定vector为栈的底层实现，初始化语句如下：

```cpp
std::stack<int, std::vector<int> > third;  // 使用vector为底层容器的栈
```

刚刚讲过栈的特性，对应的队列的情况是一样的。

队列中先进先出的数据结构，同样不允许有遍历行为，不提供迭代器, **SGI STL中队列一样是以deque为缺省情况下的底部结构。**

也可以指定list 为起底层实现，初始化queue的语句如下：

```cpp
std::queue<int, std::list<int>> third; // 定义以list为底层容器的队列
```

所以STL 队列也不被归类为容器，而被归类为container adapter（ 容器适配器）。

## 栈与队列经典题目

### 1、用栈实现队列

[232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)（2022.6.7）

使用栈实现队列的下列操作：

push(x) -- 将一个元素放入队列的尾部。
pop() -- 从队列首部移除元素。
peek() -- 返回队列首部的元素。
empty() -- 返回队列是否为空。

示例:

```cpp
MyQueue queue = new MyQueue();
queue.push(1);
queue.push(2);
queue.peek();  // 返回 1
queue.pop();   // 返回 1
queue.empty(); // 返回 false
```

```cpp
class MyQueue {
public:
    stack<int> stIn;
    stack<int> stOut;
    /** Initialize your data structure here. */
    MyQueue() {

    }
    /** Push element x to the back of queue. */
    void push(int x) {
        stIn.push(x);
    }

    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        // 只有当stOut为空的时候，再从stIn里导入数据（导入stIn全部数据）
        if (stOut.empty()) {
            // 从stIn导入数据直到stIn为空
            while(!stIn.empty()) {
                stOut.push(stIn.top());
                stIn.pop();
            }
        }
        int result = stOut.top();
        stOut.pop();
        return result;
    }

    /** Get the front element. */
    int peek() {
        int res = this->pop(); // 直接使用已有的pop函数
        stOut.push(res); // 因为pop函数弹出了元素res，所以再添加回去
        return res;
    }

    /** Returns whether the queue is empty. */
    bool empty() {
        return stIn.empty() && stOut.empty();
    }
};
```

### 2、用队列实现栈

[225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)（2022.6.7）

使用队列实现栈的下列操作：

- push(x) -- 元素 x 入栈
- pop() -- 移除栈顶元素
- top() -- 获取栈顶元素
- empty() -- 返回栈是否为空

```cpp
class MyStack {
public:
    queue<int> que1;
    queue<int> que2; // 辅助队列，用来备份
    /** Initialize your data structure here. */
    MyStack() {

    }

    /** Push element x onto stack. */
    void push(int x) {
        que1.push(x);
    }

    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        int size = que1.size();
        size--;
        while (size--) { // 将que1 导入que2，但要留下最后一个元素
            que2.push(que1.front());
            que1.pop();
        }

        int result = que1.front(); // 留下的最后一个元素就是要返回的值
        que1.pop();
        que1 = que2;            // 再将que2赋值给que1
        while (!que2.empty()) { // 清空que2
            que2.pop();
        }
        return result;
    }

    /** Get the top element. */
    int top() {
        return que1.back();
    }

    /** Returns whether the stack is empty. */
    bool empty() {
        return que1.empty();
    }
};
```

### 3、括号匹配问题

[20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)（2022.6.7）

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

- 左括号必须用相同类型的右括号闭合。
- 左括号必须以正确的顺序闭合。
- 注意空字符串可被认为是有效字符串。

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<int> st;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(') st.push(')');
            else if (s[i] == '{') st.push('}');
            else if (s[i] == '[') st.push(']');
            // 第三种情况：遍历字符串匹配的过程中，栈已经为空了，没有匹配的字符了，说明右括号没有找到对应的左括号 return false
            // 第二种情况：遍历字符串匹配的过程中，发现栈里没有我们要匹配的字符。所以return false
            else if (st.empty() || st.top() != s[i]) return false;
            else st.pop(); // st.top() 与 s[i]相等，栈弹出元素
        }
        // 第一种情况：此时我们已经遍历完了字符串，但是栈不为空，说明有相应的左括号没有右括号来匹配，所以return false，否则就return true
        return st.empty();
    }
};
```

### 4、字符串去重问题

[1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)（2022/6/7）

给出由小写字母组成的字符串 S，重复项删除操作会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

示例：

- 输入："abbaca"
- 输出："ca"
- 解释：例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。

```cpp
class Solution {
public:
    string removeDuplicates(string S) {
        stack<char> st;
        for (char s : S) {
            if (st.empty() || s != st.top()) {
                st.push(s);
            } else {
                st.pop(); // s 与 st.top()相等的情况
            }
        }
        string result = "";
        while (!st.empty()) { // 将栈中元素放到result字符串汇总
            result += st.top();
            st.pop();
        }
        reverse (result.begin(), result.end()); // 此时字符串需要反转一下
        return result;

    }
};
```

### 5、逆波兰表达式问题

[150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)（2022.6.8）

根据 逆波兰表示法，求表达式的值。

有效的运算符包括 + , - , * , / 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。

说明：

整数除法只保留整数部分。 给定逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。

示例 1：

- 输入: ["2", "1", "+", "3", " * "]
- 输出: 9
- 解释: 该算式转化为常见的中缀算术表达式为：((2 + 1) * 3) = 9

示例 2：

- 输入: ["4", "13", "5", "/", "+"]
- 输出: 6
- 解释: 该算式转化为常见的中缀算术表达式为：(4 + (13 / 5)) = 6

示例 3：

- 输入: ["10", "6", "9", "3", "+", "-11", " * ", "/", " * ", "17", "+", "5", "+"]

- 输出: 22

- 解释:该算式转化为常见的中缀算术表达式为：

  ```cpp
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5       
  = ((10 * (6 / (12 * -11))) + 17) + 5       
  = ((10 * (6 / -132)) + 17) + 5     
  = ((10 * 0) + 17) + 5     
  = (0 + 17) + 5    
  = 17 + 5    
  = 22    
  ```

逆波兰表达式：是一种后缀表达式，所谓后缀就是指算符写在后面。

平常使用的算式则是一种中缀表达式，如 ( 1 + 2 ) * ( 3 + 4 ) 。

该算式的逆波兰表达式写法为 ( ( 1 2 + ) ( 3 4 + ) * ) 。

```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> st;
        for (int i = 0; i < tokens.size(); i++) {
            if (tokens[i] == "+" || tokens[i] == "-" || tokens[i] == "*" || tokens[i] == "/") {
                int num1 = st.top();
                st.pop();
                int num2 = st.top();
                st.pop();
                if (tokens[i] == "+") st.push(num2 + num1);
                if (tokens[i] == "-") st.push(num2 - num1);
                if (tokens[i] == "*") st.push(num2 * num1);
                if (tokens[i] == "/") st.push(num2 / num1);
            } else {
                st.push(stoi(tokens[i]));
            }
        }
        int result = st.top();
        st.pop(); // 把栈里最后一个元素弹出（其实不弹出也没事）
        return result;
    }
};
```

### 6、滑动窗口最大值

[239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)（2022.6.8）

给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回 滑动窗口中的最大值 。

```cpp
class Solution {
private:
    class MyQueue { //单调队列（从大到小）
    public:
        deque<int> que; // 使用deque来实现单调队列
        // 每次弹出的时候，比较当前要弹出的数值是否等于队列出口元素的数值，如果相等则弹出。
        // 同时pop之前判断队列当前是否为空。
        void pop(int value) {
            if (!que.empty() && value == que.front()) {
                que.pop_front();
            }
        }
        // 如果push的数值大于入口元素的数值，那么就将队列后端的数值弹出，直到push的数值小于等于队列入口元素的数值为止。
        // 这样就保持了队列里的数值是单调从大到小的了。
        void push(int value) {
            while (!que.empty() && value > que.back()) {
                que.pop_back();
            }
            que.push_back(value);

        }
        // 查询当前队列里的最大值 直接返回队列前端也就是front就可以了。
        int front() {
            return que.front();
        }
    };
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        MyQueue que;
        vector<int> result;
        for (int i = 0; i < k; i++) { // 先将前k的元素放进队列
            que.push(nums[i]);
        }
        result.push_back(que.front()); // result 记录前k的元素的最大值
        for (int i = k; i < nums.size(); i++) {
            que.pop(nums[i - k]); // 滑动窗口移除最前面元素
            que.push(nums[i]); // 滑动窗口前加入最后面的元素
            result.push_back(que.front()); // 记录对应的最大值
        }
        return result;
    }
};
```

### 7、前K个高频元素

[347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)（2022.6.8）

给你一个整数数组 `nums` 和一个整数 `k` ，请你返回其中出现频率前 `k` 高的元素。你可以按 **任意顺序** 返回答案。

```cpp
// 时间复杂度：O(nlogk)
// 空间复杂度：O(n)
class Solution {
public:
    // 小顶堆
    class mycomparison {
    public:
        bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs) {
            return lhs.second > rhs.second;
        }
    };
    vector<int> topKFrequent(vector<int>& nums, int k) {
        // 要统计元素出现频率
        unordered_map<int, int> map; // map<nums[i],对应出现的次数>
        for (int i = 0; i < nums.size(); i++) {
            map[nums[i]]++;
        }

        // 对频率排序
        // 定义一个小顶堆，大小为k
        priority_queue<pair<int, int>, vector<pair<int, int>>, mycomparison> pri_que;

        // 用固定大小为k的小顶堆，扫面所有频率的数值
        for (unordered_map<int, int>::iterator it = map.begin(); it != map.end(); it++) {
            pri_que.push(*it);
            if (pri_que.size() > k) { // 如果堆的大小大于了K，则队列弹出，保证堆的大小一直为k
                pri_que.pop();
            }
        }

        // 找出前K个高频元素，因为小顶堆先弹出的是最小的，所以倒序来输出到数组
        vector<int> result(k);
        for (int i = k - 1; i >= 0; i--) {
            result[i] = pri_que.top().first;
            pri_que.pop();
        }
        return result;

    }
};
```



# 二叉树

## 二叉树理论基础

### 二叉树的种类

在我们解题过程中二叉树有两种主要的形式：满二叉树和完全二叉树。

#### 满二叉树

满二叉树：如果一棵二叉树只有度为0的结点和度为2的结点，并且度为0的结点在同一层上，则这棵二叉树为满二叉树。

如图所示：

![完全二叉树](D:\xzz\LeetCode\Tree\完全二叉树.png)

这棵二叉树为满二叉树，也可以说深度为k，有2^k-1个节点的二叉树。

#### 完全二叉树

完全二叉树的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~ 2^(h-1)  个节点。

![完全二叉树判断](D:\xzz\LeetCode\Tree\完全二叉树判断.png)

**之前我们刚刚讲过优先级队列其实是一个堆，堆就是一棵完全二叉树，同时保证父子节点的顺序关系。**

#### 二叉搜索树

前面介绍的树，都没有数值的，而二叉搜索树是有数值的了，**二叉搜索树是一个有序树**。

- 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
- 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
- 它的左、右子树也分别为二叉排序树

下面这两棵树都是搜索树 

![二叉搜索树](D:\xzz\LeetCode\Tree\二叉搜索树.png)

#### 平衡二叉搜索树

平衡二叉搜索树：又被称为AVL（Adelson-Velsky and Landis）树，且具有以下性质：它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。

如图：

![AVL](D:\xzz\LeetCode\Tree\AVL.png)

最后一棵 不是平衡二叉树，因为它的左右两个子树的高度差的绝对值超过了1。

**C++中map、set、multimap，multiset的底层实现都是平衡二叉搜索树**，所以map、set的增删操作时间时间复杂度是logn，注意我这里没有说unordered_map、unordered_set，unordered_map、unordered_map底层实现是哈希表。

### 二叉树的存储方式

**二叉树可以链式存储，也可以顺序存储。**

链式存储方式就用指针， 顺序存储的方式就是用数组。

顾名思义就是顺序存储的元素在内存是连续分布的，而链式存储则是通过指针把分布在散落在各个地址的节点串联一起。

链式存储如图：

![链式存储](D:\xzz\LeetCode\Tree\链式存储.png)

顺序存储其实就是用数组来存储二叉树，如图：

![顺序存储](D:\xzz\LeetCode\Tree\顺序存储.png)

用数组来存储二叉树如何遍历的呢？

**如果父节点的数组下标是 i，那么它的左孩子就是 i \* 2 + 1，右孩子就是 i \* 2 + 2。**

但是用链式表示的二叉树，更有利于我们理解，所以一般我们都是用链式存储二叉树。

**所以大家要了解，用数组依然可以表示二叉树。**

### 二叉树的遍历方式

二叉树主要有两种遍历方式：

1. 深度优先遍历：先往深走，遇到叶子节点再往回走。
2. 广度优先遍历：一层一层的去遍历。

**这两种遍历是图论中最基本的两种遍历方式。**

那么从深度优先遍历和广度优先遍历进一步拓展，才有如下遍历方式：

- 深度优先遍历
  - 前序遍历（递归法，迭代法）
  - 中序遍历（递归法，迭代法）
  - 后序遍历（递归法，迭代法）
- 广度优先遍历
  - 层次遍历（迭代法）

在深度优先遍历中：有三个顺序，前中后序遍历， **这里前中后，其实指的就是中间节点的遍历顺序**。

看如下中间节点的顺序，就可以发现，中间节点的顺序就是所谓的遍历方式

- 前序遍历：中左右
- 中序遍历：左中右
- 后序遍历：左右中

![深度优先遍历](D:\xzz\LeetCode\Tree\深度优先遍历.png)

最后再说一说二叉树中深度优先和广度优先遍历实现方式，我们做二叉树相关题目，经常会使用递归的方式来实现深度优先遍历，也就是实现前中后序遍历，使用递归是比较方便的。

**之前我们讲栈与队列的时候，就说过栈其实就是递归的一种是实现结构**，也就说前中后序遍历的逻辑其实都是可以借助栈使用非递归的方式来实现的。

而广度优先遍历的实现一般使用队列来实现，这也是队列先进先出的特点所决定的，因为需要先进先出的结构，才能一层一层的来遍历二叉树。

**这里其实我们又了解了栈与队列的一个应用场景了。**

### 二叉树的定义

刚刚我们说过了二叉树有两种存储方式顺序存储和链式存储，顺序存储就是用数组来存，这个定义没啥可说的，我们来看看链式存储的二叉树节点的定义方式。

C++代码如下：

```cpp
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
```

大家会发现二叉树的定义 和链表是差不多的，相对于链表 ，二叉树的节点里多了一个指针， 有两个指针，指向左右孩子。

这里要提醒大家要注意二叉树节点定义的书写方式。

**在现场面试的时候 面试官可能要求手写代码，所以数据结构的定义以及简单逻辑的代码一定要锻炼白纸写出来。**

### 总结

二叉树是一种基础数据结构，在算法面试中都是常客，也是众多数据结构的基石。

本篇我们介绍了二叉树的种类、存储方式、遍历方式以及定义，比较全面的介绍了二叉树各个方面的重点，帮助大家扫一遍基础。

**说到二叉树，就不得不说递归，很多同学对递归都是又熟悉又陌生，递归的代码一般很简短，但每次都是一看就会，一写就废。**



## 二叉树的遍历方式

### 1、深度优先遍历

#### 二叉树递归遍历

##### 递归算法的三要素：

1. **确定递归函数的参数和返回值：** 确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数，并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。
2. **确定终止条件：** 写完了递归算法，运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。
3. **确定单层递归的逻辑：** 确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程。

##### 前序遍历：

[144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)（2022.6.16）

```cpp
class Solution {
public:
   void traversal(TreeNode* cur, vector<int>& vec){
       if(cur == nullptr) return;
       vec.push_back(cur->val);
       traversal(cur->left, vec);
       traversal(cur->right, vec);
   }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root, result);
        return result;
    }
};
```

##### 中序遍历

[94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)（2022.6.16）

```cpp
class Solution {
public:
   void traversal(TreeNode* cur, vector<int>& vec){
       if(cur == nullptr) return;
       traversal(cur->left, vec);
       vec.push_back(cur->val);
       traversal(cur->right, vec);
   }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root, result);
        return result;
    }
};
```

##### 后序遍历

[145. 二叉树的后序遍历 - 力扣](https://leetcode.cn/problems/binary-tree-postorder-traversal/)（2022.6.16）

```cpp
class Solution {
public:
   void traversal(TreeNode* cur, vector<int>& vec){
       if(cur == nullptr) return;
       traversal(cur->left, vec);
       traversal(cur->right, vec);
       vec.push_back(cur->val);
   }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root, result);
        return result;
    }
};
```

#### 二叉树迭代遍历

##### 前序遍历

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root){
        stack<TreeNode*> st;
        vector<int> result;
        if(root == nullptr) return result;
        st.push(root);
        while(!st.empty()){
            TreeNode* node = st.top();
            st.pop();
            result.push_back(node->val);
            if(node->right) st.push(node->right);
            if(node->left) st.push(node->left);
        }
        return result;
    }
};
```

##### 中序遍历

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root){
        stack<TreeNode*> st;
        vector<int> result;
        TreeNode* cur = root;
        while(!st.empty() || cur != nullptr){
            if(cur){
                st.push(cur);
                cur = cur->left;
            }else{
                cur = st.top();
                st.pop();
                result.push_back(cur->val);
                cur = cur->right;
            }
        }
        return result;
    }
};
```

##### 后序遍历

```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root){
        stack<TreeNode*> st;
        vector<int> result;
        if(root == nullptr) return result;
        st.push(root);
        while(!st.empty()){
            TreeNode* node = st.top();
            st.pop();
            result.push_back(node->val);
            if(node->right) st.push(node->right);
            if(node->left) st.push(node->left);
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```

#### 二叉树统一迭代遍历

##### 前序遍历

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root){
        stack<TreeNode*> st;
        vector<int> result;
        if(root != nullptr) st.push(root);
        while(!st.empty()){
            TreeNode* node = st.top();
            if(node != nullptr){
                st.pop();
                if(node->right) st.push(node->right);
                if(node->left) st.push(node->left);
                st.push(node);
                st.push(nullptr);    
            }else{
                st.pop();
                node = st.top();
                st.pop();
                result.push_back(node->val);
            }
        }
        return result;
    }
};
```

##### 中序遍历

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root){
        stack<TreeNode*> st;
        vector<int> result;
        if(root != nullptr) st.push(root);
        while(!st.empty()){
            TreeNode* node = st.top();
            if(node != nullptr){
                st.pop();
                if(node->right) st.push(node->right);
                st.push(node);
                st.push(nullptr); 
                if(node->left) st.push(node->left);
            }else{
                st.pop();
                node = st.top();
                st.pop();
                result.push_back(node->val);
            }
        }
        return result;
    }
};
```

##### 后序遍历

```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root){
        stack<TreeNode*> st;
        vector<int> result;
        if(root != nullptr) st.push(root);
        while(!st.empty()){
            TreeNode* node = st.top();
            if(node != nullptr){
                st.pop();
                st.push(node);
                st.push(nullptr); 
                if(node->right) st.push(node->right);
                if(node->left) st.push(node->left);   
            }else{
                st.pop();
                node = st.top();
                st.pop();
                result.push_back(node->val);
            }
        }
        return result;
    }
};
```



### 2、广度优先遍历

[102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)（2022.6.16）

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root){
        queue<TreeNode*> que;
        vector<vector<<int>> result;
        if(root != nullptr) st.push(root);
        while(!que.empty()){
            int size = que.size();
            vector<int> vec;
            for(int i = 0; i < size; ++i){
                TreeNode* node = que.front();
                que.pop();
                vec.push_back(node->val);
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
            result.push_back(vec);
        }
        return result;
    }
};
```

```cpp
//递归法
class Solution {
public:
    void traversal(TreeNode* cur, vector<vector<int>>& result, int depth){
        if(cur == nullptr) return result;
        if(result.size() == depth) result.push_back(vector<int>());
        result[depth].push_back(cur->val);
        traversal(cur->left, result, depth);
        traversal(cur->right, result, depth);
    }
    vector<vector<int>> levelOrder(TreeNode* root){
        vector<int> result;
        int depth = 0;
        traversal(root, result, depth);
        return result;
    }
};
```

[107. 二叉树的层序遍历 II ](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/)（2022.6.16）

```cpp
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root){
        queue<TreeNode*> que;
        vector<vector<<int>> result;
        if(root != nullptr) st.push(root);
        while(!que.empty()){
            int size = que.size();
            vector<int> vec;
            for(int i = 0; i < size; ++i){
                TreeNode* node = que.front();
                que.pop();
                vec.push_back(node->val);
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
            result.push_back(vec);
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```

[199. 二叉树的右视图 ](https://leetcode.cn/problems/binary-tree-right-side-view/)（2022.6.16）

```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root){
        queue<TreeNode*> que;
        vector<int> result;
        if(root != nullptr) st.push(root);
        while(!que.empty()){
            int size = que.size();
            for(int i = 0; i < size; ++i){
                TreeNode* node = que.front();
                que.pop();
                if(i == (size - 1)) result.push_back(node->val);
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
            result.push_back(vec);
        }
        return result;
    }
};
```

[637. 二叉树的层平均值](https://leetcode.cn/problems/average-of-levels-in-binary-tree/)（2022.6.16）

```cpp
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root){
        queue<TreeNode*> que;
        vector<double> result;
        if(root != nullptr) st.push(root);
        while(!que.empty()){
            int size = que.size();
            double sum = 0;
            for(int i = 0; i < size; ++i){
                TreeNode* node = que.front();
                que.pop();
                sum += node->val;
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
            result.push_back(sum / size);
        }
        return result;
    }
};
```

[429. N 叉树的层序遍历 ](https://leetcode.cn/problems/n-ary-tree-level-order-traversal/)（2022.6.16）

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        queue<Node*> que;
        vector<vector<int>> result;
        if(root != nullptr) que.push(root);
        while(!que.empty()){
            int size = que.size();
            vector<int> vec;
            for(int i = 0; i < size; ++i){
                Node* node = que.front();
                que.pop();
                vec.push_back(node->val);
                for(int j = 0; j < node->children[i].size(); ++j){
                    if(node->children[j]) que.push(node->children[j]);
                }
            }
            result.push_back(vec);
        }
        return result;
    }
};
```

[515. 在每个树行中找最大值](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)（2022.6.16）

```cpp
class Solution {
public:
    vector<int> largestValues(TreeNode* root) {
        queue<TreeNode*> que;
        vector<int> result;
        if(root != nullptr) que.push(root);
        while(!que.empty()){
            int size = que.size();
            int maxValue = INT_MIN;
            for(int i = 0; i < size; ++i){
                TreeNode* node = que.front();
                que.pop();
                maxValue = node->val > maxValue ? node->val : maxValue;
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
            result.push_back(maxValue);
        }
        return result;
    }
};    
```

[116. 填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)（2022.6.16）

```cpp
class Solution {
public:
    Node* connect(Node* root) { {
        queue<Node*> que;
        if(root != nullptr) que.push(root);
        while(!que.empty()){
            int size = que.size();
            Node* nodePre;
            Node* node;
            for(int i = 0; i < size; ++i){
                if(i == 0){
                    nodePre = que.front();
                    que.pop();
                    node = nodePre;
                }else{
                    node = que.front();
                    que.pop();
                    nodePre->next = node;
                    nodePre = nodePre->next;
                }
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
            nodePre->next = nullptr;
        }
        return root;
    }
};    
```

[117. 填充每个节点的下一个右侧节点指针 II ](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/)（2022.6.16）

```cpp
class Solution {
public:
    Node* connect(Node* root) { {
        queue<Node*> que;
        if(root != nullptr) que.push(root);
        while(!que.empty()){
            int size = que.size();
            Node* nodePre;
            Node* node;
            for(int i = 0; i < size; ++i){
                if(i == 0){
                    nodePre = que.front();
                    que.pop();
                    node = nodePre;
                }else{
                    node = que.front();
                    que.pop();
                    nodePre->next = node;
                    nodePre = nodePre->next;
                }
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
            nodePre->next = nullptr;
        }
        return root;
    }
};      
```

[104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)（2022.6.16）

```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        queue<TreeNode*> que;
        int depth = 0;
        que.push(root);
        while(!que.empty()){
            int size = que.size();
            depth++;
            for(int i = 0; i < size; ++i){
                TreeNode* node = que.front();
                que.pop();
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
        }
        return depth;
    }
};    
```

[111. 二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)（2022.6.16）

```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        queue<TreeNode*> que;
        int depth = 0;
        que.push(root);
        while(!que.empty()){
            int size = que.size();
            depth++;
            for(int i = 0; i < size; ++i){
                TreeNode* node = que.front();
                que.pop();
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
                if(node->left == nullptr && node->right == nullptr) return depth;
            }
        }
        return depth;
    }
};    
```

## 求二叉树的属性

### 二叉树：是否对称

[101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)（2022.6.17）

#### 递归

后序，比较的是根节点的左子树与右子树是不是相互翻转

```cpp
class Solution {
public:
    bool compare(TreeNode* left, TreeNode* right){
        if(left == nullptr && right == nullptr) return true;
        else if(left == nullptr && right != nullptr) return false;
        else if(left != nullptr && right == nullptr) return false;
        else if(left->val != right->val) return false;
        
        bool outSide = compare(left->left, right->right);
        bool inSide = compare(left->right, right->left);
        bool isSame = outSide && inSide;
        return isSame;
    }
    bool isSymmetric(TreeNode* root) {
        if(root == nullptr) return true;
        return compare(root->left, root->right);
    }
};
```

#### 迭代

使用队列/栈将两个节点顺序放入容器中进行比较

```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root == nullptr) return true;
        queue<TreeNode*> que;
        que.push(root->left);
        que.push(root->right);
        while(!que.empty()){
            TreeNode* leftNode = que.front(); que.pop();
            TreeNode* rightNode = que.front(); que.pop();
            if(!leftNode && !rightNode) continue;
            if(!leftNode || !rightNode || leftNode->val != rightNode->val) return false;
            que.push(leftNode->left);
            que.push(rightNode->right);
            que.push(leftNode->right);
            que.push(rightNode->left);
        }
        return true;
    }
};
```

### 二叉树：求最大深度

[104. 二叉树的最大深度 ](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)（2022.6.17）

#### 递归

后序，求根节点最大高度就是最大深度，通过递归函数的返回值做计算树的高度

```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        return 1 + max(maxDepth(root->left), maxDepth(root->right));
    }
};
```

#### 迭代

层序遍历

```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        queue<TreeNode*> que;
        que.push(root);
        int depth = 0;
        while(!que.empty()){
            int size = que.size();
            depth++;
            for(int i = 0; i < size; ++i){
                TreeNode* node = que.front();
                que.pop();
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
        }
        return depth;
    }
};
```

### N叉树：求最大深度

[559. N 叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-n-ary-tree/)（2022.6.17）

#### 递归

后序，求根节点最大高度就是最大深度，通过递归函数的返回值做计算树的高度

```cpp
class Solution {
public:
    int maxDepth(Node* root) {
        if(root == nullptr) return 0;
        int depth = 0;
        for(int i = 0; i < root->children.size(); ++i){
            depth = max(depth, maxDepth(root->children[i]));
        }
        return 1 + depth;
    }
};
```

#### 迭代

层序遍历

```cpp
class Solution {
public:
    int maxDepth(Node* root) {
        int depth = 0;
        queue<Node*> que;
        if(root != nullptr) que.push(root);
        while(!que.empty()){
            int size = que.size();
            depth++;
            for(int i = 0; i < size; ++i){
                Node* node = que.front();
                que.pop();
                for(int i = 0; i < node-children.size(); ++i){
                    if(node->children[i]) que.push(node->children[i]);
                }
            }
        }
        return depth;
    }
};
```

### 二叉树：求最小深度

[111. 二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)（2022.6.17）

#### 递归

后序，求根节点最小高度就是最小深度，注意最小深度的定义

```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        if(root->left == nullptr && root->right != nullptr) return 1 + minDepth(root->right);
        if(root->left != nullptr && root->right == nullptr) return 1 + minDepth(root->left);
        return 1 + min(minDepth(root->left), minDepth(root->right));
    }
};
```

#### 迭代

层序遍历

```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        queue<TreeNode*> que;
        if(root) que.push(root);
        int depth = 0;
        while(!que.empty()){
            int size = que.size();
            depth++;
            for(int i = 0; i < size; ++i){
                TreeNode* node = que.front();
                que.pop();
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
                if(!node->left && !node->right) return depth;
            }
        }
        return depth;
    }
};
```

### 二叉树：求有多少个节点

[222. 完全二叉树的节点个数 ](https://leetcode.cn/problems/count-complete-tree-nodes/)（2022.6.18）

#### 递归

后序，通过递归函数的返回值计算节点数量

```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        if(root == nullptr) return 0;
        return 1 + countNodes(root->left) + countNodes(root->right);
    }
};
```

- 时间复杂度：O(n)
- 空间复杂度：O(log n)，算上了递归系统栈占用的空间

#### 迭代

层序遍历

```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        queue<TreeNode*> que;
        if(root) que.push(root);
        int count = 0;
        while(!que.empty()){
            int size = que.size();
            for(int i = 0; i < size; ++i){
                TreeNode* node = que.front();
                que.pop();
                count++;
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
        }
        return count;
    }
};
```

- 时间复杂度：O(n)
- 空间复杂度：O(n)

#### 完全二叉树

```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        if(root == nullptr) return 0;
        TreeNode* leftNode = root->left;
        TreeNode* rightNode = root->right;
        int leftHeight = 0, rightHeight = 0;
        while(leftNode){
            leftNode = leftNode->left;
            leftHeight++;
        }
        while(rightNode){
            rightNode = rightNode->right;
            rightHeight++;
        }
        if(leftHeight == rightHeight){
            return (2 << leftHeight) - 1;
        }
        return 1 + countNodes(root->left) + countNodes(root->right);
    }
};
```

- 时间复杂度：O(log n × log n)
- 空间复杂度：O(log n)

### 二叉树：是否平衡

[110. 平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/)（2022.6.18）

#### 递归

后序，注意后序求高度和前序求深度，递归过程判断高度差

```cpp
class Solution {
public:
    int getHeight(TreeNode* root){
        if(root == nullptr) return 0;
        int leftHeight = getHeight(root->left);
        if(leftHeight == -1) return -1;
        int rightHeight = getHeight(root->right);
        if(rightHeight == -1) return -1;
        return abs(leftHeight - rightHeight) > 1 ? -1 : 1 + max(leftHeight, rightHeight);
    }

    bool isBalanced(TreeNode* root) {
        return getHeight(root) == -1 ? false : true;
    }
};
```

### 二叉树：找所有路径

[257. 二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)（2022.6.18）

#### 递归

前序，方便让父节点指向子节点，涉及回溯处理根节点到叶子的所有路径

```cpp
class Solution {
private:
    void traversal(TreeNode* cur, string path, vector<string>& result){
        path += to_string(cur->val);
        if(cur->left == nullptr && cur->right == nullptr){
            result.push_back(path);
            return;
        }
        if(cur->left){
            traversal(cur->left, path + "->", result);
        }
        if(cur->right){
            traversal(cur->right, path + "->", result);
        } 
    }
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<string> result;
        string path;
        if(root == nullptr) return result;
        traversal(root, path, result);
        return result;
    }
};
```

#### 迭代

一个栈模拟递归，一个栈来存放对应的遍历路径

```cpp
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        stack<TreeNode*> treeSt;// 保存树的遍历节点
        stack<string> pathSt;   // 保存遍历路径的节点
        vector<string> result;  // 保存最终路径集合
        if (root == NULL) return result;
        treeSt.push(root);
        pathSt.push(to_string(root->val));
        while (!treeSt.empty()) {
            TreeNode* node = treeSt.top(); treeSt.pop(); // 取出节点 中
            string path = pathSt.top();pathSt.pop();    // 取出该节点对应的路径
            if (node->left == NULL && node->right == NULL) { // 遇到叶子节点
                result.push_back(path);
            }
            if (node->right) { // 右
                treeSt.push(node->right);
                pathSt.push(path + "->" + to_string(node->right->val));
            }
            if (node->left) { // 左
                treeSt.push(node->left);
                pathSt.push(path + "->" + to_string(node->left->val));
            }
        }
        return result;
    }
};
```

### 二叉树：求左叶子之和

[404. 左叶子之和](https://leetcode.cn/problems/sum-of-left-leaves/)（2022.6.18）

#### 递归

后序，必须三层约束条件，才能判断是否是左叶子。

```cpp
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        if (root == NULL) return 0;

        int leftValue = sumOfLeftLeaves(root->left);    // 左
        int rightValue = sumOfLeftLeaves(root->right);  // 右
                                                        // 中
        int midValue = 0;
        if (root->left && !root->left->left && !root->left->right) { // 中
            midValue = root->left->val;
        }
        int sum = midValue + leftValue + rightValue;
        return sum;
    }
};
```

#### 迭代

直接模拟前序遍历

```cpp
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        if(root == nullptr) return 0;
        stack<TreeNode*> st;
        st.push(root);
        int sum = 0;
        while(!st.empty()){
            TreeNode* node = st.top();
            st.pop();
            if(node->left && node->left->left == nullptr && node->left->right ==nullptr){
                sum += node->left->val;
            }
            if(node->left) st.push(node->left);
            if(node->right) st.push(node->right);
        }
        return sum;
    }
};
```

### 二叉树：求左下角的值

[513. 找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/)（2022.6.18）

#### 递归

顺序无所谓，优先左孩子搜索，同时找深度最大的叶子节点。

```cpp
class Solution {
public:
    int maxLen = INT_MIN;
    int maxleftValue;
    void traversal(TreeNode* root, int leftLen) {
        if (root->left == NULL && root->right == NULL) {
            if (leftLen > maxLen) {
                maxLen = leftLen;
                maxleftValue = root->val;
            }
            return;
        }
        if (root->left) {
            traversal(root->left, leftLen + 1); // 隐藏着回溯
        }
        if (root->right) {
            traversal(root->right, leftLen + 1); // 隐藏着回溯
        }
        return;
    }
    int findBottomLeftValue(TreeNode* root) {
        traversal(root, 0);
        return maxleftValue;
    }
};
```

#### 迭代

层序遍历找最后一行最左边

```cpp
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        queue<TreeNode*> que;
        if(root) que.push(root);
        int result = 0;
        while(!que.empty()){
            int size = que.size();
            for(int i = 0; i < size; ++i){
                TreeNode* node = que.front();
                que.pop();
                if(i == 0) result = node->val;
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
        }
        return result;
    }
};
```

### 二叉树：求路径总和

[112. 路径总和](https://leetcode.cn/problems/path-sum/)（2022.6.18）

#### 递归

顺序无所谓，递归函数返回值为bool类型是为了搜索一条边，没有返回值是搜索整棵树。

```cpp
class solution {
private:
    bool traversal(treenode* cur, int count) {
        if (!cur->left && !cur->right && count == 0) return true; // 遇到叶子节点，并且计数为0
        if (!cur->left && !cur->right) return false; // 遇到叶子节点直接返回

        if (cur->left) { // 左
            count -= cur->left->val; // 递归，处理节点;
            if (traversal(cur->left, count)) return true;
            count += cur->left->val; // 回溯，撤销处理结果
        }
        if (cur->right) { // 右
            count -= cur->right->val; // 递归，处理节点;
            if (traversal(cur->right, count)) return true;
            count += cur->right->val; // 回溯，撤销处理结果
        }
        return false;
    }

public:
    bool haspathsum(treenode* root, int sum) {
        if (root == null) return false;
        return traversal(root, sum - root->val);
    }
};
```

#### 迭代

栈里元素不仅要记录节点指针，还要记录从头结点到该节点的路径数值总和

```cpp
class solution {

public:
    bool haspathsum(treenode* root, int sum) {
        if (root == null) return false;
        // 此时栈里要放的是pair<节点指针，路径数值>
        stack<pair<treenode*, int>> st;
        st.push(pair<treenode*, int>(root, root->val));
        while (!st.empty()) {
            pair<treenode*, int> node = st.top();
            st.pop();
            // 如果该节点是叶子节点了，同时该节点的路径数值等于sum，那么就返回true
            if (!node.first->left && !node.first->right && sum == node.second) return true;

            // 右节点，压进去一个节点的时候，将该节点的路径数值也记录下来
            if (node.first->right) {
                st.push(pair<treenode*, int>(node.first->right, node.second + node.first->right->val));
            }

            // 左节点，压进去一个节点的时候，将该节点的路径数值也记录下来
            if (node.first->left) {
                st.push(pair<treenode*, int>(node.first->left, node.second + node.first->left->val));
            }
        }
        return false;
    }
};
```

[113. 路径总和 II ](https://leetcode.cn/problems/path-sum-ii/)（2022.6.18）

```cpp
class solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    // 递归函数不需要返回值，因为我们要遍历整个树
    void traversal(treenode* cur, int count) {
        if (!cur->left && !cur->right && count == 0) { // 遇到了叶子节点且找到了和为sum的路径
            result.push_back(path);
            return;
        }

        if (!cur->left && !cur->right) return ; // 遇到叶子节点而没有找到合适的边，直接返回

        if (cur->left) { // 左 （空节点不遍历）
            path.push_back(cur->left->val);
            count -= cur->left->val;
            traversal(cur->left, count);    // 递归
            count += cur->left->val;        // 回溯
            path.pop_back();                // 回溯
        }
        if (cur->right) { // 右 （空节点不遍历）
            path.push_back(cur->right->val);
            count -= cur->right->val;
            traversal(cur->right, count);   // 递归
            count += cur->right->val;       // 回溯
            path.pop_back();                // 回溯
        }
        return ;
    }

public:
    vector<vector<int>> pathsum(treenode* root, int sum) {
        result.clear();
        path.clear();
        if (root == null) return result;
        path.push_back(root->val); // 把根节点放进路径
        traversal(root, sum - root->val);
        return result;
    }
};
```

## 二叉树的修改与构造

### 翻转二叉树

[226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)（2022.6.19）

- #### 递归

  前序，交换左右孩子

  ```cpp
  class Solution {
  public:
      TreeNode* invertTree(TreeNode* root) {
          if (root == NULL) return root;
          swap(root->left, root->right);  // 中
          invertTree(root->left);         // 左
          invertTree(root->right);        // 右
          return root;
      }
  };
  ```

- #### 迭代

  直接模拟前序遍历

  ```cpp
  class Solution {
  public:
      TreeNode* invertTree(TreeNode* root) {
          if (root == NULL) return root;
          stack<TreeNode*> st;
          st.push(root);
          while(!st.empty()) {
              TreeNode* node = st.top();              // 中
              st.pop();
              swap(node->left, node->right);
              if(node->right) st.push(node->right);   // 右
              if(node->left) st.push(node->left);     // 左
          }
          return root;
      }
  };
  ```

### 构造二叉树

[106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)（2022.6.19）

- #### 递归

  前序，重点在于找分割点，分左右区间构造

  ```cpp
  class Solution {
  private:
      // 中序区间：[inorderBegin, inorderEnd)，后序区间[postorderBegin, postorderEnd)
      TreeNode* traversal (vector<int>& inorder, int inorderBegin, int inorderEnd, vector<int>& postorder, int postorderBegin, int postorderEnd) {
          if (postorderBegin == postorderEnd) return NULL;
  
          int rootValue = postorder[postorderEnd - 1];
          TreeNode* root = new TreeNode(rootValue);
  
          if (postorderEnd - postorderBegin == 1) return root;
  
          int delimiterIndex;
          for (delimiterIndex = inorderBegin; delimiterIndex < inorderEnd; delimiterIndex++) {
              if (inorder[delimiterIndex] == rootValue) break;
          }
          // 切割中序数组
          // 左中序区间，左闭右开[leftInorderBegin, leftInorderEnd)
          int leftInorderBegin = inorderBegin;
          int leftInorderEnd = delimiterIndex;
          // 右中序区间，左闭右开[rightInorderBegin, rightInorderEnd)
          int rightInorderBegin = delimiterIndex + 1;
          int rightInorderEnd = inorderEnd;
  
          // 切割后序数组
          // 左后序区间，左闭右开[leftPostorderBegin, leftPostorderEnd)
          int leftPostorderBegin =  postorderBegin;
          int leftPostorderEnd = postorderBegin + delimiterIndex - inorderBegin; // 终止位置是 需要加上 中序区间的大小size
          // 右后序区间，左闭右开[rightPostorderBegin, rightPostorderEnd)
          int rightPostorderBegin = postorderBegin + (delimiterIndex - inorderBegin);
          int rightPostorderEnd = postorderEnd - 1; // 排除最后一个元素，已经作为节点了
  
          root->left = traversal(inorder, leftInorderBegin, leftInorderEnd,  postorder, leftPostorderBegin, leftPostorderEnd);
          root->right = traversal(inorder, rightInorderBegin, rightInorderEnd, postorder, rightPostorderBegin, rightPostorderEnd);
  
          return root;
      }
  public:
      TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
          if (inorder.size() == 0 || postorder.size() == 0) return NULL;
          // 左闭右开的原则
          return traversal(inorder, 0, inorder.size(), postorder, 0, postorder.size());
      }
  };
  ```

  [105. 从前序与中序遍历序列构造二叉树 ](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)（2022.6.19）

  ```cpp
  class Solution {
  private:
          TreeNode* traversal (vector<int>& inorder, int inorderBegin, int inorderEnd, vector<int>& preorder, int preorderBegin, int preorderEnd) {
          if (preorderBegin == preorderEnd) return NULL;
  
          int rootValue = preorder[preorderBegin]; // 注意用preorderBegin 不要用0
          TreeNode* root = new TreeNode(rootValue);
  
          if (preorderEnd - preorderBegin == 1) return root;
  
          int delimiterIndex;
          for (delimiterIndex = inorderBegin; delimiterIndex < inorderEnd; delimiterIndex++) {
              if (inorder[delimiterIndex] == rootValue) break;
          }
          // 切割中序数组
          // 中序左区间，左闭右开[leftInorderBegin, leftInorderEnd)
          int leftInorderBegin = inorderBegin;
          int leftInorderEnd = delimiterIndex;
          // 中序右区间，左闭右开[rightInorderBegin, rightInorderEnd)
          int rightInorderBegin = delimiterIndex + 1;
          int rightInorderEnd = inorderEnd;
  
          // 切割前序数组
          // 前序左区间，左闭右开[leftPreorderBegin, leftPreorderEnd)
          int leftPreorderBegin =  preorderBegin + 1;
          int leftPreorderEnd = preorderBegin + 1 + delimiterIndex - inorderBegin; // 终止位置是起始位置加上中序左区间的大小size
          // 前序右区间, 左闭右开[rightPreorderBegin, rightPreorderEnd)
          int rightPreorderBegin = preorderBegin + 1 + (delimiterIndex - inorderBegin);
          int rightPreorderEnd = preorderEnd;
  
          root->left = traversal(inorder, leftInorderBegin, leftInorderEnd,  preorder, leftPreorderBegin, leftPreorderEnd);
          root->right = traversal(inorder, rightInorderBegin, rightInorderEnd, preorder, rightPreorderBegin, rightPreorderEnd);
  
          return root;
      }
  
  public:
      TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
          if (inorder.size() == 0 || preorder.size() == 0) return NULL;
  
          // 参数坚持左闭右开的原则
          return traversal(inorder, 0, inorder.size(), preorder, 0, preorder.size());
      }
  };
  ```

### 构造最大的二叉树

[654. 最大二叉树](https://leetcode.cn/problems/maximum-binary-tree/)（2022.6.19）

- #### 递归

  前序，分割点为数组最大值，分左右区间构造

  ```cpp
  class Solution {
  private:
      // 在左闭右开区间[left, right)，构造二叉树
      TreeNode* traversal(vector<int>& nums, int left, int right) {
          if (left >= right) return nullptr;
  
          // 分割点下标：maxValueIndex
          int maxValueIndex = left;
          for (int i = left + 1; i < right; ++i) {
              if (nums[i] > nums[maxValueIndex]) maxValueIndex = i;
          }
  
          TreeNode* root = new TreeNode(nums[maxValueIndex]);
  
          // 左闭右开：[left, maxValueIndex)
          root->left = traversal(nums, left, maxValueIndex);
  
          // 左闭右开：[maxValueIndex + 1, right)
          root->right = traversal(nums, maxValueIndex + 1, right);
  
          return root;
      }
  public:
      TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
          return traversal(nums, 0, nums.size());
      }
  };
  ```

### 合并两个二叉树

[617. 合并二叉树 ](https://leetcode.cn/problems/merge-two-binary-trees/)（2022.6.19）

- #### 递归

  前序，同时操作两个树的节点，注意合并的规则

  ```cpp
  class Solution {
  public:
      TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
          if(root1 == nullptr) return root2;
          if(root2 == nullptr) return root1;
          root1->val += root2->val;
          root1->left = mergeTrees(root1->left, root2->left);
          root1->right = mergeTrees(root1->right, root2->right);
          return root1;
      }
  };
  ```

- #### 迭代

  使用队列，类似层序遍历

  ```cpp
  class Solution {
  public:
      TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
          if(root1 == nullptr) return root2;
          if(root2 == nullptr) return root1;
          queue<TreeNode*> que;
          que.push(root1);
          que.push(root2);
          while(!que.empty()){
              TreeNode* node1 = que.front();
              que.pop();
              TreeNode* node2 = que.front();
              que.pop();
              node1->val += node2->val;
              if(node1->left && node2->left){
                  que.push(node1->left);
                  que.push(node2->left);
              }
              if(node1->right && node2->right){
                  que.push(node1->right);
                  que.push(node2->right);
              }
              if(node1->left == nullptr && node2->left){
                  node1->left = node2->left;
              }
              if(node1->right == nullptr && node2->right){
                  node1->right = node2->right;
              }
          }
          return root1;
      }
  };
  ```

## 求二叉搜索树的属性

### 二叉搜索树中的搜索

[700. 二叉搜索树中的搜索 ](https://leetcode.cn/problems/search-in-a-binary-search-tree/)（2022.6.20）

- #### 递归

```cpp
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if (root == NULL || root->val == val) return root;
        if (root->val > val) return searchBST(root->left, val);
        if (root->val < val) return searchBST(root->right, val);
        return NULL;
    }
};
```

- #### 迭代

```cpp
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        while (root != NULL) {
            if (root->val > val) root = root->left;
            else if (root->val < val) root = root->right;
            else return root;
        }
        return NULL;
    }
};
```

### 验证二叉搜索树

[98. 验证二叉搜索树 ](https://leetcode.cn/problems/validate-binary-search-tree/)（2022.6.20）

- #### 递归

```cpp
class Solution {
private:
    vector<int> vec;
    void traversal(TreeNode* root) {
        if (root == NULL) return;
        traversal(root->left);
        vec.push_back(root->val); // 将二叉搜索树转换为有序数组
        traversal(root->right);
    }
public:
    bool isValidBST(TreeNode* root) {
        vec.clear(); // 不加这句在leetcode上也可以过，但最好加上
        traversal(root);
        for (int i = 1; i < vec.size(); i++) {
            // 注意要小于等于，搜索树里不能有相同元素
            if (vec[i] <= vec[i - 1]) return false;
        }
        return true;
    }
};
```

- #### 迭代

```cpp
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        stack<TreeNode*> st;
        TreeNode* cur = root;
        TreeNode* pre = NULL; // 记录前一个节点
        while (cur != NULL || !st.empty()) {
            if (cur != NULL) {
                st.push(cur);
                cur = cur->left;                // 左
            } else {
                cur = st.top();                 // 中
                st.pop();
                if (pre != NULL && cur->val <= pre->val)
                return false;
                pre = cur; //保存前一个访问的结点

                cur = cur->right;               // 右
            }
        }
        return true;
    }
};
```

### 二叉搜索树的最小绝对差

[530. 二叉搜索树的最小绝对差 ](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)（2022.6.20）

- #### 递归

```cpp
class Solution {
private:
vector<int> vec;
void traversal(TreeNode* root) {
    if (root == NULL) return;
    traversal(root->left);
    vec.push_back(root->val); // 将二叉搜索树转换为有序数组
    traversal(root->right);
}
public:
    int getMinimumDifference(TreeNode* root) {
        vec.clear();
        traversal(root);
        if (vec.size() < 2) return 0;
        int result = INT_MAX;
        for (int i = 1; i < vec.size(); i++) { // 统计有序数组的最小差值
            result = min(result, vec[i] - vec[i-1]);
        }
        return result;
    }
};
```

- #### 迭代

```cpp
class Solution {
public:
    int getMinimumDifference(TreeNode* root) {
        stack<TreeNode*> st;
        TreeNode* cur = root;
        TreeNode* pre = NULL;
        int result = INT_MAX;
        while (cur != NULL || !st.empty()) {
            if (cur != NULL) { // 指针来访问节点，访问到最底层
                st.push(cur); // 将访问的节点放进栈
                cur = cur->left;                // 左
            } else {
                cur = st.top();
                st.pop();
                if (pre != NULL) {              // 中
                    result = min(result, cur->val - pre->val);
                }
                pre = cur;
                cur = cur->right;               // 右
            }
        }
        return result;
    }
};
```

### 二叉搜索树中的众数

[501. 二叉搜索树中的众数](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)（2022.6.20）

- #### 递归

```cpp
//非二叉搜索树
class Solution {
private:

void searchBST(TreeNode* cur, unordered_map<int, int>& map) { // 前序遍历
    if (cur == NULL) return ;
    map[cur->val]++; // 统计元素频率
    searchBST(cur->left, map);
    searchBST(cur->right, map);
    return ;
}
bool static cmp (const pair<int, int>& a, const pair<int, int>& b) {
    return a.second > b.second;
}
public:
    vector<int> findMode(TreeNode* root) {
        unordered_map<int, int> map; // key:元素，value:出现频率
        vector<int> result;
        if (root == NULL) return result;
        searchBST(root, map);
        vector<pair<int, int>> vec(map.begin(), map.end());
        sort(vec.begin(), vec.end(), cmp); // 给频率排个序
        result.push_back(vec[0].first);
        for (int i = 1; i < vec.size(); i++) {
            // 取最高的放到result数组中
            if (vec[i].second == vec[0].second) result.push_back(vec[i].first);
            else break;
        }
        return result;
    }
};
```

```cpp
//二叉搜索树
class Solution {
private:
    int maxCount; // 最大频率
    int count; // 统计频率
    TreeNode* pre;
    vector<int> result;
    void searchBST(TreeNode* cur) {
        if (cur == NULL) return ;

        searchBST(cur->left);       // 左
                                    // 中
        if (pre == NULL) { // 第一个节点
            count = 1;
        } else if (pre->val == cur->val) { // 与前一个节点数值相同
            count++;
        } else { // 与前一个节点数值不同
            count = 1;
        }
        pre = cur; // 更新上一个节点

        if (count == maxCount) { // 如果和最大值相同，放进result中
            result.push_back(cur->val);
        }

        if (count > maxCount) { // 如果计数大于最大值频率
            maxCount = count;   // 更新最大频率
            result.clear();     // 很关键的一步，不要忘记清空result，之前result里的元素都失效了
            result.push_back(cur->val);
        }

        searchBST(cur->right);      // 右
        return ;
    }

public:
    vector<int> findMode(TreeNode* root) {
        count = 0;
        maxCount = 0;
        TreeNode* pre = NULL; // 记录前一个节点
        result.clear();

        searchBST(root);
        return result;
    }
};
```

- #### 迭代

```cpp
class Solution {
public:
    vector<int> findMode(TreeNode* root) {
        stack<TreeNode*> st;
        TreeNode* cur = root;
        TreeNode* pre = NULL;
        int maxCount = 0; // 最大频率
        int count = 0; // 统计频率
        vector<int> result;
        while (cur != NULL || !st.empty()) {
            if (cur != NULL) { // 指针来访问节点，访问到最底层
                st.push(cur); // 将访问的节点放进栈
                cur = cur->left;                // 左
            } else {
                cur = st.top();
                st.pop();                       // 中
                if (pre == NULL) { // 第一个节点
                    count = 1;
                } else if (pre->val == cur->val) { // 与前一个节点数值相同
                    count++;
                } else { // 与前一个节点数值不同
                    count = 1;
                }
                if (count == maxCount) { // 如果和最大值相同，放进result中
                    result.push_back(cur->val);
                }

                if (count > maxCount) { // 如果计数大于最大值频率
                    maxCount = count;   // 更新最大频率
                    result.clear();     // 很关键的一步，不要忘记清空result，之前result里的元素都失效了
                    result.push_back(cur->val);
                }
                pre = cur;
                cur = cur->right;               // 右
            }
        }
        return result;
    }
};
```

### 二叉搜索树中的搜索

[538. 把二叉搜索树转换为累加树](https://leetcode.cn/problems/convert-bst-to-greater-tree/)（2022.6.20）

- #### 递归

```cpp
class Solution {
private:
    int pre; // 记录前一个节点的数值
    void traversal(TreeNode* cur) { // 右中左遍历
        if (cur == NULL) return;
        traversal(cur->right);
        cur->val += pre;
        pre = cur->val;
        traversal(cur->left);
    }
public:
    TreeNode* convertBST(TreeNode* root) {
        pre = 0;
        traversal(root);
        return root;
    }
};
```

- #### 迭代

```cpp
class Solution {
public:
    TreeNode* convertBST(TreeNode* root) {
        stack<TreeNode*> st;
        int pre = 0;
        if(root) st.push(root);
        while(!st.empty()){
            TreeNode* cur = st.top();
            if(cur){
                st.pop();
                if(cur->left) st.push(cur->left);
                st.push(cur);
                st.push(nullptr);
                if(cur->right) st.push(cur->right);
            }else{
                st.pop();
                cur = st.top();
                st.pop();
                cur->val += pre;
                pre = cur->val;
            }
        }
        return root;
    }
};
```

## 二叉树公共祖先问题

### 二叉树的公共祖先问题

[236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)（2022.6.21）

- #### 递归

  后序，回溯，找到左子树出现目标值，右子树节点目标值的节点。

  ```cpp
  class Solution {
  public:
      TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
          if (root == q || root == p || root == NULL) return root;
          TreeNode* left = lowestCommonAncestor(root->left, p, q);
          TreeNode* right = lowestCommonAncestor(root->right, p, q);
          if (left != NULL && right != NULL) return root;
          if (left == NULL) return right;
          return left;
      }
  };
  ```

### 二叉搜索树的公共祖先问题

[235. 二叉搜索树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/)（2022.6.21）

- #### 递归

  顺序无所谓，如果节点的数值在目标区间就是最近公共祖先

  ```cpp
  class Solution {
  public:
      TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
          if (root->val > p->val && root->val > q->val) {
              return lowestCommonAncestor(root->left, p, q);
          } else if (root->val < p->val && root->val < q->val) {
              return lowestCommonAncestor(root->right, p, q);
          } else return root;
      }
  };
  ```

- #### 迭代

  按序遍历

  ```cpp
  class Solution {
  public:
      TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
          while(root) {
              if (root->val > p->val && root->val > q->val) {
                  root = root->left;
              } else if (root->val < p->val && root->val < q->val) {
                  root = root->right;
              } else return root;
          }
          return NULL;
      }
  };
  ```

## 二叉搜索树的修改与构造

### 二叉搜索树中的插入操作

[701. 二叉搜索树中的插入操作](https://leetcode.cn/problems/insert-into-a-binary-search-tree/)

- #### 递归

  顺序无所谓，通过递归函数返回值添加节点

  ```cpp
  class Solution {
  public:
      TreeNode* insertIntoBST(TreeNode* root, int val) {
          if (root == NULL) {
              TreeNode* node = new TreeNode(val);
              return node;
          }
          if (root->val > val) root->left = insertIntoBST(root->left, val);
          if (root->val < val) root->right = insertIntoBST(root->right, val);
          return root;
      }
  };
  ```

- #### 迭代

  按序遍历，需要记录插入父节点，这样才能做插入操作

  ```cpp
  class Solution {
  public:
      TreeNode* insertIntoBST(TreeNode* root, int val) {
          if (root == NULL) {
              TreeNode* node = new TreeNode(val);
              return node;
          }
          TreeNode* cur = root;
          TreeNode* parent = root; // 这个很重要，需要记录上一个节点，否则无法赋值新节点
          while (cur != NULL) {
              parent = cur;
              if (cur->val > val) cur = cur->left;
              else cur = cur->right;
          }
          TreeNode* node = new TreeNode(val);
          if (val < parent->val) parent->left = node;// 此时是用parent节点的进行赋值
          else parent->right = node;
          return root;
      }
  };
  ```

### 二叉搜索树中的删除操作

[450. 删除二叉搜索树中的节点 ](https://leetcode.cn/problems/delete-node-in-a-bst/)（2022.6.21）

- #### 递归

  前序，想清楚删除非叶子节点的情况

  ```cpp
  class Solution {
  public:
      TreeNode* deleteNode(TreeNode* root, int key) {
          if (root == nullptr) return root; // 第一种情况：没找到删除的节点，遍历到空节点直接返回了
          if (root->val == key) {
              // 第二种情况：左右孩子都为空（叶子节点），直接删除节点， 返回NULL为根节点
              if (root->left == nullptr && root->right == nullptr) {
                  ///! 内存释放
                  delete root;
                  return nullptr;
              }
              // 第三种情况：其左孩子为空，右孩子不为空，删除节点，右孩子补位 ，返回右孩子为根节点
              else if (root->left == nullptr) {
                  auto retNode = root->right;
                  ///! 内存释放
                  delete root;
                  return retNode;
              }
              // 第四种情况：其右孩子为空，左孩子不为空，删除节点，左孩子补位，返回左孩子为根节点
              else if (root->right == nullptr) {
                  auto retNode = root->left;
                  ///! 内存释放
                  delete root;
                  return retNode;
              }
              // 第五种情况：左右孩子节点都不为空，则将删除节点的左子树放到删除节点的右子树的最左面节点的左孩子的位置
              // 并返回删除节点右孩子为新的根节点。
              else {
                  TreeNode* cur = root->right; // 找右子树最左面的节点
                  while(cur->left != nullptr) {
                      cur = cur->left;
                  }
                  cur->left = root->left; // 把要删除的节点（root）左子树放在cur的左孩子的位置
                  TreeNode* tmp = root;   // 把root节点保存一下，下面来删除
                  root = root->right;     // 返回旧root的右孩子作为新root
                  delete tmp;             // 释放节点内存（这里不写也可以，但C++最好手动释放一下吧）
                  return root;
              }
          }
          if (root->val > key) root->left = deleteNode(root->left, key);
          if (root->val < key) root->right = deleteNode(root->right, key);
          return root;
      }
  };
  ```


### 修剪二叉搜索树

[669. 修剪二叉搜索树](https://leetcode.cn/problems/trim-a-binary-search-tree/)（2022.6.21）

- #### 递归

  前序，通过递归函数返回值删除节点

  ```cpp
  class Solution {
  public:
      TreeNode* trimBST(TreeNode* root, int low, int high) {
          if (root == nullptr) return nullptr;
          if (root->val < low) return trimBST(root->right, low, high);
          if (root->val > high) return trimBST(root->left, low, high);
          root->left = trimBST(root->left, low, high);
          root->right = trimBST(root->right, low, high);
          return root;
      }
  };
  ```

- #### 迭代

  有序遍历，较复杂

  ```cpp
  
  ```

### 构造二叉搜索树

[108. 将有序数组转换为二叉搜索树 ](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/)（2022.6.21）

- #### 递归

  前序，数组中间节点分割

  ```cpp
  class Solution {
  private:
      TreeNode* traversal(vector<int>& nums, int left, int right) {
          if (left > right) return nullptr;
          int mid = left + ((right - left) / 2);
          TreeNode* root = new TreeNode(nums[mid]);
          root->left = traversal(nums, left, mid - 1);
          root->right = traversal(nums, mid + 1, right);
          return root;
      }
  public:
      TreeNode* sortedArrayToBST(vector<int>& nums) {
          TreeNode* root = traversal(nums, 0, nums.size() - 1);
          return root;
      }
  };
  ```

# 回溯算法

## 回溯法理论基础

**回溯是递归的副产品，只要有递归就会有回溯**，所以回溯法也经常和二叉树遍历，深度优先搜索混在一起，因为这两种方式都是用了递归。

回溯法就是暴力搜索，并不是什么高效的算法，最多再剪枝一下。

回溯算法能解决如下问题：

- 组合问题：N个数里面按一定规则找出k个数的集合
- 排列问题：N个数按一定规则全排列，有几种排列方式
- 切割问题：一个字符串按一定规则有几种切割方式
- 子集问题：一个N个数的集合里有多少符合条件的子集
- 棋盘问题：N皇后，解数独等等

**我在回溯算法系列讲解中就按照这个顺序给大家讲解，可以说深入浅出，步步到位**。

回溯法确实不好理解，所以需要把回溯法抽象为一个图形来理解就容易多了，**在后面的每一道回溯法的题目我都将遍历过程抽象为树形结构方便大家的理解**。

回溯法的模板：

```cpp
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```

## 组合问题

### 组合问题

[77. 组合 - 力扣（LeetCode）](https://leetcode.cn/problems/combinations/)（2022.6.22）

```cpp
class Solution {
private:
    vector<vector<int>> result; // 存放符合条件结果的集合
    vector<int> path; // 用来存放符合条件结果
    void backtracking(int n, int k, int startIndex) {
        if (path.size() == k) {
            result.push_back(path);
            return;
        }
        for (int i = startIndex; i <= n; i++) {
            path.push_back(i); // 处理节点 
            backtracking(n, k, i + 1); // 递归
            path.pop_back(); // 回溯，撤销处理的节点
        }
    }
public:
    vector<vector<int>> combine(int n, int k) {
        result.clear(); // 可以不写
        path.clear();   // 可以不写
        backtracking(n, k, 1);
        return result;
    }
};
```

```cpp
//剪枝优化
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(int n, int k, int startIndex) {
        if (path.size() == k) {
            result.push_back(path);
            return;
        }
        for (int i = startIndex; i <= n - (k - path.size()) + 1; i++) { // 优化的地方
            path.push_back(i); // 处理节点
            backtracking(n, k, i + 1);
            path.pop_back(); // 回溯，撤销处理的节点
        }
    }
public:

    vector<vector<int>> combine(int n, int k) {
        backtracking(n, k, 1);
        return result;
    }
};
```

### 组合总和

#### 组合总和（一）

[216. 组合总和 III](https://leetcode.cn/problems/combination-sum-iii/)（2022.6.22）

```cpp
class Solution {
private:
    vector<vector<int>> result; // 存放结果集
    vector<int> path; // 符合条件的结果
    // targetSum：目标和，也就是题目中的n。
    // k：题目中要求k个数的集合。
    // sum：已经收集的元素的总和，也就是path里元素的总和。
    // startIndex：下一层for循环搜索的起始位置。
    void backtracking(int targetSum, int k, int sum, int startIndex) {
        if (path.size() == k) {
            if (sum == targetSum) result.push_back(path);
            return; // 如果path.size() == k 但sum != targetSum 直接返回
        }
        for (int i = startIndex; i <= 9; i++) {
            sum += i; // 处理
            path.push_back(i); // 处理
            backtracking(targetSum, k, sum, i + 1); // 注意i+1调整startIndex
            sum -= i; // 回溯
            path.pop_back(); // 回溯
        }
    }

public:
    vector<vector<int>> combinationSum3(int k, int n) {
        result.clear(); // 可以不加
        path.clear();   // 可以不加
        backtracking(n, k, 0, 1);
        return result;
    }
};
```

```cpp
//剪枝
class Solution {
private:
    vector<vector<int>> result; // 存放结果集
    vector<int> path; // 符合条件的结果
    void backtracking(int targetSum, int k, int sum, int startIndex) {
        if (sum > targetSum) { // 剪枝操作
            return; // 如果path.size() == k 但sum != targetSum 直接返回
        }
        if (path.size() == k) {
            if (sum == targetSum) result.push_back(path);
            return;
        }
        for (int i = startIndex; i <= 9 - (k - path.size()) + 1; i++) { // 剪枝
            sum += i; // 处理
            path.push_back(i); // 处理
            backtracking(targetSum, k, sum, i + 1); // 注意i+1调整startIndex
            sum -= i; // 回溯
            path.pop_back(); // 回溯
        }
    }

public:
    vector<vector<int>> combinationSum3(int k, int n) {
        result.clear(); // 可以不加
        path.clear();   // 可以不加
        backtracking(n, k, 0, 1);
        return result;
    }
};
```

#### 组合总和（二）

[39. 组合总和](https://leetcode.cn/problems/combination-sum/)（2022.6.22）

```cpp
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& candidates, int target, int sum, int startIndex) {
        if (sum > target) {
            return;
        }
        if (sum == target) {
            result.push_back(path);
            return;
        }

        for (int i = startIndex; i < candidates.size(); i++) {
            sum += candidates[i];
            path.push_back(candidates[i]);
            backtracking(candidates, target, sum, i); // 不用i+1了，表示可以重复读取当前的数
            sum -= candidates[i];
            path.pop_back();
        }
    }
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        result.clear();
        path.clear();
        backtracking(candidates, target, 0, 0);
        return result;
    }
};
```

```cpp
//剪枝
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& candidates, int target, int sum, int startIndex) {
        if (sum == target) {
            result.push_back(path);
            return;
        }

        // 如果 sum + candidates[i] > target 就终止遍历
        for (int i = startIndex; i < candidates.size() && sum + candidates[i] <= target; i++) {
            sum += candidates[i];
            path.push_back(candidates[i]);
            backtracking(candidates, target, sum, i);
            sum -= candidates[i];
            path.pop_back();

        }
    }
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        result.clear();
        path.clear();
        sort(candidates.begin(), candidates.end()); // 需要排序
        backtracking(candidates, target, 0, 0);
        return result;
    }
};
```

#### 组合总和（三）

[40. 组合总和 II - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum-ii/)

```cpp
//使用标记数组去重
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& candidates, int target, int sum, int startIndex, vector<bool>& used) {
        if (sum == target) {
            result.push_back(path);
            return;
        }
        for (int i = startIndex; i < candidates.size() && sum + candidates[i] <= target; i++) {
            // used[i - 1] == true，说明同一树枝candidates[i - 1]使用过
            // used[i - 1] == false，说明同一树层candidates[i - 1]使用过
            // 要对同一树层使用过的元素进行跳过
            if (i > 0 && candidates[i] == candidates[i - 1] && used[i - 1] == false) {
                continue;
            }
            sum += candidates[i];
            path.push_back(candidates[i]);
            used[i] = true;
            backtracking(candidates, target, sum, i + 1, used); // 和39.组合总和的区别1，这里是i+1，每个数字在每个组合中只能使用一次
            used[i] = false;
            sum -= candidates[i];
            path.pop_back();
        }
    }

public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<bool> used(candidates.size(), false);
        path.clear();
        result.clear();
        // 首先把给candidates排序，让其相同的元素都挨在一起。
        sort(candidates.begin(), candidates.end());
        backtracking(candidates, target, 0, 0, used);
        return result;
    }
};
```

```cpp
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& candidates, int target, int sum, int startIndex) {
        if (sum == target) {
            result.push_back(path);
            return;
        }
        for (int i = startIndex; i < candidates.size() && sum + candidates[i] <= target; i++) {
            // 要对同一树层使用过的元素进行跳过
            if (i > startIndex && candidates[i] == candidates[i - 1]) {
                continue;
            }
            sum += candidates[i];
            path.push_back(candidates[i]);
            backtracking(candidates, target, sum, i + 1); // 和39.组合总和的区别1，这里是i+1，每个数字在每个组合中只能使用一次
            sum -= candidates[i];
            path.pop_back();
        }
    }

public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        path.clear();
        result.clear();
        // 首先把给candidates排序，让其相同的元素都挨在一起。
        sort(candidates.begin(), candidates.end());
        backtracking(candidates, target, 0, 0);
        return result;
    }
};

```

### 多个集合求组合

[17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)（2022.6.23）

```cpp
class Solution {
private:
    const string letterMap[10] = {
        "", // 0
        "", // 1
        "abc", // 2
        "def", // 3
        "ghi", // 4
        "jkl", // 5
        "mno", // 6
        "pqrs", // 7
        "tuv", // 8
        "wxyz", // 9
    };
public:
    vector<string> result;
    string s;
    void backtracking(const string& digits, int index) {
        if (index == digits.size()) {
            result.push_back(s);
            return;
        }
        int digit = digits[index] - '0';        // 将index指向的数字转为int
        string letters = letterMap[digit];      // 取数字对应的字符集
        for (int i = 0; i < letters.size(); i++) {
            s.push_back(letters[i]);            // 处理
            backtracking(digits, index + 1);    // 递归，注意index+1，一下层要处理下一个数字了
            s.pop_back();                       // 回溯
        }
    }
    vector<string> letterCombinations(string digits) {
        s.clear();
        result.clear();
        if (digits.size() == 0) {
            return result;
        }
        backtracking(digits, 0);
        return result;
    }
};
```

## 切割问题

[131. 分割回文串 ](https://leetcode.cn/problems/palindrome-partitioning/)（2022.6.23）

**难点：**

- 切割问题其实类似组合问题
- 如何模拟那些切割线
- 切割问题中递归如何终止
- 在递归循环中如何截取子串
- 如何判断回文

```cpp
class Solution {
private:
    vector<vector<string>> result;
    vector<string> path; // 放已经回文的子串
    void backtracking (const string& s, int startIndex) {
        // 如果起始位置已经大于s的大小，说明已经找到了一组分割方案了
        if (startIndex >= s.size()) {
            result.push_back(path);
            return;
        }
        for (int i = startIndex; i < s.size(); i++) {
            if (isPalindrome(s, startIndex, i)) {   // 是回文子串
                // 获取[startIndex,i]在s中的子串
                string str = s.substr(startIndex, i - startIndex + 1);
                path.push_back(str);
            } else {                                // 不是回文，跳过
                continue;
            }
            backtracking(s, i + 1); // 寻找i+1为起始位置的子串
            path.pop_back(); // 回溯过程，弹出本次已经填在的子串
        }
    }
    bool isPalindrome(const string& s, int start, int end) {
        for (int i = start, j = end; i < j; i++, j--) {
            if (s[i] != s[j]) {
                return false;
            }
        }
        return true;
    }
public:
    vector<vector<string>> partition(string s) {
        result.clear();
        path.clear();
        backtracking(s, 0);
        return result;
    }
};
```

## 子集问题

### 子集问题（一）

[78. 子集](https://leetcode.cn/problems/subsets/)（2022.6.23）

```CPP
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums, int startIndex) {
        result.push_back(path); // 收集子集，要放在终止添加的上面，否则会漏掉自己
        if (startIndex >= nums.size()) { // 终止条件可以不加
            return;
        }
        for (int i = startIndex; i < nums.size(); i++) {
            path.push_back(nums[i]);
            backtracking(nums, i + 1);
            path.pop_back();
        }
    }
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        result.clear();
        path.clear();
        backtracking(nums, 0);
        return result;
    }
};

```

### 子集问题（二）

[90. 子集 II ](https://leetcode.cn/problems/subsets-ii/)（2022.6.23）

```cpp
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums, int startIndex) {
        result.push_back(path);
        for (int i = startIndex; i < nums.size(); i++) {
            // 而我们要对同一树层使用过的元素进行跳过
            if (i > startIndex && nums[i] == nums[i - 1] ) { // 注意这里使用i > startIndex
                continue;
            }
            path.push_back(nums[i]);
            backtracking(nums, i + 1);
            path.pop_back();
        }
    }

public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        result.clear();
        path.clear();
        sort(nums.begin(), nums.end()); // 去重需要排序
        backtracking(nums, 0);
        return result;
    }
};
```

### 递增子序列

[491. 递增子序列](https://leetcode.cn/problems/increasing-subsequences/)（2022.6.23）

```cpp
// 使用unordered_set降重
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums, int startIndex) {
        if (path.size() > 1) {
            result.push_back(path);
            // 注意这里不要加return，要取树上的节点
        }
        unordered_set<int> uset; // 使用set对本层元素进行去重
        for (int i = startIndex; i < nums.size(); i++) {
            if ((!path.empty() && nums[i] < path.back())
                    || uset.find(nums[i]) != uset.end()) {
                    continue;
            }
            uset.insert(nums[i]); // 记录这个元素在本层用过了，本层后面不能再用了
            path.push_back(nums[i]);
            backtracking(nums, i + 1);
            path.pop_back();
        }
    }
public:
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        result.clear();
        path.clear();
        backtracking(nums, 0);
        return result;
    }
};
```

```cpp
// 使用数组作为哈希表降重
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking(vector<int>& nums, int startIndex) {
        if (path.size() > 1) {
            result.push_back(path);
        }
        int used[201] = {0}; // 这里使用数组来进行去重操作，题目说数值范围[-100, 100]
        for (int i = startIndex; i < nums.size(); i++) {
            if ((!path.empty() && nums[i] < path.back())
                    || used[nums[i] + 100] == 1) {
                    continue;
            }
            used[nums[i] + 100] = 1; // 记录这个元素在本层用过了，本层后面不能再用了
            path.push_back(nums[i]);
            backtracking(nums, i + 1);
            path.pop_back();
        }
    }
public:
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        result.clear();
        path.clear();
        backtracking(nums, 0);
        return result;
    }
};
```

## 排列问题

### 排列问题（一）

[46. 全排列](https://leetcode.cn/problems/permutations/)（2022.6.24）

```cpp
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking (vector<int>& nums, vector<bool>& used) {
        // 此时说明找到了一组
        if (path.size() == nums.size()) {
            result.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            if (used[i] == true) continue; // path里已经收录的元素，直接跳过
            used[i] = true;
            path.push_back(nums[i]);
            backtracking(nums, used);
            path.pop_back();
            used[i] = false;
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        result.clear();
        path.clear();
        vector<bool> used(nums.size(), false);
        backtracking(nums, used);
        return result;
    }
};
```

### 排列问题（二）

[47. 全排列 II ](https://leetcode.cn/problems/permutations-ii/)（2022.6.24）

```cpp
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;
    void backtracking (vector<int>& nums, vector<bool>& used) {
        // 此时说明找到了一组
        if (path.size() == nums.size()) {
            result.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            // used[i - 1] == true，说明同一树枝nums[i - 1]使用过
            // used[i - 1] == false，说明同一树层nums[i - 1]使用过
            // 如果同一树层nums[i - 1]使用过则直接跳过
            if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) {
                continue;
            }
            if (used[i] == false) {
                used[i] = true;
                path.push_back(nums[i]);
                backtracking(nums, used);
                path.pop_back();
                used[i] = false;
            }
        }
    }
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        result.clear();
        path.clear();
        sort(nums.begin(), nums.end()); // 排序
        vector<bool> used(nums.size(), false);
        backtracking(nums, used);
        return result;
    }
};
```

## 去重问题

## 重新安排行程

[332. 重新安排行程](https://leetcode.cn/problems/reconstruct-itinerary/)（2022.6.24）

```cpp
class Solution {
private:
// unordered_map<出发机场, map<到达机场, 航班次数>> targets
unordered_map<string, map<string, int>> targets;
bool backtracking(int ticketNum, vector<string>& result) {
    if (result.size() == ticketNum + 1) {
        return true;
    }
    for (pair<const string, int>& target : targets[result[result.size() - 1]]) {
        if (target.second > 0 ) { // 记录到达机场是否飞过了
            result.push_back(target.first);
            target.second--;
            if (backtracking(ticketNum, result)) return true;
            result.pop_back();
            target.second++;
        }
    }
    return false;
}
public:
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        targets.clear();
        vector<string> result;
        for (const vector<string>& vec : tickets) {
            targets[vec[0]][vec[1]]++; // 记录映射关系
        }
        result.push_back("JFK"); // 起始机场
        backtracking(tickets.size(), result);
        return result;
    }
};
```

## 棋盘问题

### N皇后问题

[51. N 皇后](https://leetcode.cn/problems/n-queens/) （2022.6.24）

```cpp

```



### 解数独问题

[37. 解数独 ](https://leetcode.cn/problems/sudoku-solver/)（2022.6.24）

```cpp

```



## 性能分析

以下在计算空间复杂度的时候我都把系统栈（不是数据结构里的栈）所占空间算进去。

### 子集问题分析

- 时间复杂度：O(2^n)，因为每一个元素的状态无外乎取与不取，所以时间复杂度为O(2^n)
- 空间复杂度：O(n)，递归深度为n，所以系统栈所用空间为O(n)，每一层递归所用的空间都是常数级别，注意代码里的result和path都是全局变量，就算是放在参数里，传的也是引用，并不会新申请内存空间，最终空间复杂度为O(n)

### 排列问题分析

- 时间复杂度：O(n!)，这个可以从排列的树形图中很明显发现，每一层节点为n，第二层每一个分支都延伸了n-1个分支，再往下又是n-2个分支，所以一直到叶子节点一共就是 n * n-1 * n-2 * ..... 1 = n!。
- 空间复杂度：O(n)，和子集问题同理。

### 组合问题分析

- 时间复杂度：O(2^n)，组合问题其实就是一种子集的问题，所以组合问题最坏的情况，也不会超过子集问题的时间复杂度。
- 空间复杂度：O(n)，和子集问题同理。

### N皇后问题分析

- 时间复杂度：O(n!) ，其实如果看树形图的话，直觉上是O(n^n)，但皇后之间不能见面所以在搜索的过程中是有剪枝的，最差也就是O（n!），n!表示n * (n-1) * .... * 1。
- 空间复杂度：O(n)，和子集问题同理。

### 解数独问题分析

- 时间复杂度：O(9^m) , m是'.'的数目。
- 空间复杂度：O(n^2)，递归的深度是n^2

**一般说到回溯算法的复杂度，都说是指数级别的时间复杂度**



# 贪心算法

## 贪心理论基础

### 什么是贪心

**贪心的本质是选择每一阶段的局部最优，从而达到全局最优**。

### 贪心的套路（什么时候用贪心）

**说实话贪心算法并没有固定的套路**。

所以唯一的难点就是如何通过局部最优，推出整体最优。

那么如何能看出局部最优是否能推出整体最优呢？有没有什么固定策略或者套路呢？

**不好意思，也没有！** 靠自己手动模拟，如果模拟可行，就可以试一试贪心策略，如果不可行，可能需要动态规划。

如何验证可不可以用贪心算法呢？

**最好用的策略就是举反例，如果想不到反例，那么就试一试贪心吧**。

可有有同学认为手动模拟，举例子得出的结论不靠谱，想要严格的数学证明。

一般数学证明有如下两种方法：

- 数学归纳法
- 反证法

**面试中基本不会让面试者现场证明贪心的合理性，代码写出来跑过测试用例即可，或者自己能自圆其说理由就行了**。

**刷题或者面试的时候，手动模拟一下感觉可以局部最优推出整体最优，而且想不到反例，那么就试一试贪心**。

### 贪心一般解题步骤

贪心算法一般分为如下四步：

- 将问题分解为若干个子问题
- 找出适合的贪心策略
- 求解每一个子问题的最优解
- 将局部最优解堆叠成全局最优解

### 总结

**贪心没有套路，就是常识性推导加上举反例**。

最后给出贪心的一般解题步骤，大家可以发现这个解题步骤也是比较抽象的，不像是二叉树，回溯算法，给出了那么具体的解题套路和模板。

## 贪心简单题

- [455. 分发饼干](https://leetcode.cn/problems/assign-cookies/)（2022.6.27）

```cpp
// 时间复杂度：O(nlogn)
// 空间复杂度：O(1)
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int index = s.size() - 1; // 饼干数组的下标
        int result = 0;
        for (int i = g.size() - 1; i >= 0; i--) {
            if (index >= 0 && s[index] >= g[i]) {
                result++;
                index--;
            }
        }
        return result;
    }
};
```

- [1005. K 次取反后最大化的数组和](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/)（2022.6.27）

```cpp
class Solution {
static bool cmp(int a, int b) {
    return abs(a) > abs(b);
}
public:
    int largestSumAfterKNegations(vector<int>& A, int K) {
        sort(A.begin(), A.end(), cmp);       // 第一步
        for (int i = 0; i < A.size(); i++) { // 第二步
            if (A[i] < 0 && K > 0) {
                A[i] *= -1;
                K--;
            }
        }
        if (K % 2 == 1) A[A.size() - 1] *= -1; // 第三步
        int result = 0;
        for (int a : A) result += a;        // 第四步
        return result;
    }
};
```

- [860. 柠檬水找零 ](https://leetcode.cn/problems/lemonade-change/)（2022.6.27）

```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int five = 0, ten = 0, twenty = 0;
        for (int bill : bills) {
            // 情况一
            if (bill == 5) five++;
            // 情况二
            if (bill == 10) {
                if (five <= 0) return false;
                ten++;
                five--;
            }
            // 情况三
            if (bill == 20) {
                // 优先消耗10美元，因为5美元的找零用处更大，能多留着就多留着
                if (five > 0 && ten > 0) {
                    five--;
                    ten--;
                    twenty++; // 其实这行代码可以删了，因为记录20已经没有意义了，不会用20来找零
                } else if (five >= 3) {
                    five -= 3;
                    twenty++; // 同理，这行代码也可以删了
                } else return false;
            }
        }
        return true;
    }
};
```

## 贪心中等题

贪心中等题，靠常识可能就有点想不出来了。开始初现贪心算法的难度与巧妙之处。

- [376. 摆动序列 - 力扣（LeetCode）](https://leetcode.cn/problems/wiggle-subsequence/)（2022.6.28）

```cpp
//贪心算法
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        if (nums.size() <= 1) return nums.size();
        int curDiff = 0; // 当前一对差值
        int preDiff = 0; // 前一对差值
        int result = 1;  // 记录峰值个数，序列默认序列最右边有一个峰值
        for (int i = 0; i < nums.size() - 1; i++) {
            curDiff = nums[i + 1] - nums[i];
            // 出现峰值
            if ((curDiff > 0 && preDiff <= 0) || (preDiff >= 0 && curDiff < 0)) {
                result++;
                preDiff = curDiff;
            }
        }
        return result;
    }
};
```

- 时间复杂度：O(n)
- 空间复杂度：O(1)

- [738. 单调递增的数字 - 力扣（LeetCode）](https://leetcode.cn/problems/monotone-increasing-digits/)（2022.6.28）

```cpp
class Solution {
public:
    int monotoneIncreasingDigits(int N) {
        string strNum = to_string(N);
        // flag用来标记赋值9从哪里开始
        // 设置为这个默认值，为了防止第二个for循环在flag没有被赋值的情况下执行
        int flag = strNum.size();
        for (int i = strNum.size() - 1; i > 0; i--) {
            if (strNum[i - 1] > strNum[i] ) {
                flag = i;
                strNum[i - 1]--;
            }
        }
        for (int i = flag; i < strNum.size(); i++) {
            strNum[i] = '9';
        }
        return stoi(strNum);
    }
};
```

- 时间复杂度：O(n)，n 为数字长度
- 空间复杂度：O(n)，需要一个字符串，转化为字符串操作更方便



### 贪心解决股票问题

大家都知道股票系列问题是动规的专长，其实用贪心也可以解决，而且还不止就这两道题目，但这两道比较典型，我就拿来单独说一说

- [122. 买卖股票的最佳时机 II - 力扣（LeetCode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)（2022.7.5）

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int result = 0;
        for (int i = 1; i < prices.size(); i++) {
            result += max(prices[i] - prices[i - 1], 0);
        }
        return result;
    }
};
```

- 时间复杂度：O(n)
- 空间复杂度：O(1)



- [714. 买卖股票的最佳时机含手续费 - 力扣（LeetCode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)（2022.7.5）

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int result = 0;
        int minPrice = prices[0]; // 记录最低价格
        for (int i = 1; i < prices.size(); i++) {
            // 情况二：相当于买入
            if (prices[i] < minPrice) minPrice = prices[i];

            // 情况三：保持原有状态（因为此时买则不便宜，卖则亏本）
            if (prices[i] >= minPrice && prices[i] <= minPrice + fee) {
                continue;
            }

            // 计算利润，可能有多次计算利润，最后一次计算利润才是真正意义的卖出
            if (prices[i] > minPrice + fee) {
                result += prices[i] - minPrice - fee;
                minPrice = prices[i] - fee; // 情况一，这一步很关键
            }
        }
        return result;
    }
};
```

- 时间复杂度：O(n)
- 空间复杂度：O(1)



### 两个维度权衡问题

在出现两个维度相互影响的情况时，两边一起考虑一定会顾此失彼，要先确定一个维度，再确定另一个一个维度。

- [135. 分发糖果 - 力扣（LeetCode）](https://leetcode.cn/problems/candy/)

```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> candyVec(ratings.size(), 1);
        // 从前向后
        for (int i = 1; i < ratings.size(); i++) {
            if (ratings[i] > ratings[i - 1]) candyVec[i] = candyVec[i - 1] + 1;
        }
        // 从后向前
        for (int i = ratings.size() - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1] ) {
                candyVec[i] = max(candyVec[i], candyVec[i + 1] + 1);
            }
        }
        // 统计结果
        int result = 0;
        for (int i = 0; i < candyVec.size(); i++) result += candyVec[i];
        return result;
    }
};
```



- [406. 根据身高重建队列 - 力扣（LeetCode）](https://leetcode.cn/problems/queue-reconstruction-by-height/)

```cpp
// 版本一
class Solution {
public:
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        if (a[0] == b[0]) return a[1] < b[1];
        return a[0] > b[0];
    }
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort (people.begin(), people.end(), cmp);
        vector<vector<int>> que;
        for (int i = 0; i < people.size(); i++) {
            int position = people[i][1];
            que.insert(que.begin() + position, people[i]);
        }
        return que;
    }
};
```

- 时间复杂度：O(nlog n + n^2)
- 空间复杂度：O(n)

```cpp
// 版本二
class Solution {
public:
    // 身高从大到小排（身高相同k小的站前面）
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        if (a[0] == b[0]) return a[1] < b[1];
        return a[0] > b[0];
    }
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort (people.begin(), people.end(), cmp);
        list<vector<int>> que; // list底层是链表实现，插入效率比vector高的多
        for (int i = 0; i < people.size(); i++) {
            int position = people[i][1]; // 插入到下标为position的位置
            std::list<vector<int>>::iterator it = que.begin();
            while (position--) { // 寻找在插入位置
                it++;
            }
            que.insert(it, people[i]);
        }
        return vector<vector<int>>(que.begin(), que.end());
    }
};
```

- 时间复杂度：O(nlog n + n^2)
- 空间复杂度：O(n)

模拟插队的时候，使用C++中的list（链表）替代了vector(动态数组)，效率会高很多。

**大家也要掌握自己所用的编程语言，理解其内部实现机制，这样才能写出高效的算法！**



## 贪心难题

这里的题目如果没有接触过，其实是很难想到的，甚至接触过，也一时想不出来，所以题目不要做一遍，要多练！

### 贪心解决区间问题

关于区间问题，大家应该印象深刻，有一周我们专门讲解的区间问题，各种覆盖各种去重。

- [55. 跳跃游戏 - 力扣（LeetCode）](https://leetcode.cn/problems/jump-game/)（2022.7.11）

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int cover = 0;
        if (nums.size() == 1) return true; // 只有一个元素，就是能达到
        for (int i = 0; i <= cover; i++) { // 注意这里是小于等于cover
            cover = max(i + nums[i], cover);
            if (cover >= nums.size() - 1) return true; // 说明可以覆盖到终点了
        }
        return false;
    }
};
```



- [45. 跳跃游戏 II - 力扣（LeetCode）](https://leetcode.cn/problems/jump-game-ii/)（2022.7.11）

```cpp
// 版本二
class Solution {
public:
    int jump(vector<int>& nums) {
        int curDistance = 0;    // 当前覆盖的最远距离下标
        int ans = 0;            // 记录走的最大步数
        int nextDistance = 0;   // 下一步覆盖的最远距离下标
        for (int i = 0; i < nums.size() - 1; i++) { // 注意这里是小于nums.size() - 1，这是关键所在
            nextDistance = max(nums[i] + i, nextDistance); // 更新下一步覆盖的最远距离下标
            if (i == curDistance) {                 // 遇到当前覆盖的最远距离下标
                curDistance = nextDistance;         // 更新当前覆盖的最远距离下标
                ans++;
            }
        }
        return ans;
    }
};
```



- [452. 用最少数量的箭引爆气球 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)（2022.7.12）

```cpp
class Solution {
private:
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        return a[0] < b[0];
    }
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        if (points.size() == 0) return 0;
        sort(points.begin(), points.end(), cmp);

        int result = 1; // points 不为空至少需要一支箭
        for (int i = 1; i < points.size(); i++) {
            if (points[i][0] > points[i - 1][1]) {  // 气球i和气球i-1不挨着，注意这里不是>=
                result++; // 需要一支箭
            }
            else {  // 气球i和气球i-1挨着
                points[i][1] = min(points[i - 1][1], points[i][1]); // 更新重叠气球最小右边界
            }
        }
        return result;
    }
};
```

- 时间复杂度：O(nlog n)，因为有一个快排
- 空间复杂度：O(1)，有一个快排，最差情况(倒序)时，需要n次递归调用。因此确实需要O(n)的栈空间



- [435. 无重叠区间 - 力扣（LeetCode）](https://leetcode.cn/problems/non-overlapping-intervals/)（2022.7.12）

```cpp
class Solution {
public:
    // 按照区间右边界排序
    static bool cmp (const vector<int>& a, const vector<int>& b) {
        return a[1] < b[1];
    }
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        if (intervals.size() == 0) return 0;
        sort(intervals.begin(), intervals.end(), cmp);
        int count = 1; // 记录非交叉区间的个数
        int end = intervals[0][1]; // 记录区间分割点
        for (int i = 1; i < intervals.size(); i++) {
            if (end <= intervals[i][0]) {
                end = intervals[i][1];
                count++;
            }
        }
        return intervals.size() - count;
    }
};
```

- 时间复杂度：O(nlog n) ，有一个快排
- 空间复杂度：O(n)，有一个快排，最差情况(倒序)时，需要n次递归调用。因此确实需要O(n)的栈空间



- [763. 划分字母区间 - 力扣（LeetCode）](https://leetcode.cn/problems/partition-labels/)（2022.7.12）

```cpp
class Solution {
public:
    vector<int> partitionLabels(string S) {
        int hash[27] = {0}; // i为字符，hash[i]为字符出现的最后位置
        for (int i = 0; i < S.size(); i++) { // 统计每一个字符最后出现的位置
            hash[S[i] - 'a'] = i;
        }
        vector<int> result;
        int left = 0;
        int right = 0;
        for (int i = 0; i < S.size(); i++) {
            right = max(right, hash[S[i] - 'a']); // 找到字符出现的最远边界
            if (i == right) {
                result.push_back(right - left + 1);
                left = i + 1;
            }
        }
        return result;
    }
};
```

- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$，使用的hash数组是固定大小



- [56. 合并区间 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-intervals/)

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> result;
        if (intervals.size() == 0) return result;
        // 排序的参数使用了lambda表达式
        sort(intervals.begin(), intervals.end(), [](const vector<int>& a, const vector<int>& b){return a[0] < b[0];});

        result.push_back(intervals[0]);
        for (int i = 1; i < intervals.size(); i++) {
            if (result.back()[1] >= intervals[i][0]) { // 合并区间
                result.back()[1] = max(result.back()[1], intervals[i][1]);
            } else {
                result.push_back(intervals[i]);
            }
        }
        return result;
    }
};
```

- 时间复杂度：O(nlog n) ，有一个快排
- 空间复杂度：O(n)，有一个快排，最差情况(倒序)时，需要n次递归调用。因此确实需要O(n)的栈空间



### 其他难题

[53. 最大子数组和 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-subarray/)（2022.7.13）

其实是动态规划的题目，但贪心性能更优，很多同学也是第一次发现贪心能比动规更优的题目。

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int result = INT32_MIN;
        int count = 0;
        for (int i = 0; i < nums.size(); i++) {
            count += nums[i];
            if (count > result) { // 取区间累计的最大值（相当于不断确定最大子序终止位置）
                result = count;
            }
            if (count <= 0) count = 0; // 相当于重置最大子序起始位置，因为遇到负数一定是拉低总和
        }
        return result;
    }
};
```

- 时间复杂度：O(n)
- 空间复杂度：O(1)



[134. 加油站 - 力扣（LeetCode）](https://leetcode.cn/problems/gas-station/)可能以为是一道模拟题，但就算模拟其实也不简单，需要把while用的很娴熟。但其实是可以使用贪心给时间复杂度降低一个数量级。

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int curSum = 0;
        int totalSum = 0;
        int start = 0;
        for (int i = 0; i < gas.size(); i++) {
            curSum += gas[i] - cost[i];
            totalSum += gas[i] - cost[i];
            if (curSum < 0) {   // 当前累加rest[i]和 curSum一旦小于0
                start = i + 1;  // 起始位置更新为i+1
                curSum = 0;     // curSum从0开始
            }
        }
        if (totalSum < 0) return -1; // 说明怎么走都不可能跑一圈了
        return start;
    }
};
```

- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$



最后贪心系列压轴题目[968. 监控二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-cameras/)（2022.7.13）

不仅贪心的思路不好想，而且需要对二叉树的操作特别娴熟，这就是典型的交叉类难题了。

```cpp
class Solution {
private:
    int result;
    int traversal(TreeNode* cur) {
        if (cur == NULL) return 2;
        int left = traversal(cur->left);    // 左
        int right = traversal(cur->right);  // 右
        if (left == 2 && right == 2) return 0;
        else if (left == 0 || right == 0) {
            result++;
            return 1;
        } else return 2;
    }
public:
    int minCameraCover(TreeNode* root) {
        result = 0;
        if (traversal(root) == 0) { // root 无覆盖
            result++;
        }
        return result;
    }
};
```





# 动态规划

## 动态规划理论基础

（2022.5.25）

### 什么是动态规划

动态规划（英语：Dynamic programming，简称 DP），是一种在数学、管理科学、计算机科学、经济学和生物信息学中使用的，通过把原问题分解为相对简单的子问题的方式求解复杂问题的方法。动态规划常常适用于有重叠子问题和最优子结构性质的问题。

如果某一问题有很多重叠子问题，使用动态规划是最有效的。所以动态规划中每一个状态一定是由上一个状态推导出来的，**这一点就区分于贪心**，贪心没有状态推导，而是从局部直接选最优的。

### 适用问题

如果一个问题，可以把所有可能的答案穷举出来，并且穷举出来后，发现存在重叠子问题，就可以考虑使用动态规划。

### 动态规划的解题步骤

1. 确定dp数组（dp table）以及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组

### 动态规划应该如何debug

**找问题的最好方式就是把dp数组打印出来，看看究竟是不是按照自己思路推导的**！

**做动规的题目，写代码之前一定要把状态转移在dp数组上的具体情况模拟一遍，心中有数，确定最后推出的是想要的结果**。

然后再写代码，如果代码没通过就打印dp数组，看看是不是和自己预先推导的哪里不一样。

如果打印出来和自己预先模拟推导是一样的，那么就是自己的递归公式、初始化或者遍历顺序有问题了。

如果和自己预先模拟推导的不一样，那么就是代码实现细节有问题。

## 动态规划经典题目

### 1、斐波那契数

[509. 斐波那契数](https://leetcode.cn/problems/fibonacci-number/)（2022.5.26）

斐波那契数 （通常用 F(n) 表示）形成的序列称为 斐波那契数列 。该数列由 0 和 1 开始，后面的每一项数字都是前面两项数字的和。也就是：

```
F(0) = 0，F(1) = 1
F(n) = F(n - 1) + F(n - 2)，其中 n > 1
给定 n ，请计算 F(n) 。 
```

示例 1：

```
输入：n = 2
输出：1
解释：F(2) = F(1) + F(0) = 1 + 0 = 1
```

示例 2：

```
输入：n = 3
输出：2
解释：F(3) = F(2) + F(1) = 1 + 1 = 2
```

示例 3：

```
输入：n = 4
输出：3
解释：F(4) = F(3) + F(2) = 2 + 1 = 3
```

```c++
class Solution {
public:
    int fib(int n) {
        if(n <= 1) return n;
        int dp[2];
        dp[0] = 0;
        dp[1] = 1;
        for(int i = 2; i <= n; ++i){
            int sum = dp[0] + dp[1];
            dp[0] = dp[1];
            dp[1] = sum;
        }
        return dp[1];
    }
};
```

### 2、爬楼梯

[70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)（2022.5.26）

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

示例 1：

```
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```

示例 2：

```
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```

```c++
class Solution {
public:
    int climbStairs(int n) {
        if(n <= 1) return n;
        int dp[3];
        dp[1] = 1;
        dp[2] = 2;
        for(int i = 3; i <= n; ++i){
            int sum = dp[1] + dp[2];
            dp[1] = dp[2];
            dp[2] = sum;
        }
        return dp[2];
    }
};
```

### 3、使用最小花费爬楼梯

[746. 使用最小花费爬楼梯]((https://leetcode.cn/problems/min-cost-climbing-stairs/))（2022.5.26）

给你一个整数数组 cost ，其中 cost[i] 是从楼梯第 i 个台阶向上爬需要支付的费用。一旦你支付此费用，即可选择向上爬一个或者两个台阶。

你可以选择从下标为 0 或下标为 1 的台阶开始爬楼梯。

请你计算并返回达到楼梯顶部的最低花费。

示例 1：

```
输入：cost = [10,15,20]
输出：15
解释：你将从下标为 1 的台阶开始。
- 支付 15 ，向上爬两个台阶，到达楼梯顶部。
  总花费为 15 。
```

示例 2：

```
输入：cost = [1,100,1,1,1,100,1,1,100,1]
输出：6
解释：你将从下标为 0 的台阶开始。
- 支付 1 ，向上爬两个台阶，到达下标为 2 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 4 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 6 的台阶。
- 支付 1 ，向上爬一个台阶，到达下标为 7 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 9 的台阶。
- 支付 1 ，向上爬一个台阶，到达楼梯顶部。
  总花费为 6 。
```

```c++
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        vector<int> dp(cost.size());
        dp[0] = cost[0];
        dp[1] = cost[1];
        for(int i = 2; i < cost.size(); ++i){
            dp[i] = cost[i] + min(dp[i - 1], dp[i - 2]);
        }
        return min(dp[cost.size() - 1], dp[cost.size() - 2]);
    }
};
```

### 4、不同路径

[62. 不同路径](https://leetcode.cn/problems/unique-paths/submissions/)（2022.5.28）

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for(int i = 0; i < m; ++i) dp[i][0] = 1;
        for(int j = 0; j < n; ++j) dp[0][j] = 1;
        for(int i = 1; i < m; ++i){
            for(int j = 1; j < n; ++j){
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
};
```

### 5、不同路径II

[63. 不同路径 II](https://leetcode.cn/problems/unique-paths-ii/)（2022.5.29）

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

网格中的障碍物和空位置分别用 1 和 0 来表示。

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 0; i < m && obstacleGrid[i][0] == 0; i++) dp[i][0] = 1;
        for (int j = 0; j < n && obstacleGrid[0][j] == 0; j++) dp[0][j] = 1;
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] == 1) continue;
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
};
```

- 时间复杂度：O(n × m)，n、m 分别为obstacleGrid 长度和宽度
- 空间复杂度：O(n × m)



### 6、整数拆分

[343. 整数拆分](https://leetcode.cn/problems/integer-break/)（2022.5.30）

给定一个正整数 n ，将其拆分为 k 个 正整数 的和（ k >= 2 ），并使这些整数的乘积最大化。

返回 你可以获得的最大乘积 。

```cpp
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n + 1);
        dp[2] = 1;
        for (int i = 3; i <= n ; i++) {
            for (int j = 1; j < i - 1; j++) {
                dp[i] = max(dp[i], max((i - j) * j, dp[i - j] * j));
            }
        }
        return dp[n];
    }
};
```

- 时间复杂度：O(n^2)
- 空间复杂度：O(n)



### 7、不同的二叉搜索树

[96. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)（2022.5.31）

```cpp
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n + 1);
        dp[0] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                dp[i] += dp[j - 1] * dp[i - j];
            }
        }
        return dp[n];
    }
};
```

- 时间复杂度：O(n^2)
- 空间复杂度：O(n)



# HOT 100

## Easy

### 1、两数之和

[1. 两数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/two-sum/)（2022.7.15）

```cpp
//暴力枚举
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for(int i = 0; i < nums.size() - 1; ++i){
            for(int j = i + 1; j < nums.size(); ++j){
                if(nums[i] + nums[j] == target){
                    return {i, j};
                }
            }
        }
        return {};
    }
};
```



```cpp
//哈希表
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        std::unordered_map<int, int> map;
        for(int i = 0; i < nums.size(); ++i){
            auto iter = map.find(target - nums[i]);
            if(iter != map.end()){
                return {iter->second, i};
            }
            map.insert(pair<int, int>(nums[i], i));
        }
        return {};
    }
};
```



### 2、有效的括号

[20. 有效的括号 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-parentheses/)（2022.7.15）

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<int> st;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(') st.push(')');
            else if (s[i] == '{') st.push('}');
            else if (s[i] == '[') st.push(']');
            // 第三种情况：遍历字符串匹配的过程中，栈已经为空了，没有匹配的字符了，说明右括号没有找到对应的左括号 return false
            // 第二种情况：遍历字符串匹配的过程中，发现栈里没有我们要匹配的字符。所以return false
            else if (st.empty() || st.top() != s[i]) return false;
            else st.pop(); // st.top() 与 s[i]相等，栈弹出元素
        }
        // 第一种情况：此时我们已经遍历完了字符串，但是栈不为空，说明有相应的左括号没有右括号来匹配，所以return false，否则就return true
        return st.empty();
    }
};
```



### 3、合并两个有序链表

[21. 合并两个有序链表 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-two-sorted-lists/)（2022.7.15）

```cpp
//递归法
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (l1 == nullptr) {
            return l2;
        } else if (l2 == nullptr) {
            return l1;
        } else if (l1->val < l2->val) {
            l1->next = mergeTwoLists(l1->next, l2);
            return l1;
        } else {
            l2->next = mergeTwoLists(l1, l2->next);
            return l2;
        }
    }
};
```



### 4、最大子数组和

[53. 最大子数组和 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-subarray/)（2022.7.15）

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int result = INT32_MIN;
        int count = 0;
        for (int i = 0; i < nums.size(); i++) {
            count += nums[i];
            if (count > result) { // 取区间累计的最大值（相当于不断确定最大子序终止位置）
                result = count;
            }
            if (count <= 0) count = 0; // 相当于重置最大子序起始位置，因为遇到负数一定是拉低总和
        }
        return result;
    }
};
```



### 5、爬楼梯

[70. 爬楼梯 - 力扣（LeetCode）](https://leetcode.cn/problems/climbing-stairs/)（2022.7.15）

```cpp
class Solution {
public:
    int climbStairs(int n) {
        if(n <= 1) return n;
        vector<int> dp(n + 1);
        dp[1] = 1;
        dp[2] = 2;
        for(int i = 3; i <= n; ++i){
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
};
```

- 时间复杂度：$O(n)$
- 空间复杂度：$O(n)$

```cpp
class Solution {
public:
    int climbStairs(int n) {
        if(n <= 1) return n;
        int dp[3];
        dp[1] = 1;
        dp[2] = 2;
        for(int i = 3; i <= n; ++i){
            int sum = dp[1] + dp[2];
            dp[1] = dp[2];
            dp[2] = sum;
        }
        return dp[2];
    }
};
```

- 时间复杂度：$O(n)$
- 空间复杂度：$O(1)$

### 6、二叉树的中序遍历

[94. 二叉树的中序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-inorder-traversal/)（2022.7.18）

```cpp
//递归法
class Solution {
public:
   void traversal(TreeNode* cur, vector<int>& vec){
       if(cur == nullptr) return;
       traversal(cur->left, vec);
       vec.push_back(cur->val);
       traversal(cur->right, vec);
   }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root, result);
        return result;
    }
};
```

```cpp
//统一化迭代
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root){
        stack<TreeNode*> st;
        vector<int> result;
        if(root != nullptr) st.push(root);
        while(!st.empty()){
            TreeNode* node = st.top();
            if(node != nullptr){
                st.pop();
                if(node->right) st.push(node->right);
                st.push(node);
                st.push(nullptr); 
                if(node->left) st.push(node->left);
            }else{
                st.pop();
                node = st.top();
                st.pop();
                result.push_back(node->val);
            }
        }
        return result;
    }
};
```



### 7、对称二叉树

[101. 对称二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/symmetric-tree/)（2022.7.18）

```cpp
//递归
class Solution {
public:
    bool compare(TreeNode* left, TreeNode* right){
        if(left == nullptr && right == nullptr) return true;
        else if(left == nullptr && right != nullptr) return false;
        else if(left != nullptr && right == nullptr) return false;
        else if(left->val != right->val) return false;
        
        bool outSide = compare(left->left, right->right);
        bool inSide = compare(left->right, right->left);
        bool isSame = outSide && inSide;
        return isSame;
    }
    bool isSymmetric(TreeNode* root) {
        if(root == nullptr) return true;
        return compare(root->left, root->right);
    }
};
```



```cpp
//迭代
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root == nullptr) return true;
        queue<TreeNode*> que;
        que.push(root->left);
        que.push(root->right);
        while(!que.empty()){
            TreeNode* leftNode = que.front(); que.pop();
            TreeNode* rightNode = que.front(); que.pop();
            if(!leftNode && !rightNode) continue;
            if(!leftNode || !rightNode || leftNode->val != rightNode->val) return false;
            que.push(leftNode->left);
            que.push(rightNode->right);
            que.push(leftNode->right);
            que.push(rightNode->left);
        }
        return true;
    }
};
```



### 8、二叉树的最大深度

[104. 二叉树的最大深度 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)（2022.7.18）

```cpp
//递归
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        return 1 + max(maxDepth(root->left), maxDepth(root->right));
    }
};
```



```cpp
//BFS
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        queue<TreeNode*> que;
        que.push(root);
        int depth = 0;
        while(!que.empty()){
            int size = que.size();
            depth++;
            for(int i = 0; i < size; ++i){
                TreeNode* node = que.front();
                que.pop();
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
        }
        return depth;
    }
};
```



### 9、买卖股票的最佳时机

[121. 买卖股票的最佳时机 - 力扣（LeetCode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)（2022.7.18）

```cpp

```



### 10、只出现一次的数字

[136. 只出现一次的数字 - 力扣（LeetCode）](https://leetcode.cn/problems/single-number/)（2022.7.18）

```cpp

```

