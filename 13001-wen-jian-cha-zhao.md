# 文件查找

## 一、文件查找find

在日常中，find的使用频率很高，熟练掌握对提供工作效率很有帮助。

语法:

```
find(选项)(参数)
```

1、列出当前目录以及目录下的所有文件

```
find .
```

2、找到当前目录下名字为 11.png 的文件

```
find . -name "11.png"
```

3、找到当前目录下所有的  jpg 文件

```
find . -name "*.jpg"
```

4、找到当前目录下的  jpg 文件和 png 文件

```
find . -name "*.jpg" -o -name "*.png"
```

5、找到当前目录下不是以 png 结尾的文件

```
find . ! -nam "*.png"
```

6、根据正则表达式查找

比如：查找当前目录下，文件名都是数字的 png 文件

```
find . -regex "\./*[0-9]+\.png"
```

7、根据路径查找

找到当前目录下路径中包含 swj 的文件/路径

```
find . -path "*swj*"
```

8、根据文件类型查找

查找当前目录下，路径包含 swj 的文件

```
find . -type f -path "*swj*"
```

9、限制搜索深度

查找当前目录下所有的 png，不包括字目录

```
find . -maxdepth 1 -name "*.png"
```

相应的， mindepth 也是选项

```
find . -mindepth 2 -name "*.png"
```

10、根据文件大小

通过 -size 来过滤文件大小，支持的单位如下

* b - 块（512字节）
* c - 字节
* w - 字 （2字节）
* k - 千字节
* M - 兆字节
* G - 吉字节

找出当前目录下文件大小超过 100M 的文件

```
find . -type f -size +100M
```

11、根据访问/修改/变化时间

* 访问时间（-atime/天，-amin/分钟）：用户最近一次访问时间
* 修改时间 （-mtime/天， -mmin/分钟）： 文件最后一次修改时间
* 变化时间 （-ctime/天， -cmin/分钟）：文件数据元（例如权限的过）最后一次修改时间

找出 1天内被修改过的文件

```
find . -type f -mtime -1
```

找出最近1周内被访问过的文件

```
find . -type f -atime -7
```

12、根据权限

通过 -perm 来实现

找出当前目录下权限为777的文件

```
find . -type f -perm 777
```

找出当前目录下权限不是644的php文件

```
find . -type f -name "*.php" ! -perm 644
```

13、根据文件拥有者

找出文件拥有者为 root 的文件

```
find . -type f -user root
```

找出文件所在群组为 root 的文件

```
find . -type f -group root
```



