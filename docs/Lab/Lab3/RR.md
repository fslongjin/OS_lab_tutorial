# 时间片轮转算法



## 调度策略与算法原理

​	RR（Round-Robin，轮转调度）是一种常见的进程调度策略，它以时间片（固定时间段）为单位，依次为每个进程分配CPU时间。每个进程在就绪队列中按照到达顺序排队，并且每个进程都能够在一个时间片内获得一定的CPU执行时间，然后被放回队列尾部继续等待执行。

基本原则和策略：
1. 时间片：RR调度策略将CPU时间划分为固定的时间片。当一个进程获得CPU执行时，它被允许执行一个时间片的长度，然后被放回队列等待下一次执行。
2. 队列调度：进程按照到达顺序排队在就绪队列中，每个进程依次获得执行机会。当一个进程的时间片用完后，==它被放回队列的尾部==，下一个进程开始执行。
3. 循环执行：RR调度策略按照循环的方式执行进程，每个进程都能获得一定的CPU时间，以确保公平性。
4. 非抢占式调度：一个进程在执行过程中不会被强制中断或抢占，直到它的时间片用完。

​	RR调度策略具有公平性和响应性，因为每个进程都能获得一定的执行时间，并且长时间运行的进程不会占用所有的CPU时间。然而，如果时间片过小，会导致频繁的上下文切换，增加系统开销；如果时间片过大，会影响系统对紧急任务的响应时间。

​	RR调度策略在操作系统中被广泛应用，尤其是在分时系统和交互式系统中，它能够合理分配CPU时间，提供良好的用户体验。





## 实验步骤

​	接上文，在这里继续完成调度算法的实现

​	你只需要实现一个RR算法类，然后实现schedule函数，比如下面的方法：

```cpp
void schedule(Process processes[], int n, int time_slice) {
    queue<Process> readyQueue;

    // 将所有进程加入就绪队列
    for (int i = 0; i < n; i++) {
        readyQueue.push(processes[i]);
    }

    while (!readyQueue.empty()) {
        Process currentProcess = readyQueue.front();
        readyQueue.pop();

        // 执行一个时间片
        if (currentProcess.remaining_time > time_slice) {
            cout << "执行进程 " << currentProcess.pid << "，执行时间片 " << time_slice << endl;
            currentProcess.remaining_time -= time_slice;
        } else {
            cout << "执行进程 " << currentProcess.pid << "，执行时间片 " << currentProcess.remaining_time << endl;
            currentProcess.remaining_time = 0;
        }

        // 将已执行完的进程重新加入就绪队列
        if (currentProcess.remaining_time > 0) {
            readyQueue.push(currentProcess);
        }
    }
}
```





## 实验结果





