# Custom Computing

## はじめに

### 計算機アーキテクチャとは
- 広義には，機能，性能，コスト要件を満たす計算機をどのように構成するか
    - 例）どのようなCPU，メモリを使うか，それらをどのように接続するか
- 狭義には，ソフトウェアから見たハードウェアの抽象化 = ISA (Instruction Set Architecture)
    - 例）どのような命令セットを持つか，どのようなレジスタを持つか

### Moore's Law
- 約2年ごとに集積回路の集積度が2倍になる法則（というよりかは観測結果）
    - Apple M3 MAX (2023), TSMC 3nm, 92 billion transistors
    - NVIDIA H100 (2023), TSMS 4nm, 80 billion transistors
- ただし近年は鈍化，物理的制約により終焉が近い

### Dennard scaling

- 消費電力が一定の場合，トランジスタのサイズを小さくすると性能が向上する

$$ \text{Power} = \alpha C f V^2 + I V $$

- ここで
    - $\alpha$: キャパシタのスイッチング率
    - $C$: キャパシタの容量
    - $f$: クロック周波数
    - $V$: 電源電圧
    - $I$: **リーク電流**

- トランジスタのゲート幅と長さが $\frac{1}{k}$ になる場合の電力消費を考える🤔
    - 第一項はトータルで一定になる
        - キャパシタの容量 $C$ は $\frac{1}{k}$ 倍
        - 遅延は $\frac{1}{k}$ 倍 => クロック周波数 $f$ は $k$ 倍
        - 電源電圧 $V$ は $\frac{1}{k}$ 倍
    - 一方で，性能を考えると…？🤔
        - 同面積当たりのトランジスタ数は $k^2$ 倍
        - クロック周波数 $f$ は $k$ 倍
        - したがって同じ面積・消費電力で $k^3$ 倍の性能が得られる
   
- しかし，トランジスタのサイズが小さくなるとリーク電流（第二項）が支配的になる
- Power Wall: クロック周波数の上昇に伴って消費電力が増加，加えて冷却も困難に

### Pollack’s rule
- プロセッサの性能は複雑さの平方根に比例する
    - コアの複雑さを $2$ 倍にする => 理想的には性能は $\sqrt{2}$ 倍になる
    - コアの数を $2$ 倍にする => 理想的には性能は $2$ 倍になる
    - シングルコアからマルチコアへの移行

### Post-Moore's Law
- "No free lunch": 万能な解決策は存在しない
- トランジスタのスケーリングはもう限界
    - 技術的要因: Lithography, Power Wall, Heat
    - 経済的要因: fab (fabrication facility) cost
- 要は，汎用・マルチコアプロセッサのこれ以上の性能向上は難しい

### Dark Silicon
- チップ上で電力制約や発熱問題のために動作させることができない領域
    - 8nmプロセスで約50 - 80% [1]

### Domein-Specific Architecture (DSA)
- 特定のアプリケーションドメインに特化した専用計算機アーキテクチャ
- 同じ制約下で汎用プロセッサよりも高性能な計算機を構築可能
- エネルギー消費[2]: 計算
    - Application-aware にデータ移動を最小化，再利用を最大化することが重要
- e.g., Google TPU, Preferred Network MN-Core, Cerebras WSE, GroqChip, SambaNova, etc.
- e.g., FPGA, ASIC, CGRA, etc.


### 計算機のメトリクス
- スループット(Throughput): 1秒あたりの処理量
    - e.g., Network bandwidth [bps], Memory bandwidth[GB/s], Floating-point operations per second [FLOPS]
- レイテンシ(Latency): 1つの処理にかかる時間
    - e.g., Network latency [ms], Memory access latency [ns]
- Clock Cycle per Instruction (CPI): 1命令あたりのクロックサイクル数
- Instructions per Cycle (IPC): 1クロックサイクルあたりの命令数

- 電力(power): 1秒あたりの仕事量
- 電力量(Energy): 1つの処理にかかるエネルギー，電力と時間の積
- 電力性能(Power performance): エネルギーあたり性能，**近年は重要視されている**
    - e.g., [FLOPS/W]
- 低コスト(Low-cost)
- 高信頼性(High reliability)
- ...

### Flynn's taxonomy
- SISD: Single Instruction, Single Data
    - e.g., Single-core CPU
- SIMD: Single Instruction, Multiple Data
    - e.g., Vector processor, GPU
- MISD: Multiple Instruction, Single Data
    - e.g., Multi-core CPU
- MIMD: Multiple Instruction, Multiple Data
    - e.g., FPGA, CGRA

### Amdahl's Law
- 並列化できない部分の割合が性能をボトルネックする

$$ T_\text{per} = T_\text{seq} \left( (1-r) + \frac{r}{N} \right) $$

- $r$: 並列化できる部分の割合
- $N$: 並列化するプロセッサ数
- $T_\text{seq}$: 逐次実行時間
- $T_\text{per}$: 並列実行時間

$$ P_\text{per} = \frac{T_\text{seq}}{T_\text{per}} = \frac{1}{(1-r) + \frac{r}{N}} $$
 
 - $P_\text{per}$: 並列化効率

### Gustafson's Law
- 十分に大きな規模の問題は，効率的に並列化できる

$$ P_\text{per} = N - (1 - N) \times (1-r) $$

### Scalability
- 計算資源を投下することで性能がどれだけ向上するか
- Strong scaling: **問題サイズを固定して**計算資源を増やすことによる性能向上
- Weak scaling: **計算資源あたりの問題サイズを固定して**計算資源を増やすことによる性能向上
- Scale Up: 計算資源の性能を向上させること，高コスト
- Scale Out: 計算資源を増やすこと，低コスト

