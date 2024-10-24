# Cortex-M23 システムアドレス空間仕様 (0xE000_0000 - 0xFFFF_FFFF)

## 概要
このアドレス空間は主にプロセッサのシステム制御、デバッグ、および特権的な機能のために予約されています。

## アドレスマップ詳細

### Private Peripheral Bus (PPB) 領域
**基本アドレス: 0xE000_0000 - 0xE003_FFFF**
- システム制御領域 (SCS)
- NVIC (Nested Vectored Interrupt Controller)
- MPU (Memory Protection Unit)
- SAU (Security Attribution Unit)

### デバッグ領域
**基本アドレス: 0xE004_0000 - 0xE004_FFFF**
- MTB (Micro Trace Buffer) 設定レジスタ
- ETM (Embedded Trace Macrocell) 設定レジスタ
- CTI (Cross Trigger Interface)
- TPIU (Trace Port Interface Unit) 設定レジスタ
- アクセス: AHBポートまたはI/Oポート経由
- 属性: デバイスタイプメモリ、XN（実行不可）

### 予約済みPPB領域
**基本アドレス: 0xE005_0000 - 0xE00F_EFFF**
- PPBインターフェース経由でのみアクセス可能
- XN（実行不可）

### MCU ROM領域
**基本アドレス: 0xE00F_0000 - 0xE00F_FFFF**
- Cortex-M23 MCU ROM実装用
- アクセス: AHBポートまたはI/Oポート経由
- 属性: デバイスタイプメモリ、XN（実行不可）

### ベンダー定義システム領域
**基本アドレス: 0xE010_0000 - 0xFFFF_FFFF**
- CoreSight ROMテーブルおよび他のCoreSightコンポーネント用
- アクセス: AHBポートまたはI/Oポート経由
- 属性: デバイスタイプメモリ、XN（実行不可）

## アクセス制御と属性

### PPB (Private Peripheral Bus)
- 特権アクセスのみ許可
- 非特権アクセスはBusFault例外を生成
- ワードアクセス（32ビット）のみサポート
- ハーフワードまたはバイトアクセスはHardFault例外を生成

### メモリ保護
- MPU実装時、PPB領域を除く全領域の属性をMPUで制御可能
- PPB領域の属性は固定

### セキュリティ属性
セキュリティ拡張実装時：
- 0xEXXX_XXXX領域はSAUやIDAUで変更不可
- 0xFXXX_XXXX領域は常にSecureであり、Non-secure呼び出し不可

## 重要な注意事項

1. アクセス制限：
   - 0xE000_0000-0xEFFF_FFFFの範囲ではワードサイズ（32ビット）アクセスのみサポート
   - その他のサイズのアクセスは未定義の動作

2. MPUによる保護：
   - PPB領域以外の全領域の属性はMPUで変更可能
   - 必要な特権レベルを満たさないアクセスはバスに転送されずHardFault例外を生成

3. デバッグアクセス：
   - デバッグコンポーネントへのアクセスは非侵入的
   - プロセッサのアクセスが常に優先

4. セキュリティ関連：
   - セキュリティ拡張実装時、非セキュアアクセスによるセキュア領域へのアクセスは
     セキュアHardFaultを生成
   - 特定の領域のセキュリティ属性は固定

## システムコンポーネントの配置例

| コンポーネント | アドレス範囲 | 説明 |
|----------------|--------------|------|
| NVIC | 0xE000_E100 - 0xE000_E4EF | 割り込み制御レジスタ |
| System Control | 0xE000_ED00 - 0xE000_ED8F | システム制御レジスタ |
| MPU | 0xE000_ED90 - 0xE000_EDBF | メモリ保護ユニット |
| SAU | 0xE000_EDD0 - 0xE000_EDE8 | セキュリティ属性ユニット |
| Debug | 0xE000_EDF0 - 0xE000_EEFF | デバッグ制御レジスタ |

## Reference

[1] ARM Limited, "ARM® Cortex®-M23 Processor Technical Reference Manual," 
    ARM DDI 0550C (ID112116), Nov. 2016.
    
[2] ARM Limited, "ARM® v8-M Architecture Reference Manual," 
    ARM DDI 0553, 2016.
