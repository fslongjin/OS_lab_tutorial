# 先进先出算法



## 置换策略与算法原理

​	FIFO页面置换算法的置换策略是淘汰最先进入内存的页面，选择在内存中驻留最久的页面予以淘汰，而替换此时需要调用的页面。

​	FIFO页面置换算法的算法原理是需把一个进程已调入内存的页面按先后顺序链接成一个队列，设置一个指针（替换指针），将指针指向最久调入的页面，若在此队列中已存在，则不进行替换；否则将替换指针所指向的页面出队，新调用的页面入队。

​	FIFO页面置换算法的简单实现：可以通过维护一个队列结构去存储当前调入的页面；这样，当发生缺页中断时，需要进行置换的时候，淘汰队列头的页面，并把新调入的页面放入到队列尾部。

​	FIFO页面置换算法的优点是其实现起来比较简单，可以不需要硬件的支持，因而不需要增加系统的成本。

​	FIFO页面置换算法的缺点是没有考虑到缓存页面被使用的情况。如果一个页面被频繁访问, 我们应该将它保留在缓存中, 这样就能够提高程序的性能。但是使用FIFO算法, 很可能将一个被频繁访问的页面清除出缓存, 所以FIFO算法在实际的应用中是很少被使用到的。

​	FIFO页面置换算法还可能出现Belady现象，即分配给进程的物理块数增加时，缺页率反而提高。这是因为FIFO算法将最早调入的页调出，而调出的页在不久可能会被重新使用出现反复调入调出，缺页率反而上升。



## 实验步骤

（1）因为FIFO算法中没有利用到`read`、`write`的标记，所以我们可以简答使用int类型整数来记录虚页号和实页号完成置换算法的实现：

```cpp
// 定义一个页面置换算法FIFO类
class FIFO {
private:
  int page_num; // 页面数量
  int mem_size; // 内存大小
  int* memory; // 内存数组
  int page_faults; // 缺页次数
  int mem_index; // 内存指针

public:
  // 构造函数，根据给定的 内存大小（也就是实页数量） 初始化类的成员变量
  FIFO(int ms) {
    mem_size = ms;
    memory = new int[ms];
  }

  ~FIFO() {
    delete memory;
  }

  // 执行页面置换算法的函数，根据给定的页面访问序列进行操作，并输出结果
  void run(int query[], int len) {
    // 初始化
    page_faults = 0;
    mem_index = 0;
    memset(memory, -1, sizeof(int) * mem_size);

    for (int i = 0; i < len; i++) { // 遍历所有页面访问序列
      bool hit = false; // 是否命中标志
      for (int j = 0; j < mem_size; j++) { // 遍历所有内存块
        if (memory[j] == query[i]) { // 如果当前访问的页面已经在内存中
          hit = true; // 设置命中标志为真
          break; // 跳出循环
        }
      }
      if (!hit) { // 如果没有命中
        cout << "Page_Fault query[" << i << "] virutal_page_number: " << query[i] << endl;
        page_faults++; // 缺页次数加一
        memory[mem_index] = query[i]; // 将当前访问的页面放入内存中，替换最先进入的页面
        mem_index = (mem_index + 1) % mem_size; // 更新内存指针，循环移动
      }
    }

    // 输出结果
    cout << "FIFO page replacement algorithm results:" << endl;
    cout << "Page faults: " << page_faults << endl;
    cout << "Memory contents: " << endl;
    for (int i = 0; i < mem_size; i++) {
      cout << "Memory block " << i << ": ";
      if (memory[i] != 0) {
        cout << "Page " << memory[i] << endl;
      } else {
        cout << "Empty" << endl;
      }
    }
  }
};
```



（2）使用main函数编写测试样例：

```cpp
int main() {
    // 在该测试样例中，使用了2个实页，以及FIFO算法，导致7次访问全都page_fault

    FIFO sched(2);
    int query[] = {0, 1, 2, 0, 1, 2, 3};
    sched.run(query, 7);
}
```







## 实验样例

<img src="../../.vuepress/public/FIFO_result.png" alt="image-20230713200134638" style="zoom:80%;" />



