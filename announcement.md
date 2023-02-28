
## Homework announcement -- Model Checking with Spin 

**Deadline:** Thursday March 16 at 11:59pm

In this homework assignment we will focus on a mutual exclusion algorithm that works for multiple threads and offers FIFO guarantees, i.e., the threads get access to the critical section in the order in which they request to. This is in contrast to Peterson's algorithm, which only works for two threads. However, the queue lock algorithm that we will look at requires some hardware support in the form of an atomic `get-and-increment` operation.

We will provide an informal (in english) description of the algorithm.

Your goal is to formally define it using Promela, and then verify that it satisfies some properties.

### Queue Lock Algorithm

Suppose that you have `NTHREADS` processes that can content for access to the critical section. 

The global state of the algorithm includes an `NTHREADS`-cell array of booleans (that we will call `queue`) and a global counter variable of type `byte` (that we will call `cnt`). It is important that `cnt` be of type `byte` to keep the state space tractably small.

Initially, `cnt` is set to `0` and `queue` is all `0`s except for the first position which is set to `1`.

Each process first atomically gets and then increments `cnt`, storing the current (before the increment) value of `cnt` as a local variable called `my_cnt`. You'll need to use the `atomic` keyword for this. We will refer to the position indicated by `my_cnt` in `queue` as "this process's position in `queue`".

Then the process waits until its position (indicated by `my_cnt`) in `queue` is set to true (since `cnt` can grow larger than the size of the array, we use modulo arithmetic (using the `%` operator) to get a valid array index). When its position is set to true, the process enters the critical section. To leave the critical section, the process must set its position in `queue` to `false` and set the next position in `queue` to `true` (passing control to the next process).

Each process should do the above steps in an infinite loop, continuously trying to access the critical section.

### Part 1a

Formally define the algorithm in Promela. This requires creating a model of the processes together with the necessary global state with which they communicate. For part one, only model two processes.

Start from the template that follows:

```promela
#define NTHREADS 2

bool queue[NTHREADS];
byte cnt = 0;

active [NTHREADS] proctype process()
{
    //Your code here
    //Make sure your process repeatedly tries to enter the loop
    //(you can do this with a `goto` statement).
}
```

Feel free to use Spin to simulate while developing to debug and make sure that your model works as expected. To simulate a file called `my_file.pml` from the command line, use `spin my_file.pml`. It will probably run forever because your processes repeatedly try to enter the critical section. Use `spin -u100 my_file.pml` to simulate it for 100 steps and stop. See [this man page](https://spinroot.com/spin/Man/Spin.html) for more simulation options. You don't have to simulate if you don't want to- in the next part you will formally verify that your model is correct. 

### Part 1b

Augment your model with an LTL specification to check the following three properties:
  - mutual exclusion in the critical section, i.e., only a single process is at the critical section at any time;
  - flag slots (in `queue`) are used in order; and
  - starvation-freedom, i.e., a process that tries to enter the critical section will eventually enter (your LTL formula might need to manually refer to all threads one by one for checking starvation freedom, meaning that your code for checking it might not be parametric with respect to `NTHREADS`).

Use `spin -run my_file.pml` (replacing `my_file.pml` with the name of your .pml file) to check that these three properties are satisfied. The `-run` option is shorthand for telling Spin to generate C code to verify your model, compile that code, and run that code. From our perspective, it just means "try to verify my model".

Check to see if you have any errors in the `spin` report.

Spin might find state cycles in your program. If this error stems from unfair scheduling of processes (perhaps a process is never scheduled) you can safely suppress this error by invoking `spin -run` with the `-f` flag. The `-f` flag stands for "weak fairness" and ensures that every process will be scheduled always eventually. It might help for part 3 to think about why a cycle is found.

Use `spin -run -f my_file.pml`. Make sure that there are no errors in the `spin` report (your model meets the LTL spec).

### Part 2

Change `NTHREADS = 3` and try to verify the properties again. Does any of them fail? Why is that?

Modify your model (you might have to deviate from the English spec) to fix this error and explain why your modification is reasonable. Write your explanation as a comment at the bottom of the .pml file. 

### Part 3 (Optional) 

Credits to Caleb Stanford who suggested this part.

Is it possible to have a mutual exclusion algorithm for multiple processes that does not use an atomic `get-and-increment` but instead only uses reads and writes?

Come up with such an algorithm and verify that it guarantees mutual exclusion. Write your code in a new file called `HW3_3.pml`.

Does your algorithm sacrifice any of the above properties (FIFO, starvation-freedom)?

Hint: Your might be able to use the ideas from Peterson's algorithm as a building block.

### Deliverables
There are 4 files you should submit (and optionally two more if you did part 3). You can upload them to [gradescope](https://www.gradescope.com/courses/517616) (I made a gradescope for this class recently- I'll add everyone to it soon).

Part 1: A file `HW2_1.pml` that contains your Promela code (including LTL formulas and any assertions). Also submit a `.txt` file containing the output of spin when invoked using the `-run` flag. You can easily generate such a `.txt` file by running `spin -run -f HW2_1.pml > HW2_1_output.txt`.

Part 2: A file `HW2_2.pml` that contains your modified Promela code (an explicit indication of the modification would be nice), a brief explanation of the modification and why it is necessary (in a comment at the bottom of the file). Also submit a `.txt` file containing the output of spin when invoked using the `-run` flag. You can easily generate such a `.txt` file by running `spin -run -f HW2_2.pml > HW2_2_output.txt`.

Part 3 (Optional): Your code, Spin's output, and a brief explanation of the algorithm and the sacrificed property (if any).

### Hints

- You can use the `-bfs` flag with `spin -run` when debugging so that spin uses BFS to find the shortest counterexample trace.
- While debugging, you might also find it helpful to simulate your program, i.e., execute a single trace, by using `spin -u100 -p -l my_file.pml`, where `-u100` limits the simulation to 100 steps, and `-p` and `-l` print additional information. 
- The [Spin manual](https://spinroot.com/spin/Man/Manual.html) and [Promela man pages](https://spinroot.com/spin/Man/promela.html) are good resources.

### OH
HW #2 will be due the week after Spring Break, at 11:59pm on March 16. The assignment will be shorter than the Dafny assignment. I will host office hours 1:35-2:30pm on March 14 and 16 (after class). I will also host office hours 3:30-4:30pm on Wednesday March 15. All my OH are in GRW 057. If you can't make these OH times, please send me an email.
