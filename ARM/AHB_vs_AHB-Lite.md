# AHBとAHB-Liteの比較

AHB-Liteは、AHBバスプロトコルを単純化したサブセットとして位置付けられます。以下にその主な違いを示します。

## AHB-Liteの特徴
Cortex-M23プロセッサでは、外部インターフェースとしてAHB-Liteを採用しており、以下の特徴があります：

- HTRANSはIDLEとNONSEQUENTIALのみをサポート
- バーストトランスファーはサポートしていない (HBURSTは常に3'b000)
- アドレスはHSIZEに合わせて整列されている必要がある
- バイト、ハーフワード、ワードのアクセスをサポート

## 主な違いの比較

| 機能 | AHB | AHB-Lite |
|------|-----|-----------|
| バーストトランスファー | サポート | サポートなし |
| トランザクションタイプ | IDLE、BUSY、NONSEQ、SEQ | IDLEとNONSEQのみ |
| アービトレーション | 複数マスターをサポート | シングルマスターのみ |
| 複雑さ | より機能豊富で複雑 | シンプルで実装が容易 |

## まとめ
AHB-Liteは、シングルマスター用にAHBを簡略化したプロトコルです。<br>
必要最小限の機能に絞ることで、実装が容易になり、シンプルな設計が可能となっています。<br>
Cortex-M23のような組み込みプロセッサでの利用に適しています。

## 参考文献
- ARM® AMBA® 5 AHB Protocol Specification, AHB5, AHB-Lite (ARM IHI 0033)
- ARM® Cortex®-M23 Processor Technical Reference Manual (ARM DDI 0550)
- ARM® Cortex®-M23 Processor Integration and Implementation Manual (DIT0062D_0200_en)
