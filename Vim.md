# 2 Vim
- Vim分为4种操作模式：正常模式Normal mode、插入模式Insert mode、可视模式Visual mode和命令模式Command mode。
- 用Vim编辑器打开文件后，默认进入正常模式[[#2.1 正常模式]]。在正常模式进行某些操作以进入后3种模式，详情见[2.2 嵌入模式](#22-嵌入模式)/[[#2.3 可视模式]]/[[#2.4 命令模式]]。

## 2.0 进入Vim
- 通过`vi/vim/gvim <file>`命令用Vim编辑器打开文件。
	 - vim是vi的升级版本，它不仅兼容vi的所有指令，而且还支持新的特性。
	- `gvim`命令和`vim`命令的区别在于：`vim`占用原来的Terminal打开文件，`gvim`可以不占用原来的Terminal直接打开文件。
	- 综上所述，建议使用`gvim <file>`打开文件。
- 和Linux类似，进入Vim的命令也可以选择`<option>`。

| 命令 | 说明 |
| :--: | :--: |
| `gvim +<line_number> <file>` | 打开文件并直接定位到指定行 |
| `gvim + <file>` | 打开文件并直接定位到最后一行 |
| `gvim -d <file1> <file2>` | 以差异模式打开Vim编辑器，用来对比多个文件并高亮它们的差异 |

## 2.1 正常模式
- 本文先介绍的是正常模式。
	- 如果您的Linux中没有可以编辑的文件，可以先`gvim <your_file_name>`新建一个空白的文件，然后移步到[2.2 嵌入模式](#22-嵌入模式)试着输入一些文本后再回到此节。方便起见，我这里也提供了一份文本，您可以直接复制到Vim中（此文本来自于AI生成）。
	- 如果您的Linux中有可以编辑的文件，直接`gvim <your_file_name>`打开它，我们要开始了！

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
| `l`/`<Right>` | 向右移动1个字符 |
| `h`/`<Left>` | 向左移动1个字符 |
| `j`/`<Down>` | 向下移动1行 |
| `k`/`<Up>` | 向上移动1行 |
| `w` \| word | 移动到下1个单词或标点符号的首端 |
| `W` | 移动到下1个单词的首端，忽略标点符号 |
| `e` \| end | 移动到下1个单词或标点符号的末端 |
| `E` | 移动到下1个单词的末端，忽略标点符号 |
| `b` \| backward | 移动到上1个单词或标点符号的首端 |
| `B` | 移动到上1个单词的首端，忽略标点符号 |
| `gg` | 移动到文件首端 |
| `G` | 移动到文件末端 |
| `0` | 移动到当前行的行首 |
| `^` | 移动到当前行的第1个非空字符 |
| `$` | 移动到当前行的行尾 |

- 以上移动光标的操作可以在按键之前输入数字`<number>`来进行n次重复操作。例如`<number> + l`表示向右移动n个字符，`<number> + w`表示移动到下n个单词或标点符号的首端，这样就不需要按n次`l`或`w`了。
- 值得注意的是，`<number> + gg`/`<number> + G`的功能都是移动到文件的第n行，`0`/`^`/`$`无法通过添加`<number>`进行n次重复。
- 本节所有的操作我都称之为`<motion>`，后面的章节会用到。

### 2.1.2 删除文本

| 按键 | 说明 |
| :--: | :--: |
| `x` | 删除当前字符 |
| `d` \| ==delete== | 删除功能，需要配合`<motion>`使用 |

- `x`操作比较简单，`<number> + x`的功能是删除从光标起的n个字符，没有其他的用法了（据我所知）。
- `d`的功能就比较多了，可以配合`<motion>`和`<number>`实现多种功能，举例如下：

| 按键 | 说明 |
| :--: | :--: |
| `dd` | 删除当前行的内容 |
| `5dd`/`d5d` | 删除从当前光标所在行开始的5行文本 |
| `dW` | 删除当前光标位置到下1个单词首端的内容 |
| `d$` | 删除当前光标位置到当前行行尾的内容 |
| `d0` | 删除当前光标位置到当前行行首的内容 |
| `d^` | 删除当前光标位置到当前行第1个非空字符的内容 |
| `dG` | 删除当前行到文件末端的内容 |
| `d20G` | 删除当前行到文件第20行的内容 |

### 2.1.3 复制文本
- `y`和`d`比较类似，也可以配合`<motion>`和`<number>`实现多种功能，只不过`y`的功能是复制，举例如下：

| 按键 | 说明 |
| :--: | :--: |
| `y` \| yank | 复制功能，需要配合`<motion>`使用 |
| `yy` | 复制当前行的内容 |
| `5yy`/`y5y` | 复制从当前光标所在行开始的5行文本 |
| `yW` | 复制当前光标位置到下1个单词首端的内容 |
| `y$` | 复制当前光标位置到当前行行尾的内容 |
| `y0` | 复制当前光标位置到当前行行首的内容 |
| `y^` | 复制当前光标位置到当前行第1个非空字符的内容 |
| `yG` | 复制当前行到文件末端的内容 |
| `y20G` | 复制当前行到文件第20行的内容 |

### 2.1.4 粘贴文本
- 删除/复制文本后，可以通过以下操作粘贴。

| 按键 | 说明 |
| :--: | :--: |
| `p` \| paste | 粘贴到当前光标位置的后面 |
| `P` | 粘贴到当前光标位置的前面 |

- 需要注意的是，如果之前是以`dd`/`5yy`这种==以行为单位==的操作实现删除/复制功能之后，`p`会直接在当前行的下一行粘贴，`P`会直接在当前行的上一行粘贴，很方便（之后提到的Line Visual Mode也会有此特性）！
- [[#2.1.2 删除文本]]~[[#2.1.4 粘贴文本]]中介绍的操作我都称之为`<operator>`（当然强大的Vim不只有这几种`<operator>`），不难发现Vim中的很多操作都是`<operator> <number> <motion>`的自由组合。明白这点后，您已经对Vim有一定理解了！

### 2.1.5 撤销操作

| 按键 | 说明 |
| :--: | :--: |
| `u` \| undo | 撤销之前的操作 |
| `U` | 撤销在1行中所做的改动 |
| `<Ctrl> - r` | 撤销之前的撤销命令 |

### 2.1.10 其他

| 按键 | 说明 |
| :--: | :--: |
| `<Ctrl> - f`/`<PgDown>` | 向下翻一页 |
| `<Ctrl> - b`/`<PgUp>` | 向上翻一页 |

- 如果光标当前位置是括号`( ) [ ] { }`，按 `%` 会将光标移动到配对的括号上。

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
| `c` \| change | 删除`<motion>`指定的字符并进入嵌入模式 |

- 区别在于插入文本的位置不同。
- 需要注意的是`s`可以配合`<number>`使用，如`3s`；`c`可以配合`<number>`和`<motion>`使用，如`c2e`，具体功能如何读者可以自己探索。
- 进入嵌入模式之后就可以随意输入文本了。
- 按`<ESC>`退出嵌入模式。

## 2.3 可视模式

| 按键 | 说明 |
| :--: | :--: |
| `v` \| visual | 进入可视模式 |
| `V` | 进入行可视模式 |
| `<Ctrl> - v` | 进入列可视模式 |

- 进入可视模式后，根据[[#2.1.1 移动光标]]所讲移动光标`<motion>`以选中想操作的文本内容，再进行操作`<oprator>`，操作生效后会自动退出可视模式。

- 举例：如果想利用可视模式删掉下图光标所在行的"How quickly daft"应该怎么做？

![[image-20240510155735301.png]]

- 答：`v + 3e + d`。

- 行可视模式Line Visual Mode/列可视模式Row Visual Mode与普通的可视模式有一定区别，您可以分别进入这3种模式之后`<motion>`以探寻其中的区别。
- 在正常模式下拖动鼠标选中文本也可以进入可视模式。
- 如果进入可视模式后不想`<operator>`，按`<Esc>`退出。
