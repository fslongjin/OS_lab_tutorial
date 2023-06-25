# 完全公平调度算法





## 调度策略与算法原理

​	CFS（Completely Fair Scheduler，完全公平调度器）是一种进程调度策略，最早引入于Linux内核2.6.23版本。CFS的目标是在多核系统中提供公平的CPU时间分配，并以纳秒级的精度进行调度。

基本原则和策略：
1. 基于虚拟运行时间：CFS使用了一个称为虚拟运行时间（Virtual Runtime）的概念来衡量进程的运行时间。每个进程都被分配一个虚拟运行时间片，进程的调度顺序由虚拟运行时间的大小决定。
2. 完全公平性：CFS的目标是提供完全公平的调度，即每个进程在一段时间内都能获得相同的CPU时间。这通过动态调整虚拟运行时间来实现。
3. 红黑树结构：CFS内部使用了一个红黑树数据结构来组织就绪队列，其中每个节点表示一个运行中或就绪的进程。红黑树允许高效的插入、删除和搜索操作。
4. 虚拟运行时间更新：当一个进程被选中执行时，它的虚拟运行时间会递增，表示它消耗了一定的CPU时间。其他进程的虚拟运行时间会相应减少，以保持公平性。
5. 完全使用CPU：CFS的目标是尽量让CPU保持100%的利用率，即使只有一个进程在运行，它也会持续消耗CPU时间，而不是空闲状态。

​	CFS调度策略通过动态地调整进程的虚拟运行时间，使得每个进程能够公平地获得CPU时间，从而提供更好的公平性和响应性。它适用于多核系统和服务器环境，能够高效地管理并调度各种类型的任务，确保系统资源的公平分配。



## 实验步骤

​	接上文，在这里继续完成调度算法的实现

​	你只需要实现一个CFS算法类，然后实现schedule函数，比如下面的方法：

```cpp
void scheduleCFS(Process processes[], int n) {
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> minHeap;

    // 初始化运行队列
    for (int i = 0; i < n; i++) {
        minHeap.push(make_pair(processes[i].vruntime, i));
    }

    int current_time = 0;

    while (!minHeap.empty()) {
        pair<int, int> minProcess = minHeap.top();
        minHeap.pop();

        int process_index = minProcess.second;
        Process currentProcess = processes[process_index];

        cout << "执行进程 " << currentProcess.pid << "，执行时间 " << currentProcess.execution_time << endl;

        current_time += currentProcess.execution_time;
        currentProcess.vruntime = current_time;

        cout << "进程 " << currentProcess.pid << " 执行完毕，完成时间 " << current_time << endl;

        if (current_time < 100) {
            minHeap.push(make_pair(currentProcess.vruntime, process_index));
        }
    }
}
```





## 实验结果



