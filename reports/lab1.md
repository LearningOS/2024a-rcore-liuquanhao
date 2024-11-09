## 简单总结实现的功能

TaskControlBlock 维护开始时间stime和syscall调用次数。

syscall调用时，调用`TASK_MANAGER.syscall_times_inc`增加一次记录。

task更新状态为`TaskStatus::Running`时，stime初始化时间值就记录上。

调用`sys_task_info`时，返回`TaskInfo`信息，其中`time`信息用当前时间减去task的stime做实时计算。

## 简答作业

### 1

```shell
[kernel] PageFault in application, bad addr = 0x0, bad instruction = 0x804003a4, kernel killed it.
[kernel] IllegalInstruction in application, kernel killed it.
[kernel] IllegalInstruction in application, kernel killed it.
```

错误分别为：往`0x0`地址写入属于越界错误;U级调用`sret`会操作S级寄存器，属于越权操作;U级操作`sstatus`寄存器属于越权。

### 2

#### 2.1 L40：刚进入 __restore 时，a0 代表了什么值。请指出 __restore 的两种使用情景

a0的作用有传惨和返回值。

    a. __restore常用于从trap恢复
    b. 线程上下文切换


#### 2.2 L43-L48：这几行汇编代码特殊处理了哪些寄存器？这些寄存器的的值对于进入用户态有何意义？请分别解释

sstatus： 当前是S还是U级，存储此类状态

sepc： trap返回后下一条地址

sscratch： 保持sp栈指针

#### 2.3 L50-L56：为何跳过了 x2 和 x4

x2有sscratch自己维护栈指针，不需要恢复;x4没有用到，不用恢复。

#### 2.4 L60：该指令之后，sp 和 sscratch 中的值分别有什么意义

sp指向用户栈;sscrach指向内核栈

#### 2.5 __restore：中发生状态切换在哪一条指令？为何该指令执行之后会进入用户态

`sret`，它会利用`spec`、`sstatus`等寄存器切换环境。

#### 2.6 L13：该指令之后，sp 和 sscratch 中的值分别有什么意义

`sp`将指向内核栈;`sscratch`将指向用户栈

#### 2.7 U 态进入 S 态是哪一条指令发生的

`ecall`

## 荣誉准则

在完成本次实验的过程（含此前学习的过程）中，我曾分别与 以下各位 就（与本次实验相关的）以下方面做过交流，还在代码中对应的位置以注释形式记录了具体的交流对象及内容：

无

此外，我也参考了 以下资料 ，还在代码中对应的位置以注释形式记录了具体的参考来源及内容：

无

3. 我独立完成了本次实验除以上方面之外的所有工作，包括代码与文档。 我清楚地知道，从以上方面获得的信息在一定程度上降低了实验难度，可能会影响起评分。

4. 我从未使用过他人的代码，不管是原封不动地复制，还是经过了某些等价转换。 我未曾也不会向他人（含此后各届同学）复制或公开我的实验代码，我有义务妥善保管好它们。 我提交至本实验的评测系统的代码，均无意于破坏或妨碍任何计算机系统的正常运转。 我清楚地知道，以上情况均为本课程纪律所禁止，若违反，对应的实验成绩将按“-100”分计。