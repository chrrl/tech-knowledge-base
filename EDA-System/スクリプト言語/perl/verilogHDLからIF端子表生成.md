# verilogHDLからIF端子表を生成するスクリプト例

## タスクの要件分析：
- 入力：verilogHDLファイル
- 処理：moduleのIF情報抽出
  - module名
  - 入出力ポート（input/output/inout）
  - ポート幅
  - ポート名
- 出力：マークダウン形式のドキュメント

## 実装例(Perl)

```perl
#!/usr/bin/perl

use strict;
use warnings;

# Usage check
die "Usage: $0 <verilog_file>\n" unless @ARGV == 1;
my $file = $ARGV[0];

# Open input file
open(my $fh, '<', $file) or die "Cannot open file: $!";

my $module_name = "";
my @ports = ();
my $in_module = 0;

# Parse Verilog file
while (my $line = <$fh>) {
    chomp $line;
    
    # Remove comments
    $line =~ s/\/\/.*$//;
    
    # Handle module declaration
    if ($line =~ /module\s+(\w+)\s*\(/) {
        $module_name = $1;
        $in_module = 1;
        next;
    }
    
    # Skip if not in module
    next unless $in_module;
    
    # End of module
    if ($line =~ /endmodule/) {
        $in_module = 0;
        next;
    }
    
    # Parse port declarations
    if ($line =~ /^\s*(input|output|inout)\s*(reg|wire)?\s*(\[\d+:\d+\])?\s*(\w+)\s*[,;]/) {
        push @ports, {
            direction => $1,
            width => $3 || "1",
            name => $4
        };
    }
}

close($fh);

# Generate Markdown output
print "# Module: $module_name\n\n";
print "## Interface Description\n\n";
print "| Direction | Width | Port Name |\n";
print "|-----------|-------|-----------|\n";

foreach my $port (@ports) {
    my $width = $port->{width};
    $width = "1 bit" if $width eq "1";
    $width =~ s/\[(\d+):(\d+)\]/$1-$2/;
    print "| $port->{direction} | $width | $port->{name} |\n";
}
```

5. 使用方法：
```bash
$ chmod +x hdl_if_parser.pl
$ ./hdl_if_parser.pl your_module.v > interface_doc.md
```

