**CPU-bound workloads**

**IO-Bound workloads**

- Database call 
- Disk IO
- sort of thing that would cause the path of execution to have to wait for something

In IO workload scenario, having more threads per core can be really great


. What Go has done, what the brilliance is of this scheduler is Go has turned I/O bound work into CPU bound work. When you have CPU bound work, more threads than cores can only add load because we don't need the extra context, which is those corse will never be idle to do other work because this thread has always got work to do.