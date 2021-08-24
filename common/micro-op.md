该文件定义了 micro-op 类，其中包括 CtrlSignals 类，同时，micro-op 类也在 HasBoomUOP 虚接口中被定义， HasBoomUOP虚接口继承了 BoomBundle。

### 1.  micro-op 类

1. uopc micro-op code
2. inst     : UInt(32.W)
3. is_rvc  :  Bool
4. iq_type: 使用哪一个发射单元
5. fu_code: 使用哪一个功能单元
6. ctrl        : CtrlSignals 类
7. iw_state: 发射队列（issue window）中的 uop 下一个状态是什么
8.  iw_p1_poisoned 和 iw_p1_poisoned : Bool 操作数1 和操作数2 是否被 load 唤醒
9. is_br is_jalr is_jal is_sfb(short front branch) 是哪一种分支指令
10. br_mask 受哪一条推测式执行指令的控制， br_tag（？？？？）
11. ftq_idx FTQ索引 以找到fetch PC
12. edge_inst : bool 表征该指令跨越了 2 个 fetch packets
13. pc_lob  : UInt(log2Ceil(icBlockBytes))   PC low bits, 和 ftq[ftq_idx] 组合获得 PC。 和指令cache大小对齐，因为他是比fetch 力度更大。（？？？？）
14. taken : bool 是一条分支指令，并且被预测为 taken
15. imm_packed : UInt(LONGEST_IMM_SZ.W) 在译码的时候压缩立即数，然后在执行的时候转换并进行符号扩展
16. csr_addr ： UInt(CSR_ADDR_SZ.W)  // only used for critical path reasons in Exe（？？？？？）
17. rob stq ldq rxq_idx 部件索引
18. pdst prs1 prs2 prs3 ppred 物理寄存器号 pdst_busy prs1_busy prs2_busy prs3_busy ppred_busy 是否忙 ppred(????)是什么
19. stale_pdst 旧目的寄存器（用于回滚）
20. exception ：bool 这条指令是否发生了例外 exc_cause : UInt（Xlen.W），64 bit 太长，有待优化
21. bypassable : 是否可以 bypass ALU 的结果（不包括load csr等？？？）
22. mem_cmd 同步原语和 cache 刷新信号 mem_size mem_signed
23. is_fence is_fencei is_amo 
24. uses_ldq uses_stq
25. is_sys_pc2epc :  是 ECALL  或 断点——把 PC 设置为 EPC 
26. is_unique : 只允许这条指令在流水线中，等待stq 全部执行完成，清空它之后的所有指令（告诉ROB在ROB为空之前，不要ready）
27. flush_on_commit 某些指令需要在提交的时候刷新他们后面的流水线
28. is_sfb_br 和 is_sfb_shadow 短跳
29. ldst_is_rs1 （？？？？）
30. ldst lrs1 lrs2 lrs3 逻辑寄存器号，只用于decode-rename阶段。其中 ldst 也用于回滚
31.  ldst_val ldst有效信号，当 store指令是无效的，或者写 X0寄存器，置为无效
32.  dst_rtype lrs1_rtype lrs2_rtype frs3_en 寄存器类型
33. xcpt_pf_if xcpt_ae_if xcpt_ma_if,  I_TLB, 指令cache访问例外，分支跳转到了非对齐的地址
34. allocate_brtag ： is_br 或 is_jalr
35. rf_wen 寄存器需要写回
36. unsafe 使用了load queue ; store queue 并且不是 fence；是分支指令？ 
37. fu_code_is 判断是否使用了某个功能单元的函数

### 2. CtrlSignals

1. br_type
2. op1_sel
3. op2_sel
4. imm_sel
5. op_fcn 功能码
6. fcn_dw 位数 32/64
7. csr_cmd 是否是 csr 指令
8. is_load 会引起 TLB 地址查询
9. os_sta 会引起 TLB 地址查询
10. is_std