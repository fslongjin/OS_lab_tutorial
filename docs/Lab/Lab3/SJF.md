# 非抢占式最短作业优先算法



## 调度策略与算法原理

​	SJF（Shortest Job First，最短作业优先）是一种进程调度策略，它选择执行时间最短的进程来优先执行。在SJF调度策略下，操作系统会根据进程的执行时间长度来决定调度顺序，短作业将被优先执行。

基本原则和策略：
1. 作业长度：SJF调度策略假设我们可以预测每个进程的执行时间。因此，每个进程都被分配了一个执行时间长度。
2. 最短作业优先：在就绪队列中，选择具有最短执行时间的进程来执行。这意味着进程的执行时间越短，越容易获得CPU的执行机会。
3. 非抢占式调度：一旦进程开始执行，它将一直运行到完成，除非它自愿释放CPU或阻塞。
4. 先来先服务：当多个进程具有相同的执行时间时，采用先到先服务（FIFO）原则，即按照进程到达的顺序来决定执行顺序。

​	SJF调度策略旨在减少平均等待时间，因为短作业往往能够更快地完成并释放CPU资源，从而提高系统的吞吐量和响应时间。然而，SJF调度策略需要提前了解每个进程的执行时间，而在实际情况中，通常无法准确预测进程的执行时间。因此，SJF调度策略往往用作一种参考，而非实际应用的主要调度算法。	

​	扩展：抢占式最短作业优先算法是：最短完成时间优先（Shortest Time to Complet First）调度策略。





## 实验步骤

​	接上文，在这里继续完成调度算法的实现

​	你只需要实现一个SJF算法类，然后实现schedule函数，比如下面的方法：

```cpp
bool compareByBurstTime(const Process& p1, const Process& p2) {
    return p1.burst_time < p2.burst_time;
}

void schedule(Process processes[], int n) {
    // 按照执行时间排序进程
    sort(processes, processes + n, compareByBurstTime);

    int current_time = 0;

    for (int i = 0; i < n; i++) {
        Process currentProcess = processes[i];

        cout << "执行进程 " << currentProcess.pid << "，执行时间 " << currentProcess.burst_time << endl;

        current_time += currentProcess.burst_time;

        cout << "进程 " << currentProcess.pid << " 执行完毕，完成时间 " << current_time << endl;
    }
}
```



## 实验结果



