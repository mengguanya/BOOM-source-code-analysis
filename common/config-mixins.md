定义了 BOOM 处理器的配置选项。其中 mixin 是混入的意思，在 Scala 语言中用于组成类。Scala 中 父类只能有一个（通过 extends 关键字实现），但混入可以有多个（通过 with 关键字实现）。

该文件中分别定义了 分支预测配置选项（Branch prediction configs）、处理器核配置选项。

### 1.1 分支预测配置选项

分支预测配置选项包括 **WithTAGELBPD（包括 tage 预测器，默认采用）**、WithBoom2BPD（采用和 BOOM-v2 相同的预测器 ）、WithAlpha21264BPD（采用和 Alpha21264 处理器相同的预测器）、WithSWBPD（包括 SW 预测器）。

### 1.2 处理器核配置选项

处理器核配置选项继承自 rocket 的 Config 类

（目录rocket-chip/api-config-chipsalliance/design/craft/src/config/Config.scala

选项中定义了发射宽度，译码宽度，ROB 表项个数，发射参数（发射队列个数，以及每个发射队列的表项个数），物理寄存器个数，load/store queue表项个数，最多分支数，fetch buffer 表项数量，fetch target queue 表项个数，浮点单元参数（延迟以及是否支持平方根），D-Cache 和 I-Cache 参数配置。

### 1.3 其他配置

提交阶段打印 log, 分支时打印，性能计数器配置， 同步，异步等配置。