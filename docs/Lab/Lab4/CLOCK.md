# 时钟页面置换算法(CLOCK)



## 置换策略与算法原理

​	CLOCK页面置换算法的置换策略是淘汰最近未使用的页面，选择在内存中最长时间没有被访问的页面予以淘汰，而替换此时需要调用的页面。

​	CLOCK页面置换算法的算法原理是利用一个循环队列来模拟内存中的页面，并为每个页面设置一个访问位。当某个页面首次装入内存中时，或者该页面被访问到时，将其访问位设置为1。当需要淘汰一个页面时，从当前指针所指的位置开始，检查每个页面的访问位。如果是0，则选择该页面换出，并将新页面放入该位置，并将其访问位设置为1；如果是1，则将其访问位置为0，并继续检查下一个位置。如果一轮扫描后所有页面的访问位都是1，则将所有访问位清零，并重新扫描。

​	CLOCK页面置换算法的优点是实现起来比较简单，只需要维护一个循环队列和一个指针，不需要移动队列中的元素。CLOCK算法也是一种LRU置换算法的近似算法，能够较好地反映程序局部性原理。

**扩展：**

​	CLOCK页面置换算法的缺点是没有考虑到页面是否被修改过的情况，如果一个页面被修改过，则在换出时需要写回磁盘，这会增加I/O开销。

​	因此，有一种改进型CLOCK算法，除了访问位外，还增加了一个修改位，用于记录每个页面是否被修改过，并根据不同的情况进行优先级排序。





## 实验步骤

​	接上文，在这里继续完成置换算法的实现。

```cpp
// 假设页面的结构体定义如下
struct Page {
  int read; // 读标志
  int write; // 写标志
  int time_arrive; // 到达时间
  int access; // 访问位
};

// 定义一个页面置换算法CLOCK类
class CLOCK {
private:
  int page_num; // 页面数量
  int mem_size; // 内存大小
  Page* pages; // 页面数组
  Page** memory; // 内存数组，用于存放当前正在内存中的页面
  int pointer; // 指针，用于指向当前检查的位置
  int page_faults; // 缺页次数

public:
  // 构造函数，根据给定的页面数量，内存大小和页面数组初始化类的成员变量
  CLOCK(int pn, int ms, Page* ps) {
    page_num = pn;
    mem_size = ms;
    pages = ps;
    memory = new Page*[mem_size]; // 动态分配内存数组的空间
    for (int i = 0; i < mem_size; i++) { // 初始化内存数组，开始时都为空
      memory[i] = nullptr;
    }
    pointer = 0; // 初始化指针为0，指向第一个位置
    page_faults = 0;
  }

  // 析构函数，释放内存数组的空间
  ~CLOCK() {
    delete[] memory;
  }

  // 执行页面置换算法的函数，根据给定的页面访问序列进行操作，并输出结果
  void run(Page* query[], int len) {
    for (int i = 0; i < len; i++) { // 遍历所有页面访问序列
      bool hit = false; // 是否命中标志
      for (int j = 0; j < mem_size; j++) { // 遍历所有内存块
        if (memory[j] == query[i]) { // 如果当前访问的页面已经在内存中
          hit = true; // 设置命中标志为真
          memory[j]->access = 1; // 更新该页面的访问位为1
          break; // 跳出循环
        }
      }
      if (!hit) { // 如果没有命中
        page_faults++; // 缺页次数加一
        bool replaced = false; // 是否替换标志
        while (!replaced) { // 循环直到找到一个可以替换的位置为止
          if (memory[pointer]->access == 0) { // 如果当前指针所指的位置的访问位为0，则可以替换该位置的页面
            memory[pointer] = query[i]; // 将当前访问的页面放入该位置，并将其访问位设置为1
            memory[pointer]->access = 1;
            replaced = true; // 设置替换标志为真，跳出循环
          } else { // 如果当前指针所指的位置的访问位为1，则将其访问位清零，并继续检查下一个位置
            memory[pointer]->access = 0;
            pointer = (pointer + 1) % mem_size; // 指针向前移动一位，如果到达末尾，则回到开头，形成循环队列
          }
        }
      }
    }

    // 输出结果
    cout << "CLOCK page replacement algorithm results:" << endl;
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





