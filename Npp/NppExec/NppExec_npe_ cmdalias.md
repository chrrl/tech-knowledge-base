[前述の内容に以下を追加]

## コマンドエイリアス操作

| コマンド | 説明 | 使用例 |
|----------|------|--------|
| npe_cmdalias | コマンドエイリアスを表示/設定/削除する | `npe_cmdalias` - 全エイリアスを表示<br>`npe_cmdalias alias` - 特定のエイリアスを表示<br>`npe_cmdalias alias =` - エイリアスを削除<br>`npe_cmdalias alias = command` - エイリアスを設定 |
| npe_cmdalias local | スクリプトローカルのエイリアスを設定する | `npe_cmdalias local alias = command` |

### npe_cmdalias の詳細説明

#### 基本的な使い方
```
// エイリアスの一覧表示
npe_cmdalias

// 特定のエイリアスの内容表示
npe_cmdalias g

// エイリアスの設定
npe_cmdalias g = gcc -o "$(NAME_PART)" "$(FILE_NAME)"

// エイリアスの削除
npe_cmdalias g =
```

#### ローカルエイリアスの使い方
```
// スクリプト内でのみ有効なローカルエイリアスを設定
npe_cmdalias local py = python "$(FILE_NAME)"

// このエイリアスはスクリプトの終了時に自動的に削除されます
```

#### エイリアスの利点
- 長いコマンドを短い別名で呼び出せる
- よく使うコマンドをカスタマイズして再利用できる
- スクリプト内で一時的なコマンド定義が可能

#### 主な使用例
```
// コンパイル用エイリアス
npe_cmdalias gcc = gcc -Wall -o "$(NAME_PART)" "$(FILE_NAME)"

// Python実行用エイリアス
npe_cmdalias py = python -u "$(FILE_NAME)"

// ディレクトリ移動用エイリアス
npe_cmdalias cdproj = cd "$(NPP_DIRECTORY)\projects"

// ビルド＆実行用エイリアス
npe_cmdalias build = npp_save && gcc "$(FILE_NAME)" && "$(NAME_PART).exe"
```

#### 注意事項
- エイリアス名は一意である必要がある
- グローバルエイリアスはNppExec全体で有効
- ローカルエイリアスは定義されたスクリプト内でのみ有効
- エイリアス内で変数展開が可能（$(変数名)の形式）
- エイリアスはネストして定義可能

#### 高度な使用例
```
// 複数のコマンドを組み合わせたエイリアス
npe_cmdalias compile = npp_save && gcc -c "$(FILE_NAME)" && npp_console off

// 条件分岐を含むエイリアス
npe_cmdalias build = if "$(EXT_PART)" == ".cpp" then g++ "$(FILE_NAME)" else gcc "$(FILE_NAME)" endif

// 引数を受け取るエイリアス
npe_cmdalias runcmd = cmd /c "$(1)" && pause

// ディレクトリ操作を含むエイリアス
npe_cmdalias mkcd = cmd /c mkdir "$(1)" && cd "$(1)"
```

#### エイリアスのスコープ管理
1. グローバルスコープ
```
// 全体で使用可能なエイリアス
npe_cmdalias gl_cmd = some_command
```

2. ローカルスコープ
```
// スクリプト内でのみ有効なエイリアス
npe_cmdalias local temp_cmd = some_command
```

これらの機能を使用することで、NppExecのスクリプトをより効率的に記述できます。
