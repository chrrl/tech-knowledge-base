# NppExec コマンドリファレンス

## 基本コマンド

| コマンド | 説明 | 使用例 |
|----------|------|--------|
| cls | コンソール画面をクリアする | `cls` |
| cd | カレントディレクトリを表示/変更する | `cd` - 表示<br>`cd path` - 変更<br>`cd drive:\path` - ドライブとパスを変更 |
| dir | ファイル一覧を表示する | `dir` - 全ファイル表示<br>`dir *.txt` - .txtファイルのみ表示 |
| echo | テキストをコンソールに出力する | `echo Hello world` |
| exit | 現在のスクリプトを終了する | `exit` |

## 変数操作コマンド

| コマンド | 説明 | 使用例 |
|----------|------|--------|
| set | 通常変数を設定/表示する | `set` - 全変数表示<br>`set var=value` - 変数設定 |
| set local | ローカル変数を設定/表示する | `set local var=value` |
| unset | 変数を削除する | `unset var` |
| env_set | 環境変数を設定する | `env_set PATH=value` |
| env_unset | 環境変数を削除/復元する | `env_unset PATH` |

## Notepad++操作コマンド

| コマンド | 説明 | 使用例 |
|----------|------|--------|
| npp_exec | NppExecスクリプトを実行する | `npp_exec "script_name"` |
| npp_save | 現在のファイルを保存する | `npp_save` |
| npp_open | ファイルを開く | `npp_open file.txt` |
| npp_close | ファイルを閉じる | `npp_close file.txt` |
| npp_switch | 指定ファイルに切り替える | `npp_switch file.txt` |
| npp_run | 外部プロセス/コマンドを実行する | `npp_run calc.exe` |
| npp_console | コンソールウィンドウの表示制御 | `npp_console on/off` |

## テキスト操作コマンド

| コマンド | 説明 | 使用例 |
|----------|------|--------|
| sel_saveto | 選択テキストをファイルに保存 | `sel_saveto file.txt` |
| sel_loadfrom | ファイルから選択位置にテキストを読み込む | `sel_loadfrom file.txt` |
| sel_settext | 選択テキストを置換する | `sel_settext "new text"` |
| text_saveto | 全テキストをファイルに保存 | `text_saveto file.txt` |
| text_loadfrom | ファイルから全テキストを読み込む | `text_loadfrom file.txt` |

## 制御コマンド

| コマンド | 説明 | 使用例 |
|----------|------|--------|
| if | 条件分岐を行う | `if $(var)==1 goto label` |
| goto | 指定ラベルにジャンプする | `goto label` |
| sleep | 指定ミリ秒待機する | `sleep 1000` |

## ダイアログコマンド

| コマンド | 説明 | 使用例 |
|----------|------|--------|
| inputbox | 入力ダイアログを表示する | `inputbox "Enter value:"` |
| messagebox | メッセージボックスを表示する | `messagebox "Hello"` |

## システム変数

| 変数 | 説明 |
|------|------|
| $(FULL_CURRENT_PATH) | 現在のファイルのフルパス |
| $(CURRENT_DIRECTORY) | カレントディレクトリ |
| $(FILE_NAME) | ファイル名 |
| $(NAME_PART) | 拡張子を除いたファイル名 |
| $(EXT_PART) | 拡張子 |
| $(NPP_DIRECTORY) | Notepad++のインストールディレクトリ |
| $(PLUGINS_CONFIG_DIR) | プラグイン設定ディレクトリ |
| $(CURRENT_WORD) | カーソル位置の単語 |
| $(CURRENT_LINE) | 現在の行番号 |
| $(CURRENT_COLUMN) | 現在の列番号 |
| $(SELECTED_TEXT) | 選択されているテキスト |
| $(CLIPBOARD_TEXT) | クリップボードのテキスト |

## 注意事項

- コマンドは大文字小文字を区別しません
- コマンドライン引数にスペースが含まれる場合は "" で囲みます
- // をコメント記号として使用できます
- 変数は $() で囲んで参照します
- スクリプトのネスト実行が可能です
