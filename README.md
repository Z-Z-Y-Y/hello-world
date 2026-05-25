# hello-world
# 我的 NPU 系统架构图

... (一些描述文字) ...

```mermaid
flowchart TD
    subgraph "系统外设与接口"
        A1[传感器<br>摄像头/麦克风]
        A2[主存<br>DDR4/DDR5 DRAM]
        A3[外部存储<br>eMMC/UFS/SSD]
        A4[网络与显示<br>PCIe / USB / HDMI]
    end

    subgraph SoC [片上系统 (SoC) - 系统集成视角]
        subgraph "总线与互联"
            B1[系统总线<br>AMBA APB/AHB]
            B2[高性能总线<br>AMBA AXI / CHI]
            B3[一致性互联<br>CCI/CMN]
        end

        subgraph "CPU复合体 - Control Plane"
            C1[多核CPU<br>Cortex-A/M/R]
            C2[中断控制器<br>GIC]
            C3[内存管理单元<br>MMU/TLB]
        end

        subgraph "存储器子系统 - Memory Plane"
            D1[CPU专用缓存<br>L1/L2]
            D2[末级共享缓存<br>LLC]
            D3[片上SRAM]
            D4[内存控制器<br>DRAM Controller]
        end

        subgraph "加速器 - Data Plane (异构计算)"
            subgraph NPU_Core [可重构NPU]
                E1[全局调度器<br>任务分发与配置]
                E2[可重构互联网络<br>脉动阵列↔广播模式]
                E3[PE阵列<br>支持多模态模式]
                E4[专用控制单元<br>ReLU/池化/量化]
                E5[片上暂存器<br>Scratchpad Memory]
            end
            G[DSP]
            H[专用硬件<br>ISP/VPU]
        end

        subgraph "I/O与内存管理"
            I1[内存管理单元<br>SMMU/IOMMU]
            J[DMA引擎]
            K[I/O桥接器]
        end
    end

    %% 接口连接
    A1 --> K
    A2 --> D4
    A3 --> K
    A4 --> K

    %% SoC内部互联
    B1 --- B2
    B2 --- B3
    B3 --- C1
    B3 --- D2
    B3 --- K
    B3 --- J
    C1 --- B1
    C1 --- D1
    D1 --- D2
    D2 --- D4
    D3 --- B2
    E1 --- B2
    E1 --- E2
    E2 --- E3
    E3 --- E4
    E3 --- E5
    G --- B2
    H --- B2
    I1 --- B2
    J --- B2
    K --- B2

    %% 数据流
    D4 <--> A2
    J <--> D4
    E5 <--> J
```
