# HDL設計・検証エンジニアのための実用的なLinuxコマンド集

## ファイル検索・解析系

### 1. ag (The Silver Searcher)
grepの高速・高機能版。コードベース内の検索に特に有効。
```bash
# ファイル内容の高速検索（バイナリファイルやバックアップを自動除外）
ag "pattern" 

# 特定の拡張子のみを検索
ag "pattern" -G "\.v$"

# 大文字小文字を区別せず検索
ag -i "pattern"
```

### 2. ripgrep (rg)
agよりさらに高速な検索ツール。
```bash
# 再帰的に検索（.gitignoreを自動的に考慮）
rg "pattern"

# ファイル名のみ表示
rg -l "pattern"

# 特定のファイルタイプのみ検索
rg -t verilog "pattern"
```

### 3. fd
findの高速・簡単版。
```bash
# 特定パターンのファイルを検索
fd "\.v$"

# 特定のディレクトリタイプのみ検索
fd -t d "test"

# 更新時間でフィルタ
fd -t f --changed-within 24h
```

## テキスト処理系

### 4. sed（一括置換用）
```bash
# バックアップを作成して一括置換
find . -name "*.v" -exec sed -i.bak 's/old_pattern/new_pattern/g' {} \;

# 特定行の前後に行を追加
sed -i '/pattern/i\新しい行を前に追加' file.v
sed -i '/pattern/a\新しい行を後に追加' file.v
```

### 5. xargs（複数ファイルの一括処理）
```bash
# 検索結果に対して一括処理
find . -name "*.v" | xargs grep "pattern"

# 並列処理での高速化
find . -name "*.log" | xargs -P 4 grep "error"
```

### 6. awk（高度なテキスト処理）
```bash
# 特定フィールドの抽出
awk '/error/ {print $1, $NF}' log.txt

# 条件付き処理
awk '$3 > 100 {print $0}' timing.rpt
```

## ファイル操作系

### 7. rsync（高度なファイルコピー）
```bash
# 差分のみ同期
rsync -av source/ destination/

# バックアップを取りながら同期
rsync -av --backup --backup-dir=/backup source/ destination/
```

### 8. tree（ディレクトリ構造の可視化）
```bash
# ディレクトリ構造を表示
tree -L 2  # 深さ2まで表示

# 特定のパターンのファイルのみ表示
tree -P "*.v"
```

## プロセス管理系

### 9. htop（高機能なtop）
```bash
# プロセスの詳細な監視
htop

# 特定ユーザーのプロセスのみ表示
htop -u username
```

### 10. watch（コマンドの定期実行）
```bash
# 2秒ごとにログファイルの末尾を監視
watch -n 2 "tail -n 20 simulation.log"

# 差分を強調表示
watch -d "ls -l"
```

## シミュレーション・ログ解析用

### 11. tail -f と grep の組み合わせ
```bash
# リアルタイムログ監視（エラーのみ）
tail -f simulation.log | grep --color=auto "ERROR"

# 複数のパターンを同時監視
tail -f simulation.log | grep -E "ERROR|WARN|FAIL"
```

### 12. tee（出力の分岐）
```bash
# 標準出力を画面とファイルの両方に出力
command | tee output.log

# 既存ファイルに追記
command | tee -a output.log
```

## 使用上の Tips

1. **エイリアスの活用**
```bash
# よく使う長いコマンドをエイリアス化
alias findv='find . -name "*.v" -o -name "*.sv"'
alias grepv='grep --color=auto -n -r --include="*.v" --include="*.sv"'
```

2. **関数の定義**
```bash
# バックアップ付き一括置換の関数
replace_all() {
    if [ "$#" -ne 2 ]; then
        echo "Usage: replace_all 'old_pattern' 'new_pattern'"
        return 1
    fi
    find . -type f -name "*.v" -exec bash -c 'cp "$1" "$1.bak" && sed -i "s/$2/$3/g" "$1"' -- {} "$1" "$2" \;
}
```

注意点：
- 新しいコマンドを使用する前に、必ずテスト環境で動作確認をしてください
- バックアップを作成してから一括置換を行うようにしましょう
- 大規模なファイル操作を行う前に、対象を確認するためのドライランを行うことをお勧めします
