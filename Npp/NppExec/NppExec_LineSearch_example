### 行全体の検索・抽出パターン

#### 基本パターン
```
// 指定の文字を含む行全体を検索
sci_find $(NPE_SF_REGEXP) ".*keyword.*"

// 大文字小文字を区別して行全体を検索
sci_find $(NPE_SF_REGEXP | NPE_SF_MATCHCASE) ".*Keyword.*"

// 行頭から検索（先頭の空白を含む）
sci_find $(NPE_SF_REGEXP) "^.*keyword.*$"
```

#### 具体的な使用例
```
// "error"という文字を含む行全体を検索して選択
sci_find $(NPE_SF_REGEXP | NPE_SF_SETSEL) ".*error.*"

// "WARNING:"を含む行を"CAUTION:"に置換
sci_replace $(NPE_SF_REGEXP | NPE_SF_REPLACEALL) "^.*WARNING:.*$" "CAUTION: This line was marked as warning"

// 数字を含む行全体を検索
sci_find $(NPE_SF_REGEXP | NPE_SF_SETSEL) ".*[0-9]+.*"

// コメント行全体を検索（//で始まる行）
sci_find $(NPE_SF_REGEXP | NPE_SF_SETSEL) "^[ \t]*//.*$"
```

#### 応用パターン
```
// 特定の単語を含む行を抽出してファイルに保存
npp_console local -
set local flags = $(NPE_SF_REGEXP | NPE_SF_SETSEL)
sci_find $(flags) ".*TODO:.*"
sel_saveto "$(CURRENT_DIRECTORY)\todos.txt"

// 複数のキーワードのいずれかを含む行を検索
sci_find $(NPE_SF_REGEXP | NPE_SF_SETSEL) ".*(error|warning|fatal).*"

// 空行を除いて特定のパターンを含む行を検索
sci_find $(NPE_SF_REGEXP) "^.+.*keyword.*$"

// 行頭のスペースやタブを含めて検索
sci_find $(NPE_SF_REGEXP) "^[ \t]*.*keyword.*$"
```

#### ログファイル処理の例
```
// タイムスタンプ付きのログから特定のイベントを含む行を抽出
sci_find $(NPE_SF_REGEXP | NPE_SF_SETSEL) "^.*\d{4}-\d{2}-\d{2}.*ERROR.*$"

// ログレベルごとの行を異なる色で置換
set local flags = $(NPE_SF_REGEXP | NPE_SF_REPLACEALL)
sci_replace $(flags) "^.*INFO:.*$" "\033[32m&\033[0m"  // 緑色
sci_replace $(flags) "^.*WARNING:.*$" "\033[33m&\033[0m"  // 黄色
sci_replace $(flags) "^.*ERROR:.*$" "\033[31m&\033[0m"  // 赤色
```

#### 複数行の処理
```
// 複数行にまたがるパターンを検索
// 例：開始タグと終了タグで囲まれた範囲を検索
sci_find $(NPE_SF_REGEXP) "<start>.*[\r\n]*.*</end>"

// 連続する空行を検索
sci_find $(NPE_SF_REGEXP) "\r\n\r\n+"
```

### 注意事項
- `.*`は改行を除く任意の文字にマッチします
- 行末の改行コードまで含めて検索する場合は`$`を使用します
- 大量のテキストで`.*`を使用すると処理が遅くなる可能性があります
- 正規表現の`.`は改行にマッチしないため、複数行を跨ぐ検索には注意が必要です
- パフォーマンスを考慮する場合は、可能な限り具体的なパターンを使用することを推奨します
