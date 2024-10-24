# GrepとFindのオプションや使い方まとめ

## Grepコマンドのオプション

| オプション | 説明 | 使用例 |
|------------|------|---------|
| `-i` | 大文字小文字を区別しない | `grep -i "Error" log.txt` |
| `-r, -R` | ディレクトリを再帰的に検索 | `grep -r "TODO" ./src/` |
| `-n` | 一致した行の行番号を表示 | `grep -n "main" program.c` |
| `-v` | 一致しない行を表示 | `grep -v "^#" config.txt` |
| `-l` | 一致したファイル名のみ表示 | `grep -l "error" *.log` |
| `-c` | 一致した行数を表示 | `grep -c "WARNING" error.log` |
| `-w` | 単語単位で検索（完全一致） | `grep -w "log" script.sh` |
| `-A n` | 一致した行の後n行を表示 | `grep -A 3 "exception" log.txt` |
| `-B n` | 一致した行の前n行を表示 | `grep -B 2 "error" log.txt` |
| `-C n` | 一致した行の前後n行を表示 | `grep -C 1 "warning" log.txt` |
| `-e` | 複数のパターンを指定 | `grep -e "error" -e "warning" log.txt` |
| `-f` | パターンをファイルから読み込む | `grep -f patterns.txt data.log` |
| `--color` | マッチした部分を色付けして表示 | `grep --color "ERROR" log.txt` |

## Findコマンドのオプション

| オプション | 説明 | 使用例 |
|------------|------|---------|
| `-name` | ファイル名で検索（ワイルドカード使用可） | `find . -name "*.txt"` |
| `-iname` | ファイル名で検索（大文字小文字区別なし） | `find . -iname "*.PDF"` |
| `-type` | ファイルタイプで検索<br>f（通常ファイル）<br>d（ディレクトリ）<br>l（シンボリックリンク） | `find . -type f`<br>`find . -type d` |
| `-mtime` | 更新日時で検索（日単位）<br>n（n日前）<br>+n（n日より前）<br>-n（n日以内） | `find . -mtime -7`<br>`find . -mtime +30` |
| `-mmin` | 更新日時で検索（分単位） | `find . -mmin -60` |
| `-size` | ファイルサイズで検索<br>c（バイト）<br>k（キロバイト）<br>M（メガバイト）<br>G（ギガバイト） | `find . -size +100M`<br>`find . -size -1G` |
| `-empty` | 空のファイル/ディレクトリを検索 | `find . -type f -empty` |
| `-user` | 所有ユーザーで検索 | `find . -user username` |
| `-group` | 所有グループで検索 | `find . -group groupname` |
| `-perm` | パーミッションで検索 | `find . -perm 644` |
| `-exec` | 検索結果に対してコマンドを実行 | `find . -name "*.tmp" -exec rm {} \;` |
| `-maxdepth` | 検索する深さを制限 | `find . -maxdepth 2 -name "*.log"` |
| `-mindepth` | 検索を開始する深さを指定 | `find . -mindepth 2 -name "*.txt"` |
| `-newer` | 指定したファイルより新しいファイルを検索 | `find . -newer reference.txt` |

注意点：
- 複数のオプションを組み合わせることで、より詳細な検索条件を指定できます
- `-exec`オプションは慎重に使用してください（特に削除操作時）
- 大量のファイルを検索する場合は、適切な検索深さ制限を設定することをおすすめします


# findで検索したファイルの中身をgrepで検索
find . -name "*.log" -exec grep "error" {} \;

#特定のディレクトリ内の全ファイルから文字列を検索
find . -type f -exec grep "検索文字列" {} \;