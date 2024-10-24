



# findで検索したファイルの中身をgrepで検索
find . -name "*.log" -exec grep "error" {} \;

#特定のディレクトリ内の全ファイルから文字列を検索
find . -type f -exec grep "検索文字列" {} \;