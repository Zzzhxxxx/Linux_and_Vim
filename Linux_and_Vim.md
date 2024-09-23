# 1 Linux
## 1.0 `man`*
- `man`：**最重要的命令！Read The Friendly Manual！**
- `man <command>`：当您的英文足够好时，您只需要用这一个命令就可以学会整个 Linux 操作系统。其功能是打开 `<command>` 这项命令的操作指南。
- 举例：`man man`/`man ls`/`man cp`。
- 注：[1.0 man](#10-man)~[1.6 其他命令](#16-其他命令) 都表示在终端输入的命令，输入命令之后还需要输入 `<Enter>`。

## 1.3 输入输出控制*

| 命令 | 全称 | 说明 |
| :--: | :--: | :--: |
| `tee` | T（形似分岔路口） | 将数据流向2个方向 |
| `xargs` | e**x**tended**arg**ument**s** | 从输入接收数据，并把这些数据作为参数提供给另一个命令 |

- 重定向：在Linux中，重定向Redirection是一种常见的功能，它允许你改变命令的标准输入输出流。在命令行中，重定向可以将命令的输出从默认的Terminal转移到一个文件，或者将文件的内容作为命令的输入，甚至是将一个命令的输出直接作为另一个命令的输入。在Linux中有如下几种基础重定向操作（还有很多高级重定向操作，读者可以自行探索）：

| 符号 | 说明 | 举例 |
| :--: | :--: | :--: |
| `>` | 标准输出重定向：将命令的输出覆盖写入到文件中 | `cat <file1> <file2> > <file3>` |
| `>>` | 追加重定向：将命令的输出追加写入到文件中 | `echo "hello again" >> hello.txt` |
| `<` | 标准输入重定向：将文件的内容作为命令的输入 | `sort < <unsorted_file> > <sorted_file>` |
| `2>` | 标准错误重定向：将命令的错误输出写入到文件中 | `ls <not_exist_file> 2> error.log` |

- 由于使用尖括号的缘故，上表可能看起来有些不易理解，请读者多点耐心辨识重定向符号。
- 管道`|`：将管道前命令的输出直接作为管道后命令的输入，可以理解为特殊的重定向。
- 您可以在Terminal中分别输入：

```Terminal
seq 1 10
seq 1 10 | shuf
seq 1 10 | shuf | sort -n
```

- 比较一下输出的结果有何不同，据此结果思考这些命令中蕴含了什么样的重定向过程（注：`shuf`的功能是乱序，`sort -n`的功能是按数字大小排序）。

## 1.9 配置Shell
- 在开始本章节前，需要先确定您的shell类型。常见的shell类型有bash/csh/zsh等等。
- 使用`echo $SHELL`命令查看您的shell类型。
- 如果返回的是`/bin/bash`，说明您的shell类型是bash（下面的操作暂时以bash为例）。
- 进入`home`目录，然后打开`.bashrc`文件：

```shell
cd ~
givm .bashrc
```

- 注：如果您的 Shell 不是 bash，可以在 home 目录下使用 `ls -a` 命令查看名为 `.<shell_type>rc` 的文件并打开它。这里的 `rc` 意为 `run commands`，代表启动 Shell 时自动执行的命令。[2.8 配置Vim](#28-配置Vim) 中的`.vimrc`也是同样的道理。
- 打开`.bashrc`文件后，输入以下命令以配置 Shell。

```.bashrc
function cdls() 
	{
		builtin cd "$1" && ls
	}
alias g='gvim'
alias ..='cd ..'
alias gb='gvim .bashrc'
alias cd="cdls"
alias py='python3'
alias rp='realpath'
alias h='history'
alias cs='cd /mnt/hgfs/Share'
alias ch='cd ~/Global_Workspace'
alias favr='find "$(pwd)" -name "*.v"' # Find All .v Files Recursively 
alias fafr='find "$(pwd)" -name "*.f"' # Find All .f Files Recursively 
```

- 配置完成后`source .bashrc`并重启Terminal，就可以用更简单的命令了。比如我想用Vim打开某个文件，以前需要使用`gvim <file>`命令，现在只需要使用`g <file>`命令即可。
- 注：上面只是抛砖引玉，主要功能是简化一些命令。`.bashrc`的自定义程度很高，读者可以自行配置。

# 2 Vim
- Vim 分为多种操作模式：正常模式 `Normal mode`、插入模式 `Insert mode`、可视模式 `Visual mode` 和命令模式 `Command mode` 等。
- 用 Vim 编辑器打开文件后，默认进入正常模式 [2.1 正常模式](#21-正常模式)。在正常模式进行某些操作以进入其他模式，详情见 [2.2 嵌入模式](#22-嵌入模式) ~ [2.6 替换模式](#26-替换模式)。
- 在其他模式下 `<ESC>` 回到正常模式。

## 2.0 进入 Vim
- 通过 `vi/vim/gvim <file>` 命令用 Vim 编辑器打开文件。
  - Vim 是 vi 的升级版本，它不仅兼容 vi 的所有指令，而且还支持新的特性。
  - `gvim` 命令和 `vim` 命令的区别在于：`vim` 占用原来的 Terminal 打开文件，`gvim` 可以不占用原来的 Terminal 直接打开文件。
  - 综上所述，建议使用 `gvim <file>` 打开文件。
- 和 Linux 类似，进入 Vim 的命令也可以选择 `<option>`。

| 命令 | 说明 |
| :--: | :--: |
| `gvim +<line_number> <file>` | 打开文件并直接定位到指定行 |
| `gvim + <file>` | 打开文件并直接定位到最后一行 |
| `gvim -d <file1> <file2>` | 以差异模式打开 Vim 编辑器，用来对比多个文件并高亮它们的差异 |

## 2.1 正常模式
- 本文先介绍的是正常模式。
  - 如果您的 Linux 中没有可以编辑的文件，可以先 `gvim <your_file_name>` 新建一个空白的文件，然后移步到 [2.2 嵌入模式](#22-嵌入模式) 试着输入一些文本后再回到此节。方便起见，我这里也提供了一份文本，您可以直接复制到 Vim 中（此文本来自于 AI 生成）。
  - 如果您的 Linux 中有可以编辑的文件，直接 `gvim <your_file_name>` 打开它，我们要开始了！

```Text
The quick brown fox jumps over the lazy dog.
Every good boy does fine.
Pack my box with five dozen liquor jugs.
Bright vixens jump; dozy fowl quack.
Sphinx of black quartz, judge my vow.
How vexingly quick daft zebras jump!
Blowzy night-frumps vex'd Jack Q.
Glum Schwartzkopf vex'd by NJ IQ.
A large fawn jumped quickly over white zinc boxes.
Viewing quizzical abstracts mixed up hefty jocks.
Exquisite farm wench gives body jolt to prize stinker.
Jack amazed a few girls by dropping the antique onyx vase!
Quick wafting zephyrs vex bold Jim.
Crazy Fredericka bought many very exquisite opal jewels.
Sixty zippers were quickly picked from the woven jute bag.
A quick movement of the enemy will jeopardize six gunboats.
All questions asked by five watched experts amaze the judge.
Jack quietly moved up front and seized the big ball of wax.
The five boxing wizards jump quickly.
How quickly daft jumping zebras vex.
Chromium, magnesium, and zinc are metallic elements.
The job requires extra pluck and zeal from every young wage earner.
A mad boxer shot a quick, gloved jab to the jaw of his dizzy opponent.
Whenever the black fox jumped the squirrel gazed suspiciously.
Bright vixens jump; dozy fowl quack.
```

### 2.1.1 移动光标

| 按键 | 说明 |
| :--: | :--: |
| `l`/`<Right>` | 向右移动 1 个字符 |
| `h`/`<Left>` | 向左移动 1 个字符 |
| `j`/`<Down>` | 向下移动 1 行 |
| `k`/`<Up>` | 向上移动 1 行 |
| `w` \| word | 移动到下 1 个单词或标点符号的首端 |
| `W` | 移动到下 1 个单词的首端，忽略标点符号 |
| `e` \| end | 移动到下 1 个单词或标点符号的末端 |
| `E` | 移动到下 1 个单词的末端，忽略标点符号 |
| `b` \| backward | 移动到上 1 个单词或标点符号的首端 |
| `B` | 移动到上 1 个单词的首端，忽略标点符号 |
| `gg` | 移动到文件首端 |
| `G` | 移动到文件末端 |
| `0` | 移动到当前行的行首 |
| `^` | 移动到当前行的第 1 个非空字符 |
| `$` | 移动到当前行的行尾 |

- 以上移动光标的操作可以在按键之前输入数字 `<number>` 来进行 n 次重复操作。例如 `<number> + l` 表示向右移动 n 个字符，`<number> + w` 表示移动到下 n 个单词或标点符号的首端，这样就不需要按 n 次 `l` 或 `w` 了。
- 值得注意的是，`<number> + gg`/`<number> + G` 的功能都是移动到文件的第 n 行，`0`/`^`/`$` 无法通过添加 `<number>` 进行 n 次重复。
- 本节所有的操作我都称之为 `<motion>`，后面的章节会用到。

### 2.1.2 删除文本

| 按键 | 说明 |
| :--: | :--: |
| `x` | 删除当前字符 |
| `d` \| delete | 删除功能，需要配合 `<motion>` 使用 |

- `x` 操作比较简单，`<number> + x` 的功能是删除从光标起的 n 个字符，没有其他的用法了（据我所知）。
- `d` 的功能就比较多了，可以配合 `<motion>` 和 `<number>` 实现多种功能，举例如下：

| 按键 | 说明 |
| :--: | :--: |
| `dd` | 删除当前行的内容 |
| `5dd`/`d5d` | 删除从当前光标所在行开始的 5 行文本 |
| `dW` | 删除当前光标位置到下 1 个单词首端的内容 |
| `d$` | 删除当前光标位置到当前行行尾的内容 |
| `d0` | 删除当前光标位置到当前行行首的内容 |
| `d^` | 删除当前光标位置到当前行第 1 个非空字符的内容 |
| `dG` | 删除当前行到文件末端的内容 |
| `d20G` | 删除当前行到文件第 20 行的内容 |

### 2.1.3 复制文本
- `y` 和 `d` 比较类似，也可以配合 `<motion>` 和 `<number>` 实现多种功能，只不过 `y` 的功能是复制，举例如下：

| 按键 | 说明 |
| :--: | :--: |
| `y` \| yank | 复制功能，需要配合 `<motion>` 使用 |
| `yy` | 复制当前行的内容 |
| `5yy`/`y5y` | 复制从当前光标所在行开始的 5 行文本 |
| `yW` | 复制当前光标位置到下 1 个单词首端的内容 |
| `y$` | 复制当前光标位置到当前行行尾的内容 |
| `y0` | 复制当前光标位置到当前行行首的内容 |
| `y^` | 复制当前光标位置到当前行第 1 个非空字符的内容 |
| `yG` | 复制当前行到文件末端的内容 |
| `y20G` | 复制当前行到文件第 20 行的内容 |

### 2.1.4 粘贴文本
- 删除/复制文本后，可以通过以下操作粘贴。

| 按键 | 说明 |
| :--: | :--: |
| `p` \| paste | 粘贴到当前光标位置的后面 |
| `P` | 粘贴到当前光标位置的前面 |

- 需要注意的是，如果之前是以 `dd`/`5yy` 这种以行为单位的操作实现删除/复制功能之后，`p` 会直接在当前行的下一行粘贴，`P` 会直接在当前行的上一行粘贴，很方便（之后提到的 Line Visual Mode 也会有此特性）。

### 2.1.5 替换文本

| 按键 | 说明 |
| :--: | :--: |
| `r` \| replace | 替换当前光标的内容 |

- 格式为：`<number> r <new_character>`，举例：`10rl` / `3rx`。
- [2.1.2 删除文本](#212-删除文本)~[2.1.5 替换文本](#215-替换文本) 中介绍的操作我都称之为 `<operator>`（当然强大的 Vim 不只有这几种 `<operator>`），不难发现 Vim 中的很多操作都是 `<operator> <number> <motion>` 的自由组合。明白这点后，您已经对 Vim 有一定理解了！

### 2.1.6 撤销操作

| 按键 | 说明 |
| :--: | :--: |
| `u` \| undo | 撤销之前的操作 |
| `U` | 撤销在当前行所做的改动 |
| `<Ctrl> - r` | 撤销之前的撤销命令 |

## 2.2 嵌入模式
- 从正常模式进入嵌入模式有以下方法：

| 按键 | 说明 |
| :--: | :--: |
| `i` \| insert | 在光标前进入嵌入模式 |
| `I` | 在行首进入嵌入模式 |
| `a` \| append | 在光标后进入嵌入模式 |
| `A` | 在行尾进入嵌入模式 |
| `o` \| open | 在当前行的下方打开一行并进入嵌入模式 |
| `O` | 在当前行的上方打开一行并进入嵌入模式 |
| `s` \| substitute | 删除当前光标所在的字符并进入嵌入模式 |
| `S` | 删除当前光标所在行并进入嵌入模式 |
| `c` \| change | 删除 `<motion>` 指定的字符并进入嵌入模式 |

- 区别在于插入文本的位置不同。
- 需要注意的是 `s` 可以配合 `<number>` 使用，如 `3s`；`c` 可以配合 `<number>` 和 `<motion>` 使用，如 `c2e`，具体功能如何读者可以自己探索。
- 进入嵌入模式之后就可以随意输入文本了。

## 2.3 可视模式

| 按键 | 说明 |
| :--: | :--: |
| `v` \| visual | 进入可视模式 |
| `V` | 进入行可视模式 |
| `<Ctrl> - v` | 进入列可视模式 |

- 进入可视模式后，根据 [2.1.1 移动光标](#211-移动光标) 所讲移动光标 `<motion>` 以选中想操作的文本内容，再进行操作 `<operator>`，操作生效后会自动退出可视模式。
- 举例：如果想利用可视模式删掉下图光标所在行的 "How quickly daft" 应该怎么做？

![image-20240510155735301](https://github.com/Zzzhxxxx/Linux-and-Vim/assets/131342513/315cfbda-84ca-40a0-a3aa-df0ea9d71877)

- 答：`v + 3e + d`。
- 行可视模式 `Line Visual Mode` / 列可视模式 `Row Visual Mode` 与普通的可视模式有一定区别，您可以分别进入这 3 种模式后 `<motion>` 以探寻其中的区别。
- 在正常模式下拖动鼠标选中文本也可以进入可视模式，但不建议这种做法。

## 2.4 命令模式
- 命令模式以 `:` 开始，以 `<Enter>` 结束。
- 为了和其他模式区分并让命令看起来简洁，我会在下表中列出 `:` 而不列出 `<Enter>`，您在输入时请不要忘记 `<Enter>`。

| 按键 | 说明 |
| :--: | :--: |
| `:w` \| write | 保存 |
| `:q` \| quit | 在没有改动的情况下退出 |
| `:q!` | 在有改动的情况下不保存改动并退出 |
| `:wq` | 保存并退出 |

## 2.5 查找模式
- 查找模式以 `/` 开始，以 `<Enter>` 结束。
- 为了和其他模式区分并让命令看起来简洁，我会在下表中列出 `:` 而不列出 `<Enter>`，您在输入时请不要忘记 `<Enter>`。

| 按键 | 说明 |
| :--: | :--: |
| `/ <char>` | 正向查找文本 |
| `? <char>` | 反向查找文本 |

- 关于查找文本值得注意的是：输入上述命令后，按 `<Enter>` 开始查找。按 `n` 查找下一个 `<char>`，按 `N` 查找上一个 `<char>`。
- 不知道您有没有发现：`<shift> - /` 等价于 `?`，`<shift> - n` 等价于 `N`。

## 2.6 替换
### 2.6.1 替换模式

| 按键 | 说明 |
| :--: | :--: |
| `R` | 进入替换模式 |

- 替换模式比较简单但使用频率不高，类似于 Word 中的改写模式，读者可以自行探索。

### 2.6.2 全局替换

```vim
:s/old/new       //在一行内替换第一个字符串 old 为新的字符串 new
:s/old/new/g     //在一行内替换所有的字符串 old 为新的字符串 new
:#,#s/old/new/g  //在两行内替换所有的字符串 old 为新的字符串 new
:%s/old/new/g    //在文件内替换所有的字符串 old 为新的字符串 new
:%s/old/new/gc   //进行全文替换时询问用户确认每个替换需添加 c 标志
```

| 命令 | 说明 |
| :--: | :--: |
| `:%s/<old_char>/<new_char>/gc` | 将整个文件的 `<old_char>` 更改为 `<new_char>` |
| `:` | 进入命令行模式 |
| `%` | 指定要操作的行数，`%` 表示从第一行到最后一行 |
| `s` | 指定操作，在这种情况下是替换（查找与替代） |
| `/<old_char>/<new_char>` | 原来的字符与替换后的字符 |
| `g` | 全局，对文本行中所有匹配的字符串执行查找和替换操作。若省略 g，则只替换每个文本行中第一个匹配的字符串。 |
| `c` | 需要用户确认的替换命令，省略 `c` 则直接替换 |

- 需要确认时会提示：`replace with Line (y/n/a/q/l/^E/^Y)?`，用户进行进一步处理。

| 按键 | 说明 |
| :--: | :--: |
| `y` | 执行替换操作 |
| `n` | 跳过这个匹配的实例 |
| `a` | 对这个及随后所有匹配的字符串执行替换操作 |
| `q`/`<ESC>` | 退出替换操作 |
| `l` | 执行这次替换并退出 |
| `<Ctrl> - e`/`<Ctrl> - y` | 向下滚动和向上滚动，用于查看建议替换的上下文 |

## 2.7 其他操作
### 2.7.1 交互功能
- 执行一个外部命令：`:!<command>`，例如，`:!ls`
- 将当前Vim中正在编辑的文件保存到文件中：`:w <filename>`
- 将当前编辑文件中可视模式下选中的内容保存到文件中：`v <motion> :w <filename>`
- 提取磁盘文件并将其插入到当前文件的光标位置后面：`r <filename>`
- 读取命令的输出并将其放置到当前文件的光标位置后面：`:r! <command>`
- 显示目前光标所在位置和文件状态信息：`Ctrl + G`

### 2.7.2 其他

| 按键 | 说明 |
| :--: | :--: |
| `<Ctrl> - f`/`<PgDown>` | 向下翻一页 |
| `<Ctrl> - b`/`<PgUp>` | 向上翻一页 |
| `%` | 如果光标当前位置是括号`( ) [ ] { }`，将光标移动到配对的括号上 |

## 2.8 配置Vim
在`.vimrc`文件中输入以下命令以配置Vim（`.vimrc`的路径也在home目录下，详情见 [1.9 配置Shell](#19-配置Shell)）：

```.vimrc
syntax on
set number
set cursorline
set cursorcolumn
set ruler
set tabstop=4
set autoindent
set guicursor+=a:blinkon0
set lines=50
set columns=150
setlocal noswapfile
```

- 这里也只是提供一些参考，读者可以自行寻找适合自己的配置文件。
