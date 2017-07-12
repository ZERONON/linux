问题场景为：

两个脚本，A脚本和B脚本，想要在A脚本中调取B脚本，那调取的方法有三种方式：source、exec和fork。

先分别把脚本内容发出来：

```bash
cat A_source.sh
#!/bin/bash
##test source  exec fork
echo "source B.sh begin"
export a=1
source ./B.sh
echo "A.sh b is $b"
echo "source B.sh end"

cat A_exec.sh
#!/bin/bash
##test source  exec fork
echo "exec B.sh begin"
export a=1
exec ./B.sh
echo "A.sh b is $b"
echo "exec B.sh end"

cat A_fork.sh
#!/bin/bash
##test source  exec fork
echo "fork B.sh begin"
export a=1
./B.sh
echo "A.sh b is $b"
echo "fork B.sh end"

cat  B.sh
#!/bin/bash
b=2
echo "a is $a"
echo "b is $b"
```

在执行脚本之前，需要把A.sh和B.sh放到一个目录下，并且B.sh给x权限。

执行结果如下：

```bash
sh source_A.sh
source B.sh begin
a is 1
b is 2
B.sh b is 2
source B.sh end

sh exec_A.sh
exec B.sh begin
a is 1
b is 2

sh fork_A.sh 
fork B.sh begin
a is 1
b is 2
1.sh b is 
fork B.sh end
```

在这里，阿铭要再给大家说两个知识点就是：

> 1. source  B.sh 和  .  B.sh 是一样的效果。 你没错，这里B.sh前面是一个点，这个点的作用和source是一样的。
>
> 2. fork B.sh 和./B.sh和sh ./B.sh 是一样的效果。

通过上面的实验，总结如下结论：

* source、fork和exec都是在一个脚本中调用第二个脚本\(B.sh\)
* source、fork执行完第二个脚本\(B.sh\)后，会继续往下执行父脚本\(A.sh\)，而exec执行完第二个脚本\(B.sh\)不再继续执行父脚本\(A.sh\)
* source的时候，会把第二个脚本\(B.sh\)里面的变量带出来给父脚本\(A.sh\)，fork并不会把第二个脚本中\(B.sh\)的变量带出来给父脚本\(A.sh\)



