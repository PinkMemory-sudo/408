切割文件

关键命令：`split`,`xargs`

split a.txt wenzhou -2 -a2 -d

ls | grep wenzhou | xargs -n1 -i{} mv {} {}.csv

```bash
lineSum=`wc -l $1 | awk '{print $1}'`
echo "*"{,,,,,,}
echo "The total number of lines in the file is ${lineSum}"
echo "How many lines a file ?"
read lines
aNum=`expr $lineSum / $lines`
echo "What's the prefix"
read prefix
echo "What's the suffix(.csv, .txt, etc.)"
read suffix
a=${#aNum}
split $1 $prefix -l $lines -d -a $a
ls | grep $prefix | xargs -n1 -i{} mv {} {}$suffix
echo "*"{,,,,,,}
```

