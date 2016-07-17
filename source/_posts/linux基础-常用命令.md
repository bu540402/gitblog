title: linux基础-常用命令
date: 2016-05-18 08:54:12
tags: [操作系统,linux]
---
## 帮助类命令
1. info：列出命令的详细信息
2. whatis: 列出命令的简要用途说明
3. man：（待补充）

## 文件操作类命令
1. mkdir : 创建目录
```
 mkdir test
 mkdir -p test/subTest #如果test是不存在的，自动创建test
```
2. rm : 删除文件或目录
```
rm test.txt #删除文件
rm -r test  #删除目录
rm -f test.txt #强制删除，不给提示
```
3. rmdir: 删除空目录
```
rmdir dir #dir必须为空
rmdir -p dir # 递归的删除dir，如果删除dir的子目录之后，dir为空，则删除dir
```
3. mv ：移动文件
 ```
 mv src dir #可以实现文件重命名
 mv -f src dir #若目标文件已经存在，强制覆盖
 mv -i src dir #若目标文件已经存在，提示覆盖
 mv -u src dir #若目标文件已经存在，src较新时覆盖
 mv -t dir src1 src2 #目标文件在前
 ```
4. cp ：复制文件
```
cp srcfile dir #复制文件到目录下，如目录下有同名文件，则提示是否覆盖
cp -a dir1 dir2 #复制目录，若目标目录存在，则源目录复制到目标目录下，也就是说dir1在dir2，若目标目录不存在，则创建目标目录，dir1中的内容在dir2下。
```
5. cd : 目录切换
```
cd dir #进入到目录
cd / #进入根目录
cd .. #退到上级目录
cd ~ #进入到当前用户目录
cd - #返回进入到当前目录之前的目录
```
6. ls : 列出目录
```
ls -a #列出所有文件，包括隐藏文件
ls -t #按文件修改时间排序
ls -S #根据文件大小排序
ls -h #以容易理解的方式列出文件大小
ls -r #反序
ls -l #列出文件详情
ls -X #根据文件扩展名排序
```
6. pwd ：
```
pwd #显示当前完整路径
pwd -P #显示当前实际路径（区别连接路径）
```
7. find : 查找文件或目录（实时）
8. locate : 查看文件或目录（非实时，需要更新索引）
9. cat :显示文件
```
cat file #显示整个文件
cat > file # 创建新文件
cat file1 file2 > file3 #合并文件
cat -n file # 对输出添加行编号
```
10. head:显示文件开头
```
head file #显示文件开头10行
head -n 5 file #显示文件开头5行
head -c 5 file #显示文件开头5个字节
```
11. tail: 显示文件末尾
```
tail file #读取末尾10行
tail -n 5 file #读取末尾5行
tail -f file #循环读取文件末尾，可以用户实时监控log文件
tail -f -pid=PID file #当pid进程终止后，停止
```
12. diff: 比较文件差异
13. chown:
14. chmod:
15. 重定向：
- >  输出重定向到一个文件或设备 覆盖原来的文件
- >! 输出重定向到一个文件或设备 强制覆盖原来的文件
- >> 输出重定向到一个文件或设备 追加原来的文件
- <  输入重定向到一个程序

16. 管线命令
利用Linux所提供的管道符“|”将两个命令隔开，管道符左边命令的输出就会作为管道符右边命令的输入。连续使用管道意味着第一个命令的输出会作为 第二个命令的输入，第二个命令的输出又会作为第三个命令的输入，

## 文本处理类命令
1. touch ：创建新文件或修改文件时间戳
```
touch new.txt #创建一个不存在的文件
touch -t timestamp some.txt #修改时间戳
```
1. grep ：文本搜索
```
grep match_patten file # 默认访问匹配行
grep "class" . -R -n  #在多级目录中对文本递归搜索
grep -e "class" -e "vitural" file #匹配多个模式:
```

2. xargs ：
3. sort :（待更新）
4. uniq:
5. tr :（待更新）
6. cut :（待更新）
7. paste :（待更新）
8. wc :
9. set :（待更新）
10. awk :
## 磁盘管理类命令
1. du :
2. tar :
## 进程管理类命令
1. ps : 列出进程
```
ps -ef #查询正在运行的进程信息
ps -ajx #以完整的格式显示所有的进程
```
2. top :显示进程信息，并实时更新
输入top命令后，进入到交互界面；接着输入字符命令后显示相应的进程状态：
对于进程，平时我们最常想知道的就是哪些进程占用CPU最多，占用内存最多。以下两个命令就可以满足要求:
P：根据CPU使用百分比大小进行排序。
M：根据驻留内存大小进行排序。
i：使top不显示任何闲置或者僵死进程。

3. lsof : 查看文件打开信息
```
lsof -i:3306 #查看端口占用的进程状态
lsof -u username #查看用户username的进程所打开的文件
lsof -c init #查询init进程当前打开的文件
lsof -p 23295 #查询指定的进程ID(23295)打开的文件
$lsof +d mydir1/ #查询指定目录下被进程开启的文件（使用+D 递归目录）

```
4. kill :杀死进程
```
kill PID #杀死pid进程
kill -9 3434 #杀死相关进程
```
5. pmap :来输出进程内存的状况，可以用来分析线程堆栈
## 性能监控类命令
1. sar :
2. free :
3. watch :
## 网络类命令
1. netstat :显示各种网络相关信息
```
netstat -a #列出所有端口
netstat -at #列出所有tcp端口
netstat -l #列出所有监听的服务状态

```
2. route ：查看路由状态
```
route -n
```
4. traceroute :探测前往地址IP的路由路径:
5. host :DNS查询
## ipc资源管理类命令
1. ipcs ：
2. ulimit ：
