Tips



## 在Linux中如何查看当前目录文件大小, 并从大到小排列?

```bash
du -h --max-depth=1 | sort -hr
```





## How to open a tar file

```bash
tar -xvf file.tar
```



##  What about tar.gz

```bash
tar -zxvf file.tar.gz
```



## typedef usage in C

```c
typedef struct list {
  	item_type item;
  	struct list *next;
} list;
```



## Change the icon size in macOS



Finder->View->Show View Options



# 学习开源的步骤

个人觉得学习一个东西的步骤： 

1、看介绍 

2、看demo 

3、看入门 

4、自己做demo 

5、manual、reference

 6、应用 

7、看总结 

8、看心得 

9、思考 

10、看源码 

11、调试源码 

12、修改源码 

13、自己做简单实现 

14、不断完善

---

# Disable debug output in Goland

1. Open Help | Find Action...
2. Type Registry and hit Enter.
3. Find go.run.processes.with.pty there and turn it off.

Now IDE will **`fold`** those information and will make it look cleaner.