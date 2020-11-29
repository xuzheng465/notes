安装tmux

```bash
brew install tmux
```



<img src="./tmux.assets/image-20201129085932587.png" alt="image-20201129085932587" style="zoom:33%;" />



tmux 默认的快捷键前缀是ctrl + b

some import concepts in tmux:

1. 会话 (session), 建立一个tmux工作区会话
2. 窗口(window): 容纳多个窗格
3. 窗格(pane): 可以在窗口中分成多个窗格(pane)



创建新session

```bash
tmux new -s
```

竖分屏

```
ctrl b + %
```

横分屏

``` 
ctrl + b  "
```



新建新的Window

```
ctrl + b   c
```



可以在一个window进行代码编辑, 在另一个窗口进行提交