# EDA環境のためのPerl基礎ガイド

## 1. 基本的な文法とデータ型

### 変数宣言と使用
```perl
# スカラー変数（数値、文字列）
my $count = 0;          # 数値
my $name = "module";    # 文字列

# 配列
my @ports = ("clk", "rst_n", "data");
print $ports[0];        # 配列の要素にアクセス

# ハッシュ（連想配列）
my %port_dir = (
    "clk"   => "input",
    "data"  => "output"
);
print $port_dir{"clk"}; # ハッシュの要素にアクセス
```

### スコープと厳密な記述
```perl
use strict;    # 変数の明示的な宣言を強制
use warnings;  # 警告を有効化

# ローカルスコープ
my $var = "local";      # この変数はブロック内でのみ有効
our $var = "global";    # パッケージ全体で有効
```

## 2. ファイル操作

### ファイルの読み込み
```perl
# ファイルを開く
open(my $fh, '<', 'input.v') or die "Cannot open file: $!";

# 1行ずつ読み込み
while (my $line = <$fh>) {
    chomp $line;    # 改行を削除
    # 処理
}

# ファイルを閉じる
close($fh);

# ファイル全体を一度に読み込む
my @lines = <$fh>;
```

### ファイルの書き込み
```perl
# 書き込みモードでファイルを開く
open(my $out_fh, '>', 'output.md') or die "Cannot create file: $!";

# 書き込み
print $out_fh "# Title\n";

# ファイルを閉じる
close($out_fh);
```

## 3. 正規表現

### 基本的なパターンマッチング
```perl
my $line = "input wire [7:0] data";

# パターンマッチ
if ($line =~ /input/) {
    print "Found input\n";
}

# グループ化と抽出
if ($line =~ /(\w+)\s+wire\s+\[(\d+):(\d+)\]\s+(\w+)/) {
    my $direction = $1;
    my $msb = $2;
    my $lsb = $3;
    my $name = $4;
}
```

### よく使う正規表現パターン
```perl
\w      # 単語文字 [a-zA-Z0-9_]
\s      # 空白文字
\d      # 数字
+       # 1回以上の繰り返し
*       # 0回以上の繰り返し
?       # 0回または1回
()      # グループ化と抽出
\[      # 角括弧（エスケープ必要）
```

### 置換
```perl
# 文字列置換
$line =~ s/wire//;          # wireを削除
$line =~ s/reg/wire/;       # regをwireに置換
$line =~ s/\s+/ /g;         # 連続する空白を1つに
```

## 4. データ構造の操作

### 配列操作
```perl
# 要素の追加
push @array, "new_item";    # 末尾に追加
unshift @array, "first";    # 先頭に追加

# 要素の削除
my $last = pop @array;      # 末尾から削除
my $first = shift @array;   # 先頭から削除

# 配列の走査
foreach my $item (@array) {
    print "$item\n";
}

# 配列のスライス
my @subset = @array[1..3];  # インデックス1から3の要素
```

### ハッシュ操作
```perl
# 要素の追加/更新
$hash{"key"} = "value";

# 要素の存在確認
if (exists $hash{"key"}) {
    print "Key exists\n";
}

# ハッシュの走査
foreach my $key (keys %hash) {
    print "$key => $hash{$key}\n";
}
```

## 5. デバッグテクニック

### データ構造の中身確認
```perl
use Data::Dumper;          # デバッグ用モジュール

# データ構造の内容を表示
print Dumper(\@array);     # 配列の内容
print Dumper(\%hash);      # ハッシュの内容
```

### エラー処理
```perl
# die: エラーで終了
die "Error: $!" unless condition;

# warn: 警告を出して続行
warn "Warning: something wrong" if condition;

# eval: エラーをキャッチ
eval {
    # エラーの可能性がある処理
};
if ($@) {
    print "Error caught: $@\n";
}
```

## 6. よく使うワンライナーパターン

### ファイル処理
```perl
# ファイル内の特定パターンを含む行を表示
perl -ne 'print if /pattern/' file.txt

# 行の置換
perl -pe 's/old/new/g' file.txt

# 特定パターンにマッチする行の数を数える
perl -ne '$count++ if /pattern/; END{print "$count\n"}' file.txt
```

## 7. EDA環境での一般的なパターン

### モジュール定義の抽出
```perl
# モジュール名とポートリストの抽出
if ($line =~ /module\s+(\w+)\s*\((.*?)\)/) {
    my $module_name = $1;
    my $port_list = $2;
}

# ポート定義の抽出
if ($line =~ /^\s*(input|output|inout)\s*(reg|wire)?\s*(\[\d+:\d+\])?\s*(\w+)/) {
    my $direction = $1;
    my $type = $2 || "";
    my $width = $3 || "";
    my $name = $4;
}
```

### コメント処理
```perl
# 行末コメントの削除
$line =~ s/\/\/.*$//;

# ブロックコメントの削除（複数行）
$content =~ s/\/\*.*?\*\///gs;
```

このガイドは基本的な要素をカバーしています。実際のスクリプト開発では、これらを組み合わせて使用することになります。デバッグ時は特に`Data::Dumper`を活用すると、データ構造の中身を確認しやすくなります。</antArtifact>
