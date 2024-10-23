## Scintilla検索・置換コマンド

| コマンド | 説明 | 使用例 |
|----------|------|--------|
| sci_find | テキストの検索を行う | `sci_find flags "search_text"` |
| sci_replace | テキストの置換を行う | `sci_replace flags "search_text" "replace_text"` |

### sci_find の詳細説明

#### 基本構文
```
sci_find <flags> <検索文字列>
```

#### フラグオプション
以下のフラグを組み合わせて使用可能（ビット演算子 | で連結）：

| フラグ | 値 | 説明 |
|--------|-----|------|
| NPE_SF_MATCHCASE | 1 | 大文字小文字を区別する |
| NPE_SF_WHOLEWORD | 2 | 単語単位で検索する |
| NPE_SF_REGEXP | 4 | 正規表現を使用する |
| NPE_SF_BACKWARD | 8 | 後方検索を行う |
| NPE_SF_INWHOLETEXT | 16 | ドキュメント全体を検索する |
| NPE_SF_SETPOS | 32 | 検索開始位置を設定する |
| NPE_SF_SETSEL | 64 | 検索結果を選択する |
| NPE_SF_REPLACEALL | 128 | すべて置換する（sci_replaceのみ） |

#### 使用例
```
// 大文字小文字を区別して検索
sci_find NPE_SF_MATCHCASE "searchText"

// 単語単位で後方検索
sci_find $(NPE_SF_WHOLEWORD | NPE_SF_BACKWARD) "word"

// 正規表現を使用して全体を検索し、結果を選択
sci_find $(NPE_SF_REGEXP | NPE_SF_INWHOLETEXT | NPE_SF_SETSEL) "[0-9]+"

// 複数のフラグを組み合わせた検索
set local flags = $(NPE_SF_MATCHCASE | NPE_SF_WHOLEWORD | NPE_SF_SETSEL)
sci_find $(flags) "findText"
```

### sci_replace の詳細説明

#### 基本構文
```
sci_replace <flags> <検索文字列> <置換文字列>
```

#### 使用例
```
// 単純な置換
sci_replace NPE_SF_MATCHCASE "oldText" "newText"

// 正規表現を使用した置換
sci_replace $(NPE_SF_REGEXP) "([0-9]+)" "NUM$1"

// すべて置換
sci_replace $(NPE_SF_REPLACEALL) "old" "new"

// 単語単位ですべて置換
sci_replace $(NPE_SF_WHOLEWORD | NPE_SF_REPLACEALL) "word" "term"

// 正規表現を使用してすべて置換
set local flags = $(NPE_SF_REGEXP | NPE_SF_REPLACEALL)
sci_replace $(flags) "([A-Z])\w+" "word_$1"
```

### 高度な使用例

#### 検索と置換の組み合わせ
```
// 検索して選択し、その後で置換
sci_find $(NPE_SF_MATCHCASE | NPE_SF_SETSEL) "findText"
sci_replace NPE_SF_MATCHCASE "findText" "replaceText"

// 正規表現での検索と置換
set local flags = $(NPE_SF_REGEXP | NPE_SF_REPLACEALL)
sci_replace $(flags) "\b\d+\b" "NUM"
```

#### エラーハンドリング
```
// 検索結果の確認
sci_find $(NPE_SF_MATCHCASE) "searchText"
if $(MSG_RESULT) == -1 then
    echo "Text not found"
else
    echo "Found at position $(MSG_RESULT)"
endif
```

#### 検索範囲の制御
```
// 選択範囲内での検索
sel_settext "Search in this text only"
sci_find $(NPE_SF_INWHOLETEXT | NPE_SF_SETSEL) "text"
```

### 注意事項
- フラグは複数組み合わせることができます
- 正規表現を使用する場合はBoost正規表現構文に従います
- 置換操作の前にバックアップを取ることを推奨します
- 大規模なテキストでの全置換は時間がかかる可能性があります
- フラグの値は定数として定義されているため、直接数値を使用することも可能です

これらのコマンドを使用することで、プログラムによるテキストの検索・置換操作を効率的に行うことができます。
