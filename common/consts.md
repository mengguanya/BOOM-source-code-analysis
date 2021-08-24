这个文件中使用 trait （特征，类似于java 中的接口）定义了常量。

### 1. 发射队列类型

包括整型队列、访存队列、浮点队列和 浮点访存队列（？）

### 2. 标量操作

1. BRSC 来自哪一个分支预测器的预测（三级预测cycle 1, 2, 3，以及来自内核的即后端更新）。
2. CFI（control flow instruction）是哪一种控制流指令，包括branch， jal 和 jalr。
3. PC select signal，分支跳转类指令在执行阶段选择 目标地址，包括 PC 加 4 （PC_PLUS4）, 分支 和 直接跳转 目标地址（PC_BRJMP）和 寄存器间接跳转（PC_JALR）。 
4. 分支跳转类型。包括 不跳转， 相等跳转，不等跳转，大于等于，小于，无符号大于等于，无符号小于，直接跳转，寄存器间接跳转。
5. 操作数1选择信号。包括OP1_RS1（选择1号寄存器读取的数据）、OP1_ZERO（全零）、OP1_PC（当前指令的 PC，从 fetch target queue 中读取）
6. 操作数2选择信号。包括OP2_RS2（选择2号寄存器读取的数据）、OP2_IMM、 OP2_ZERO（全零）、OP1_NEXT（用于PC + 4 / 2），OP2_IMMC（for CSR imm found in RS1 ??? 官方注释）
7. 寄存器文件是否允许写。REN_0 和 REN_1
8. 32bit 字 OR 64 bit 双字。BW_X（XLen == 64） , DW_32 DW_64 DW_XPR(XLen == 64)
9. 是否允许访存。MEM_0 和 MEM_1
10. 立即数扩展方式选择。load 和 ALU, store, branch, lui 和 auipc, j 和 jal
11. decode 阶段控制信号。RT_FIX, RT_FLT RT_PAS（物理寄存器号等于逻辑寄存器号）
12.  微码操作码（micro-op 操作码）
13. bubble 指令的定义（？？？）
14. 定义了 空 MicroOp 。使用 def 定义 NullMicroOp 函数，返回空 微码。

### 3. RISCV 常量

1. 各寄存器号和立即数在指令中的位域。
2. 内存一致性模型。C/C++ 原子 MCM 要求对相同地址的 load 保序。
3. 一些函数。从指令中获得 微码操作码，目的寄存器，一号源寄存器。
4. 扩展压缩至零函数
5. 计算分支目标地址函数（压缩指令需要完成扩展后才能使用）
6. 计算跳转目标地址函数（压缩指令需要完成扩展后才能使用）
7. 获取 CFI 类型

### 4.  异常原因（exception cause）

ExcCauseConstants 接口中定义了新的异常原因，即 

MINI_EXCEPTION_MEM_ORDERING （a memory disambigious misspeculation occurred）