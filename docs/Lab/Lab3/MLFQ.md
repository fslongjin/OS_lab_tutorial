# 多级反馈队列调度算法



## 调度策略与算法原理 

​	MLFQ（多级反馈队列调度）是一种进程调度策略，它将进程划分为多个优先级队列，并根据优先级队列的特性进行调度。每个队列具有不同的优先级，且优先级随时间变化。MLFQ调度算法的主要思想是给予较高优先级的进程更多的CPU时间，并允许进程在不同的队列之间移动。

基本原则和策略：

1. 多个队列：MLFQ维护多个优先级队列，通常是一个队列数组，每个队列都有不同的优先级。
2. 优先级调度：较高优先级的队列拥有较短的时间片，即较高的CPU时间分配，以确保高优先级进程能够尽快得到执行。
3. 时间片轮转：每个队列采用时间片轮转调度策略，当一个进程的时间片用完时，它会被移动到较低优先级的队列中继续执行。
4. 优先级提升：为了避免饥饿情况，MLFQ允许进程在队列之间移动。例如，如果一个进程在较低优先级队列中等待了一段时间，它可以被提升到较高优先级队列中以获得更多的CPU时间。
5. 公平性控制：为了避免长时间运行的进程占用所有CPU时间，MLFQ通常采用一些机制来控制进程的优先级提升速度或降低速度，以实现公平性。

​	MLFQ的设计可以根据具体需求进行调整和改进，例如，可以调整队列数量、时间片长度、优先级提升规则等来适应不同的系统环境和应用场景。该调度策略在实践中常用于操作系统中，以平衡系统的吞吐量和响应时间，同时为不同类型的进程提供适当的执行机会。





## 实验步骤

​	接上文，在这里继续完成调度算法的实现:

​	你只需要实现一个MLFQ算法类，然后实现schedule函数，比如下面的方法：

```cpp
void schedule(Process processes[], int n, vector<int> time_quantums) {
    int num_queues = time_quantums.size();
    vector<queue<Process>> readyQueues(num_queues); // 多个优先级的就绪队列

    // 将所有进程按照优先级加入对应的就绪队列
    for (int i = 0; i < n; i++) {
        int priority = processes[i].priority;
        readyQueues[priority].push(processes[i]);
    }

    for (int i = 0; i < num_queues; i++) {
        int time_quantum = time_quantums[i];

        while (!readyQueues[i].empty()) {
            Process currentProcess = readyQueues[i].front();
            readyQueues[i].pop();

            // 执行一个时间片
            if (currentProcess.remaining_time > time_quantum) {
                cout << "执行进程 " << currentProcess.pid << "，执行时间片 " << time_quantum << endl;
                currentProcess.remaining_time -= time_quantum;

                // 降低优先级并将进程重新加入就绪队列
                currentProcess.priority++;
                if (currentProcess.priority < num_queues) {
                    readyQueues[currentProcess.priority].push(currentProcess);
                } else {
                    readyQueues[num_queues - 1].push(currentProcess);
                }
            } else {
                cout << "执行进程 " << currentProcess.pid << "，执行时间片 " << currentProcess.remaining_time << endl;
                currentProcess.remaining_time = 0;
            }

            // 进程执行完毕
            if (currentProcess.remaining_time == 0) {
                cout << "进程 " << currentProcess.pid << " 执行完毕" << endl;
            }
        }
    }
}
```





## 实验结果



