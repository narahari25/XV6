## Implemented Scheduling Algorithms

## First-Come, First-Served (FCFS)

   - **Explanation**:
        FCFS (First-Come, First-Served) prioritizes processes based on their arrival time, with the first process that arrives being the first one to execute. FCFS operates on a non-preemptive basis. FCFS is straightforward but may not always be the most efficient scheduling algorithm, as it can lead to the "convoy effect".

    - **Implementation**:

            The code runs an infinite loop (for(;;)) to continuously schedule processes which ensures that interrupts are enabled (intr_on()) to prevent potential deadlock.

            It maintains a pointer minProc to keep track of the process with the minimum creation time (ctime) among the runnable processes.

            It iterates through the proc array to find the process with the minimum ctime. The ctime represents the creation time of the process, and the code aims to select the process that has been in the runnable state for the longest time.

            Once the process with the minimum ctime is found (minProc), it acquires the process's lock to ensure exclusive access.

            It checks if the process is still in the RUNNABLE state, and if so, it increments the no_of_times_scheduled counter, changes the process's state to RUNNING, and switches the context to the selected process.

            After the process completes its execution, it sets the c->proc pointer back to 0 and releases the process's lock.

    - **Performance**:
        FCFS may not provide optimal system performance in all scenarios. FCFS has the convoy effect, where long-running processes can block shorter processes from executing, leading to inefficiencies. FCFS is not suitable for situations where some processes are time-sensitive or have higher priority requirements.
       



### Multi-Level Feedback Queue (MLFQ)
- **Explanation**:
    MLFQ (Multi-Level Feedback Queue) processes into multiple priority queues and employs a feedback mechanism to promote or demote processes between queues based on their behavior and execution history.
    MLFQ is designed to provide a balance between priority-based scheduling and fairness. It aims to ensure that CPU time is allocated fairly to all processes while allowing for priority adjustments based on their recent behavior.
    
- **Implementation**:

     In MLFQ, processes are initially placed in a queue with a higher priority , and if they use their time quantum without blocking, they are moved to a lower-priority queue. 
     
        The code runs an infinite loop (while(1)) to continuously schedule processes and  ensures that interrupts are enabled (intr_on()) to prevent potential deadlock.

        It maintains a pointer Selectproc to keep track of the process to be selected for execution.

        The code divides processes into different queues based on their queno values. Higher values of queno correspond to lower-priority queues.

        It iterates through the proc array to adjust the priority of processes based on their entrytime and queno values. If a process has exceeded a time limit (TIME_LIMIT) in the current queue, it is moved to a lower-priority queue, and its no_ticks_que and entrytime are updated.
        After a particular time(waiting time) the priorities are promoted.(Aging - to prevent that the process do not remain in the low priority queue always)

        After adjusting the priorities, it iterates through the proc array again to select a process for execution. The process with the highest-priority queue (queno) is chosen. If multiple processes share the same highest priority, the process with the earliest entrytime is selected.

        If a process is selected (Selectproc != 0), it acquires the process's lock, changes its state to RUNNING, and executes the process using swtch(). After execution, it updates the process's entrytime, increments its scheduling counter, and releases the lock.

- **Performance**:
    MLFQ is designed to provide a balance between fairness and responsiveness. It ensures that processes that use their time quantum efficiently move to lower-priority queues, allowing other processes to have a chance at the CPU.
    The algorithm is effective in preventing resource monopolization by long-running processes.
    MLFQ may not perform optimally in all scenarios and may require tuning to achieve the desired balance between fairness and responsiveness.
    
### Setup:
- Test Environment: 1 CPU
- Number of Processes: NFORK

### Results:
- RR:
  - Average Running Time: `10-11`
  - Average Waiting Time: `142`

- FCFS:
  - Average Running Time: `22`
  - Average Waiting Time: `100`

- MLFQ:
  - Average Running Time: `11-12`
  - Average Waiting Time: `144`




### Conclusion:
-Both round robin and MLFQ have almost same Avg run time and wait time but round robin is a bit more efficient. Comapared to round robin and MLFQ , FCFS is not better algorithm for scheduling.