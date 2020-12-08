<img src="/Users/xuzheng/Projects/notes/Others/Untitled.assets/lifecycle.png" alt="lifecycle" style="width:600px;" />



查看文件出于什么状态

```bash
git status
```

你工作目录下的每一个文件都不外乎这两种状态：**已跟踪**或**未跟踪**。

**已跟踪的文件**是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能处于未修改，已修改或已放入暂存区。

```bash
echo 'My Project' > README
git status
On branch master
Your branch is up to date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	README

nothing added to commit but untracked files present (use "git add" to track)
```

开始track一个文件	

```bash
git add README
```

?? 未跟踪

A 新添加到暂存去的文件

M 修改过的文件

```bash
git status -s
```

A  README
M  docs/contributing.md

MM 右边的M表示文件被修改了但还是没放到暂存去（staged）， 左边的M表示该文件被修改了并放入了暂存去。当两个都出现时表示文件在工作区被修改并提交到暂存区后又在工作中被修改了。所以在暂存区和工作区都有该文件被修改了的记录。



.gitignore 的格式规范如下：

* 所有空行或者以 `＃` 开头的行都会被 Git 忽略。
* 可以使用标准的 glob 模式匹配。
* 匹配模式可以以（`/`）开头防止递归。
* 匹配模式可以以（`/`）结尾指定目录。
* 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（`!`）取反。

glob模式：

指 shell 所使用的简化了的正则表达式。

* 星号（`*`）匹配零个或多个任意字符
* `[abc]` 匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）
* 问号（`?`）只匹配一个任意字符
* 如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如 `[0-9]` 表示匹配所有 0 到 9 的数字）
* 使用两个星号（`*`) 表示匹配任意中间目录，比如`a/**/z` 可以匹配 `a/z`, `a/b/z` 或 `a/b/c/z`等。

```bash
# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
```



比较的是工作目录中当前文件和暂存区域快照之间的差异，也就是修改之后还没有暂存起来的变化内容。

```bash
git diff
```

若要查看已暂存的将要添加到下次提交里的内容，可以用 `git diff --cached` 命令

若要查看已暂存的将要添加到下次提交里的内容

```bash
git diff --cached
git diff --staged
```

请注意，git diff 本身只显示尚未暂存的改动，而不是自上次提交以来所做的所有改动。 所以有时候你一下子暂存了所有更新过的文件后，运行 `git diff` 后却什么也没有，就是这个原因。



跳过使用暂存区域

```bash
git commit -a -m "message"
```





移除文件

```
rm projects.md
```

用`git rm`记录此次移除文件的操作

```b
$ git rm projects.md
rm 'docs/projects.md'
▶ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	deleted:    projects.md
```

下一次提交时，该文件就不再纳入版本管理了。 如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 `-f`（译注：即 force 的首字母）。 这是一种安全特性，用于防止误删还没有添加到快照的数据，这样的数据不能被 Git 恢复。



把文件从暂存区（staged）中删除。

```bash
git rm --cached README
```



删除log/目录下拓展名为`.log`的所有文件

```bash
$ git rm log/\*.log
```

运行 `git mv` 就相当于运行了下面三条命令：

```bash
$ mv README.md README
$ git rm README.md
$ git add README
```





查看提交历史

```bash
$ git log
```

显示每次提交内容的差异	

```bash
git log -p -2 # -2 表示仅显示最近两次提交
```



将提交放在一行显示

```bash
git log --pretty=oneline
```



```bash
git log --pretty=format:"%h - %an, %ar : %s"
```



| 选项  | 说明                                        |
| :---- | :------------------------------------------ |
| `%H`  | 提交对象（commit）的完整哈希字串            |
| `%h`  | 提交对象的简短哈希字串                      |
| `%T`  | 树对象（tree）的完整哈希字串                |
| `%t`  | 树对象的简短哈希字串                        |
| `%P`  | 父对象（parent）的完整哈希字串              |
| `%p`  | 父对象的简短哈希字串                        |
| `%an` | 作者（author）的名字                        |
| `%ae` | 作者的电子邮件地址                          |
| `%ad` | 作者修订日期（可以用 --date= 选项定制格式） |
| `%ar` | 作者修订日期，按多久以前的方式显示          |
| `%cn` | 提交者（committer）的名字                   |
| `%ce` | 提交者的电子邮件地址                        |
| `%cd` | 提交日期                                    |
| `%cr` | 提交日期，按多久以前的方式显示              |
| `%s`  | 提交说明                                    |



```bash

```

