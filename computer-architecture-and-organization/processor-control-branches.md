<a href="../">Notebook</a> > <a href="./">Computer Architecture & Organization</a> > Processor Control - Branches

# Processor Control - Branches



## 1. Branches

Branches kill the effectiveness of pipelines. It is important to understand how branches impact pipelines and what can be done to mitigate the impact.

##### High-level language constructs that causes branches:

* Conditional statements (e.g., `if-selse`, `switch-case` statements)
  * Conditional branches
* Loops (e.g., `for`, `do-while`, `while` loops)
  * Conditional branches
* Subroutine or function calls and returns
  * Unconditional branches in most cases

##### Grohoski Study

* $1/3$ of all branches are unconditional.
* $1/3$ are conditional loop-closing branches. 
  * e.g., `for` loop (Terminate a loop construct and is taken for the first $n-1$ times of an $n$-times loop.)
* $1/3$ are other conditional branches.



## 2. Branch Penalty

A branch instruction, when taken, loads the processor’s PC with a new non-sequential value, and the pipeline has to be refilled with instructions following the branch target address. The cost (i.e., the additional number of clock cycles) of executing an operation that causes a non-sequential flow of control is known as the **branch penalty**.        

##### Two scenarios of branch penalty:

* **Stall** the pipeline by introducing **bubbles**
* If not stalled and prediction was in correct, the pipeline has to be **flushed**.



## 3. Branch Direction



## 4. The Effect of a Branch on the Pipeline







Since the pipeline doesn't know what's going to happen, it can either:

1. **Pause Fetch and wait**

   Nice thing about this is that no bad decision has been made! It just waits till the decision is made. But, at the same time, there will always be the **lost cycles**.

2. **Take a guess**

   If guessed right there will be no performance degrade (Good!). But, if guessed worng **bubble** (or **hole**) will be introduced in pipeline. 

   Prediction methods:

   * Assume the branch is never taken.                

     e.g., Execute `then` part rather than `else` part in `if then ... else` statement

     This is said to be working approximately 80% of the time. If the branch is not taken, whatever that’s stored in PC will end up being the next instruction to be executed, life is good.

   * Branch prediction using hardware called the **Branch Prediction Table**

     Studies have shown that this works >90%  of the time.

Either way there are some potential issues. It’s the designers’ choice!