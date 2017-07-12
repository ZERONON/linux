# grep命令的-P选项

man grep的时候有一个-P，文档上的英文：

```
-P, --perl-regexp
              Interpret PATTERN as a Perl regular expression.  
              This is highly experimental and grep -P may warn of  nimplemented features.
```

这意思是，-P 可以让grep使用perl的正则表达式语法，因为perl的正则更加多元化，能实现更加复杂的场景。

最典型的用法是，匹配指定字符串之间的字符。 比如，我们想在一句话（Hello，my name is aming.\)中匹配中间的一段字符串（my name is\) 可以这样写正则表达式。 

echo "Hello, my name is aming."\|grep -P '\(?&lt;=Hello, \).\*\(?= aming.\)'

如果只需要匹配到的部分，还可以加上-o选项

echo "Hello, my name is aming."\|grep -Po '\(?&lt;=Hello, \).\*\(?= aming.\)'

根据这个思路，我们可以去匹配IP地址

ifconfig eth0 \|grep -Po '\(?&lt;=addr:\).\*\(?= Bcast\)'



