## 功能概述

fetch target queue（取指目标队列， FTQ）用于存储指令 PC 和分支预测信息。其中的 PC 用于 link 类指令，分支预测信息用于判断 jalr 类指令（寄存器间接跳转）的 mispredict。

注意 jal 类指令在前端即可得到正确的目标地址，不会出现 mispredict，branch 类指令通过对比 uop 信号簇中的 taken 和 实际计算出来的 is_taken（即pc_sel） 比较判断 mispredict。

## 参数

定义了 FtqParameters 案例类（case class），FTQ 默认有 16 个表项，这个参数可以在