### Memory Hierarchy
- **Memory Principle**: Small, Fast, Expensive $\rightarrow$ Large, Slow, Cheap
    - レジスタ(数KB, ns以下) 
    - $\rightarrow$ L1キャッシュ(数十KB, 数ns) 
    - $\rightarrow$ L2キャッシュ(数十MB, 数十ns) 
    - ...
    - $\rightarrow$ DRAM(数十GB, 数百ns)
    - $\rightarrow$ ストレージ(数TB, 数ms) 

### Roofline Model
- 視覚的に性能ボトルネックを把握するためのモデル

$$ \text{Performance} = \min \left( \text{Memory-bound}, \text{Compute-bound} \right) $$

- X軸: 算術強度(Arithmetic Intensity) [FLOP/Byte]
- Y軸: 性能 [FLOP/s]
- 演算強度が低い場合はメモリ帯域幅で律速，高い場合は演算性能で律速される(画像はWikipediaより引用)
![Roofline Model](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b1/Example_of_a_Roofline_model.svg/1920px-Example_of_a_Roofline_model.svg.png) 


### Parallelisms
- 並列(parallel) $\subset$ 並行(Concurrent)
- Instruction-level parallelism (ILP): 1つの命令を複数のステージで同時に実行
- Data-level parallelism (DLP): 1つの命令で複数のデータを同時に処理
- Thread-level parallelism (TLP): 複数のコアが複数のスレッドを同時に実行
- ...


### NVIDIA GPU Architecture

- SIMD命令を実行する粗粒度Multi-threadedコア(SIMT, Single Instruction, Multiple Threads)
- ワープ(Warp): 32 lanesのSIMDグループ，各命令は32並列に実行される
- Branch divergence: ワープ内で分岐があると，分岐を含む命令は全てのレーンで実行されるため，性能が低下
- Memory hierarchy
    - private memory: 各スレッドが独立にアクセス可能
    - shared memory: ブロック内のスレッドが共有
    - global memory: 全スレッドがアクセス可能
        - L1 cache, L2 cache, DRAM

### Pipeline
- 1つの命令を複数のステージに分割して同時に実行
- 性能向上
    - $n$: ステージ数， $N$: 入力データ数
    
$$ P_\text{pipe}(N) 
= \frac{T_\text{orig}}{T_\text{pipe}(N)} 
= \frac{ n N }{n + N -1} 
= \frac{ n }{1 + \frac{n-1}{N}} 
\rightarrow n \ (N \rightarrow \infty) $$ 

- パイプラインとしてステージを細かく分割
    - クリティカルパス（最も伝搬遅延の大きいパス）が短くなる
    - 動作可能なクロック周波数が向上し，性能が向上
- Iteration interval (II): 何サイクルごとに次の入力を投下できるか（II=1が理想）
- e.g., 画像のフィルター処理・畳み込み

### Systolic Array
- 複数の処理要素(Processing Element, PE)を行列状に配置する均質なネットワーク
- 各PEは隣接するPEと密にデータをやり取りしながら同時に計算を行う
- e.g., 1D Sorting
- e.g., 2D Matrix multiplication
    - output-stationary: 計算過程の部分和を各PEが保持
    - weight-stationary: 重み行列を各PEが保持，入力データを流し込む（Google TPU）
- 

### FPGA (Field-Programmable Gate Array)
- ユーザがプログラム可能な集積回路，DSAを実現できる．
- 構成要素
    - Logic Block: LUT, FF, MUX, etc.
    - Connection/Switch Block: ルーティング
    - I/O Block: 入出力
    - Memory: Distributed RAM, Block RAM, UltraRAM, External memory
    - DSP (Digital Signal Processor):
    - Clock Management
- どのようにデザインするか
    - Hardware Description Language (HDL): 
        - RTL (Register Transfer Level) design
        - 何をいつどのようにするかをすべて記述，Full controlだが time-consuming
        - e.g., Verilog, VHDL, Chisel
    - High-Level Synthesis (HLS): 
        - 高位の言語からHDLを生成
        - 何をするかだけを記述
        - ただし，directivesによって最適化を制御（OpenMP的）
        - e.g., C, C++, OpenCL, Python
- System-on-Chip (SoC) FPGA
    - HW/SW co-design system
    - FPGA + ARM core
    - e.g., Xilinx Zynq, Intel Stratix 10

### ASIC (Application-Specific Integrated Circuit)
- 特定のアプリケーションに特化した集積回路
- デザインから製造までのコストが高い => 高性能，低消費電力
- FPGAとの比較
    - FPGA: 低コストでプログラム可能，柔軟性が高いが性能が低い
    - ASIC: 高コストだが高性能，低消費電力，高信頼性


### Design Flow
- Input: HDL or HLS
- Synthesis: HDL -> Netlist
    - Parsing: HDL -> Abstract Syntax Tree (AST)
    - Macro detection
    - Finite State Machine (FSM) extraction
    - Low-level optimization
- Technology Mapping: device-independent -> device-specific netlist
- Place and Route
- Bitstream Generation
- Configuration: FPGA


## References
[1] Esmaeilzadeh, Hadi, et al. "Dark silicon and the end of multicore scaling." Proceedings of the 38th annual international symposium on Computer architecture. 2011.

[2] Horowitz, Mark. "1.1 computing's energy problem (and what we can do about it)." 2014 IEEE international solid-state circuits conference digest of technical papers (ISSCC). IEEE, 2014.