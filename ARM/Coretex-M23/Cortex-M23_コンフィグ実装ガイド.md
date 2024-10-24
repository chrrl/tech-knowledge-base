# Cortex-M23 Configuration and Implementation Guide

## 1. Configuration Overview

Cortex-M23プロセッサは、様々なシステム要件に対応するために高い構成の柔軟性を提供しています。構成は以下の3つの階層レベルで実装できます：

- GREBE - プロセッサコンポーネントレベル
- GREBEINTEGRATION - プロセッサ、WIC、Q-Channel、MTBまたはETM、CTIを含むレベル 
- GREBEINTEGRATION_MCU - GREBEINTEGRATIONにDAP、TPIU、MCU_ROMテーブル、APBインターコネクトを含むレベル

## 2. Configuration Process

### 2.1 設定方法

1. `logical/config/` ディレクトリに移動
2. `GREBE.conf` を編集して必要な構成を設定
3. `configure_rtl.pl` を実行して設計を構成し、読み取り専用の `grebe_defs.v` を生成

### 2.2 重要な注意事項

- `configure_rtl.pl` が生成する `grebe_defs.v` は修正してはいけません
- 構成プロセスは自己チェック機能を持ち、不正な構成を防止します
- 設計ファイルは構成に応じて複製・修正されます

## 3. Key Configuration Parameters

### 3.1 System Configuration

```verilog
// Core Features
parameter BE = 0;         // 0:Little-endian, 1:Byte-invariant big-endian
parameter DBG = 1;        // 0:Debug excluded, 1:Debug included
parameter IOP = 1;        // 0:I/O port excluded, 1:I/O port included
parameter VTOR = 1;       // 0:VTOR excluded, 1:VTOR included

// Memory Protection
parameter MPU_NS = 8;     // Non-secure MPU regions (0,4,8,12,16)
parameter MPU_S = 8;      // Secure MPU regions (0,4,8,12,16)
parameter SAU = 4;        // SAU regions (0,4,8)

// Debug Features
parameter BKPT = 4;       // Number of breakpoint comparators (0-4)
parameter WPT = 2;        // Number of watchpoint comparators (0-4)

// Interrupt Configuration
parameter NUMIRQ = 32;    // Number of interrupts (0-240)
parameter IRQDIS = 0;     // IRQ disable mask

// Security Features
parameter SECEXT = 1;     // 0:Security extensions excluded, 1:included
```

### 3.2 Performance Options

```verilog
parameter SMUL = 0;       // 0:Fast single-cycle multiplier
                         // 1:Small 32-cycle multiplier

parameter SDIV = 0;       // 0:Fast 21-cycle divider
                         // 1:Small 37-cycle divider

parameter HWF = 0;        // 0:32-bit instruction fetches when possible
                         // 1:16-bit instruction fetches only
```

## 4. Implementation Guidelines

### 4.1 Power Management Implementation

```verilog
parameter ACG = 1;        // Architectural Clock Gating
parameter RAR = 0;        // Reset All Registers
parameter BUSPROT = 0;    // Interface Protection
parameter FLOPPARITY = 0; // Flip-flop Parity
```

電力管理機能の実装時の注意点：

1. ACGを有効にする場合：
   - `grb_acg.v` モデルをターゲットライブラリのクロックゲーティングセルで置き換える
   - DFTCGENをクロックゲートのバイパス入力に接続

2. RARを有効にする場合：
   - すべてのレジスタにリセット機能が追加され、面積ペナルティが発生
   - RARが0の場合、必要なレジスタのみリセット機能を持つ

### 4.2 Debug Features Implementation

```verilog
parameter SWMD = 0;       // Serial Wire Multi-drop Support
parameter HALTEV = 0;     // Halt Event Signaling Support
parameter BASEADDR = 0xE00FF003; // DAP Base Address
```

デバッグ機能実装時の考慮事項：

1. Serial Wire実装時：
   - SWMDが0の場合、DPIDRは0x0BF11477
   - SWMDが1の場合、DPIDRは0x0BF12477

2. JTAG実装時：
   - DPIDRは0x0BF11477
   - JTAG IDCODEは0x0BA05477

### 4.3 Trace Features Implementation

```verilog
parameter MTB = 0;        // 0:MTB excluded, 1:MTB included
parameter ETM = 0;        // 0:ETM excluded, 1:ETM included
parameter MTBAWIDTH = 12; // MTB SRAM address width (5-32)
```

トレース機能の実装に関する注意：

1. MTBとETMは排他的
2. TPIUはETMが存在する場合のみ実装可能
3. MCU_ROMテーブルはTPIUが存在する場合のみ実装可能

## 5. Configuration Validation

構成の妥当性確認のためのチェックリスト：

1. コンフィグレーションの整合性確認
   - パラメータ間の依存関係の確認
   - 排他的機能の確認

2. 実装テスト
   - 実行テストベンチによる機能確認
   - エッジケースのテスト

3. タイミング検証
   - クリティカルパスの確認
   - セットアップ/ホールドタイムの確認

## 6. Common Issues and Solutions

1. クロックゲーティングの問題
   - ACG=1でクロックゲーティングセルが正しく置換されていることを確認
   - DFTCGENの接続を確認

2. デバッグインターフェースの問題
   - SWMD設定とDPIDR値の整合性を確認
   - デバッグ認証信号の接続を確認

3. メモリプロテクション設定の問題
   - MPU/SAU領域数の設定が要件に合致していることを確認
   - セキュリティ拡張機能との整合性を確認
