# Flexible Systolic Array Platform on Virtual 2-D Multi-FPGA Plane [1]



## Abstract
- Systolic arrayは高い並列性をもたらす有望なアプローチ
- しかし，大きなWorkloadに対しては，より小さな部分に分割して処理する必要がある
- そこで，複数のFPGAを仮想ネットワークで接続した2次元FPGA平面に基づくScalableなSystolic array platformを提案
    - Targetに応じて形状やサイズを自由にカスタマイズ可能
    - Off-chip memoryを介した分散メモリアクセスとシンプルなStream処理
- 提案するplatformの初期実装とその性能評価
    - FPGAの数に比例して処理性能が向上
    - 回路面積が小さいためスケーラビリティが高い
    - 処理性能がネットワーク帯域幅に依存
        - 最新の高帯域幅FPGAボードによって性能が向上できる可能性

## Introduction

- Systolic array による計算の高速化は近年注目を集めている
- e.g., 大規模言語モデル（LLMs）のような近年の大規模AI
    - 固定サイズのSysolic arrayで適切なサイズに分割されたWorkload処理することが一般的
    - e.g., Google TPU, AWS Inferentia
    - ただし，これは一般のSystolic arrayに対して最適な設計とは言えない
- FPGAはSystolic arrayを実装するためのプラットフォームとしてよく使用される
    - FPGAは再構成可能，異なるサイズや形状の柔軟なSystolic arrayプラットフォームを提供
    - ただし，リソースと帯域幅の制約がある
        - FPGA上に実装されるSystolic arrayのサイズは，利用可能なリソースの量に制約される
        - FPGA上のデータ供給の帯域幅が不十分であれば性能は向上しない
    - そこで，複数のFPGAを使用することが考えられるが…？
        - ただ複数のFPGAを使用するだけでは，実用性が低く，性能がスケーラブルではない
        - たとえば，ネットワークスイッチを使用せずに直接接続されたネットワークで複数のFPGAを使用する場合，FPGAネットワークの構成が固定されるため，Systolic arrayのカスタマイズ性が大幅に低下する
        - 一方，Ethernetなどの既存の汎用プロトコルを使用したスイッチネットワークは柔軟な通信を可能にするが，プロトコルに従ったフレーム生成などの複雑な操作が必要になる
- そこで，新しい複数FPGA Systolic array platformを提案
    - 仮想回路スイッチングネットワーク（VCSN）を使用して，形状やサイズを簡単にカスタマイズ可能な仮想2次元再構成可能FPGA平面を構築し，大規模Systolic arrayを実装
    - すべてのFPGAを統一的に設計し，複数のFPGAボード上のオフチップメモリを同時に駆動する高帯域幅動作によって，柔軟性と効率性を両立したSystolic arrayを実現
- 提案するSystolic arrayプラットフォームの設計と動作を紹介し，予備的なベンチマーク設計の性能評価を提示
- 貢献をまとめると
    - 大規模Systolic array向けのカスタマイズ可能な2次元複数FPGAプラットフォーム
    - 仮想ネットワークトポロジーで接続された複数FPGA上のスケーラブルで柔軟な2次元Systolic arrayアーキテクチャ
    - 各FPGAボードのオフチップメモリを利用した高スループットの分散メモリアクセス
    - 市販FPGAボードを用いた複数FPGA Systolic array操作の実証
    - FPGA数に応じたスケーラブルな性能を示す性能評価
- Table of Contents 
    - 2. [Related Work](#related-work): FPGAやその他のデバイス上のSystolic arrayに関する既存研究
    - 3. [Systolic Array Architecture](#systolic-array-architecture): 本研究で提案するFPGAベースのSystolic arrayアーキテクチャ
    - 4. [Systolic Array on Multi-FPGA](#systolic-array-on-multi-fpga): 仮想2次元複数FPGA平面上のカスタマイズ可能なSystolic array platform
    - 5. [Evaluation](#evaluation): 提案するSystolic arrayプラットフォームの性能評価
    - 6. [Conclusions](#conclusions)

## Related Work


## Systolic Array Architecture
## Systolic Array on Multi-FPGA
## Evaluation
## Conclusions

## 

## Reference
[1] Tomohiro Ueno, Emanuele Del Sozzo, and Kentaro Sano. 2024. Flexible Systolic Array Platform on Virtual 2-D Multi-FPGA Plane. In Proceedings of the International Conference on High Performance Computing in Asia-Pacific Region (HPCAsia '24). Association for Computing Machinery, New York, NY, USA, 84–94. https://doi.org/10.1145/3635035.3637285