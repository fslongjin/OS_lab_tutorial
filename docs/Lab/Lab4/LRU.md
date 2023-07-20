# 最近最久未使用算法（LRU）



## 置换策略与算法原理

​	LRU页面置换算法的置换策略是淘汰最近最久未使用的页面，选择在内存中最长时间没有被访问的页面予以淘汰，而替换此时需要调用的页面。

​	LRU页面置换算法的算法原理是根据页面调入内存后的使用情况来做决策的。无法预估页面将来的使用情况，利用过去的页面使用情况来进行置换的。

​	LRU页面置换算法的一种实现方法是使用一个数组`pages[]`存放当前正在内存中的页面，每一次访问页面就只需要更新`time_access`变量，当需要页面置换的时候，选出`pages[]`中`time_access`最小的页面，然后把他从数组替换成需要调入的页面。

​	※ LRU页面置换算法的另一种比较合理和高效实现方法是利用一个哈希表和一个双向链表来模拟缓存系统。哈希表用于记录每个页面的地址，双向链表用于记录每个页面被访问的先后顺序。每当进程访问页面时，便将该页面从链表中删除，并将它插入到链表头部。这样，链表头部始终是最新被访问页面的地址，而链表尾部则是最近最久没有被使用的页面的地址。

​	LRU页面置换算法的优点是能够较好地反映程序局部性原理，即最近被访问过的页面很可能在不久后再次被访问。因此，LRU算法能够减少缺页次数，提高程序性能。

​	LRU页面置换算法的缺点是实现起来比较复杂，需要维护额外的数据结构来记录每个页面的使用情况，并且需要频繁地更新这些数据结构。因此，LRU算法需要较多的时间和空间开销。



## 实验步骤

​	接上文，在这里继续完成置换算法的实现

```cpp
// 假设页面的结构体定义如下
struct Page {
  int read; // 读标志
  int write; // 写标志
  int time_arrive; // 到达时间
  int time_access; // 访问时间
};

// 定义一个页面置换算法LRU类
class LRU {
private:
  int page_num; // 页面数量
  int mem_size; // 内存大小
  Page* pages; // 页面数组
  Page** memory; // 内存数组，用于存放当前正在内存中的页面
  int page_faults; // 缺页次数

public:
  // 构造函数，根据给定的页面数量，内存大小和页面数组初始化类的成员变量
  LRU(int pn, int ms, Page* ps) {
    page_num = pn;
    mem_size = ms;
    pages = ps;
    memory = new Page*[mem_size]; // 动态分配内存数组的空间
    for (int i = 0; i < mem_size; i++) { // 初始化内存数组，开始时都为空
      memory[i] = nullptr;
    }
    page_faults = 0;
  }

  // 析构函数，释放内存数组的空间
  ~LRU() {
    delete[] memory;
  }

  // 执行页面置换算法的函数，根据给定的页面访问序列进行操作，并输出结果
  void run(Page* query[], int len) {
    for (int i = 0; i < len; i++) { // 遍历所有页面访问序列
      bool hit = false; // 是否命中标志
      for (int j = 0; j < mem_size; j++) { // 遍历所有内存块
        if (memory[j] == query[i]) { // 如果当前访问的页面已经在内存中
          hit = true; // 设置命中标志为真
          memory[j]->time_access = i + 1; // 更新该页面的访问时间为当前序号加一（避免为零）
          break; // 跳出循环
        }
      }
      if (!hit) { // 如果没有命中
        page_faults++; // 缺页次数加一
        int min_time = INT_MAX; // 最小访问时间，初始为最大整数值
        int min_index = -1; // 最小访问时间对应的内存块索引，初始为-1
        for (int k = 0; k < mem_size; k++) { // 遍历所有内存块，寻找最小访问时间的页面
          if (memory[k]->time_access < min_time) { // 如果当前内存块中的页面访问时间小于最小访问时间
            min_time = memory[k]->time_access; // 更新最小访问时间为当前访问时间
            min_index = k; // 更新最小访问时间对应的内存块索引为当前索引
          }
        }
        memory[min_index] = query[i]; // 将当前访问的页面放入内存中，替换最近最久未使用的页面
        memory[min_index]->time_access = i + 1; // 更新该页面的访问时间为当前序号加一（避免为零）
      }
    }

    // 输出结果
    cout << "LRU page replacement algorithm results:" << endl;
    cout << "Page faults: " << page_faults << endl;
    cout << "Memory contents: " << endl;
    for (int i = 0; i < mem_size; i++) {
      cout << "Memory block " << i << ": ";
      if (memory[i] != nullptr) {
        cout << "Page " << memory[i] - pages << endl;
      } else {
        cout << "Empty" << endl;
      }
    }
  }
};
```







## 实验样例





