core.scala 是 BOOM 处理器的顶层文件，它定义了 BoomCore 类，用于连接 frontend，lsu, rocc， tlb, fpu到处理器核（core, 在这里指流水线后端）。可以发现 根据 BOOM 的定义 前端 lsu 等部件并不属于 core。

### 1. 交互接口（io）

1. hartid : input 硬件线程
2. interrupts : input
3. ifu ptw rocc lsu ptw_tlb 使用 flipped 直接把相应部件的 IO 翻转
4. trace : output
5. fcsr_rm

### 2. 端口定义

1. 浮点流水线, 浮点流水线有独立的发射窗口，寄存器堆和运算单元

2. 寄存器文件堆读写口
3. 快速唤醒端口
4. 发射唤醒端口数量
5. 重命名唤醒端口数量
6. 浮点唤醒端口

### 3. 例化功能单元

1. 执行单元。
2. decode 单元 ，使用了 yield 和 for 循环， 例化了 decodeWidth 个decode 单元，该参数在config-mixins.scala 定义。
3. 分支掩码产生逻辑。个数等于流水线宽度（根据config-mixins.scala 应该是等于译码宽度的）
4. 重命名阶段。个数等于流水线宽度。需要指定物理寄存器个数，重命名唤醒端口个数
5. 发射单元。包括访存发射单元和整型发射单元。个数在config-mixins.scala中定义
6. 分发单元。
7. 寄存器文件。iregfile 和 pregfile。需要指定物理寄存器个数，读写端口个数，位宽和 bypass
8. 写回仲裁器 ll_wbarb。
9. 寄存器读端口。处理读寄存器逻辑和 bypass 逻辑
10. ROB。

### 4.  流水线状态（reg 和 wire）

1. decode / rename1 stage  : valid uops fire ready xcpts stalls  等于流水线宽度（corewidth）
2. renames / dispatch stage: valid uops fire ready 等于流水线宽度（corewidth）
3. issue / register read          : valid uops bypasses pred_bypasses

### 5. 处理分支结果

1. 每个 ALU 对应一个 brinfos -> BrResolutionInfo
2. brupdate 把所有的 brinfos 汇总一起
3. mask 计算 ？？？
4. 遍历 brinfos， 根据 rob 号找到最老的 mispredict

### 6. lsu 和 exeUnit

把在前面定义的memory 单元的接口和 core 的接口接起来。

### 7. 打印信息

### 8. 流水线

#### 8.1 前端

youngest_com_idx ？？？ 

