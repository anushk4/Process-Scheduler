# Simple Scheduler

- A `Simple scheduler` runs the `NCPU` number of processes, and after `TSLICE` milliseconds, it stops the execution of the running processes and runs the next NCPU processes. This follows a `round-robin` scheduling policy.
- This is repeated till all the processes terminate.
- NCPU and TSLICE are taken as command line parameters along with shell
```
./shell <NCPU> <TSLICE>
```
- Timer interrupts are generated to raise the `SIGALRM` signal after every TSLICE milliseconds, which signals the scheduler to switch the processes.
- Signals such as `SIGSTOP` and `SIGCONT` are used to pause and resume the child's processes.
- The Simple Scheduler is a `daemon`; it is a background process that runs simultaneously along with the shell. It starts scheduling only when SIGCONT is passed to it.

## Priority queues

```
submit ./a.out <PRIORITY>
```
Apart from unbiased round-robin, the order in which the processes execute also depends on the `priority` assigned to it, which is the third command line parameter of the submit command. The range available is `1-4`, and the default priority is 1. Four ready queues are created to classify the processes according to their priorities and run round-robin on them accordingly. The highest priority is assigned to 1 and the lowest to 4.

### Heuristics followed
- It follows `non - preemptive priority scheduling`, that is, if a process of higher priority arrives before the process of lower priority completes its TSLICE, it will wait and will start executing only in the next TSLICE.
- Processes of different priorities cannot run together. First, all the processes of higher priority will run, followed by lower priority. The number of CPUs available has no role in this.
