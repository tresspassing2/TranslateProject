LFCS 第 12 讲：如何使用已安装的帮助文档和工具探索 Linux
==================================================================================

由于 2016 年 2 月 2 日 LFCS 考试要求变更生效，我们正在向 [LFCS 系列][1] 增加必要的课题并发布在 [这里][1] 。为了准备考试，同时也强烈建议你阅读 [LFCE 系列][2] 的文章。

![](http://www.tecmint.com/wp-content/uploads/2016/03/Explore-Linux-with-Documentation-and-Tools.png)
>LFCS: 使用已安装的帮助文档和工具探索 Linux ——第 12 部分

一旦你习惯于使用命令行工作并且对此感到惬意，你会意识到一份常规的 Linux 安装已经包含了使用和配置系统所需的全部说明文档。

去熟悉命令行的帮助工具的另一个好的理由是，在 [LFCS][3] 和 [LFCE][4] 考试中，它们是你可用的信息资源——因为没有互联网和谷歌，只有你和命令行。

出于这个原因，这篇文章将给你一些关于有效利用已安装的文档和和工具来准备通过 **Linux Foundation Certification** （即 LFC)考试的小贴士。

### Linux 帮助页面（Man Page）

“man page”，是“manual page”的缩写，意思即为其字面理解：已有工具的使用手册。它包含命令支持的选项列表（连带其解释），有些使用说明甚至还包括使用实例。

要打开帮助页面，使用加在你所想要进一步了解的工具名称后面的 **帮助命令** 。例如：

```
# man diff
```

将会打开“diff”（“diff”为“different”的缩写，中文解释：区别。）的帮助页面，该工具用于逐行比较文本文件（想要退出，只要按一下q键。）。

假设我们想要在Linux系统中比较两个名为“file1”和“file2”的文本文件。这些文件包含了两台安装了相同发行版且版本相同的 Linux 电脑上所安装的软件包列表。

对“diff1”和“diff2”这两个文件执行“diff”命令，将会返回告诉我们这些列表中 是否有区别：

```
# diff file1 file2
```

![](http://www.tecmint.com/wp-content/uploads/2016/03/Compare-Two-Text-Files-in-Linux.png)
>在Linux下比较两个文本文件

有“<”符号的地方表示在“file2”中有行缺失；取而代之的是，如果在“file1”中有行缺失，它们将由“>”符号表示。

另一方面， **7d6** 表示文件中的第 **#7** 行应该被删除以与“file2”一致 ( **24d22** 和 **41d38** 是同样的道理), 以及 **65,67d61** 告诉我们，我们需要移除一号文件中的第 **65** 行到第 **67** 行。如果我们做了这些更正，它们将会变成两个完全相同的文件。

另外，根据帮助页面，你可以通过使用选项“-y”来并列显示两个文件。你会发现这样来确定文件中缺失的行更简单：

```
# diff -y file1 file2
```

![](http://www.tecmint.com/wp-content/uploads/2016/03/Compare-and-List-Difference-of-Two-Files.png)
>比较并列出两个文件的不同之处

同样的，你也可以使用“diff”命令比较两个二进制文件。如果他们完全相同，“diff”命令将没有任何输出并会静默退出。否则，它会返回如下消息：“**Binary files X and Y differ**”（二进制文件X和Y有区别）。

###“--help”（帮助）选项

在许多（不是所有的）命令中，“--help”选项被用于显示该命令的一份简短使用指南，尽管它并不提供这个工具的综合描述，它仍然是一个快速获取有关该程序的用法和其可用选项的好方法。

例如：

```
# sed --help
```

显示“sed”(stream editor，一个非交互式上下文编辑器)的各可用选项的用法。

一个经典的例子是使用“sed”来替换文件中的字符。通过使用“-i”选项（原处打开文件），你可以在不打开文件的情况下编辑一个文件。如果你还想要对原文件做一个备份，
在“-i“选项后面加一个后缀来使用原文件的内容创建一个独立的文件。

例如，要把“lorem.txt”文件中每一个出现的“Lorem”单词替换为“Tecmint”（不分大小写）并用原文件的内容创建一个新文件，执行如下操作：

```
# less lorem.txt | grep -i lorem
# sed -i.orig 's/Lorem/Tecmint/gI' lorem.txt
# less lorem.txt | grep -i lorem
# less lorem.txt.orig | grep -i lorem
```

请注意，“lorem.txt”文件中的每一个出现的“Lorem”都被替换成了“Tecmint”，而原文件的内容被保存至“lorem.txt.orig”了。

![](http://www.tecmint.com/wp-content/uploads/2016/03/Replace-A-String-in-File.png)
>在文件中替换字符串

###在 /usr/share/doc 目录下已安装的帮助文件

这或许是我最喜欢的一招。如果你转到“/usr/share/doc”目录下并且列出子目录，你会发现许许多多以你 Linux 系统中已安装的工具命名的目录。

根据 [文件系统层次结构标准][5]，这些目录包含着帮助页面中也许没有的有用信息，同时还有能使配置过程更简便的模板和配置文件。

例如，let鈥檚 consider `squid-3.3.8` (version may vary from distribution to distribution) for the popular HTTP proxy and [squid cache server][6].

Let鈥檚 `cd` into that directory:

```
# cd /usr/share/doc/squid-3.3.8
```

and do a directory listing:

```
# ls
```

![](http://www.tecmint.com/wp-content/uploads/2016/03/List-Files-in-Linux.png)
>Linux Directory Listing with ls Command

You may want to pay special attention to `QUICKSTART` and `squid.conf.documented`. These files contain an extensive documentation about Squid and a heavily commented configuration file, respectively. For other packages, the exact names may differ (as **QuickRef** or **00QUICKSTART**, for example), but the principle is the same.

Other packages, such as the Apache web server, provide configuration file templates inside `/usr/share/doc`, that will be helpful when you have to configure a standalone server or a virtual host, to name a few cases.

### GNU info Documentation

You can think of info documents as man pages on steroids. As such, they not only provide help for a specific tool, but also they do so with hyperlinks (yes, hyperlinks in the command line!) that allow you to navigate from a section to another using the arrow keys and Enter to confirm.

Perhaps the most illustrative example is:

```
# info coreutils
```

Since coreutils contains the [basic file, shell and text manipulation utilities][7] which are expected to exist on every operating system, you can reasonably expect a detailed description for each one of those categories in info **coreutils**.

![](http://www.tecmint.com/wp-content/uploads/2016/03/Info-Coreutils.png)
>Info Coreutils

As it is the case with man pages, you can exit an info document by pressing the `q` key.

Additionally, GNU info can be used to display regular man pages as well when followed by the tool name. For example:

```
# info tune2fs
```

will return the man page of **tune2fs**, the ext2/3/4 filesystems management tool.

And now that we鈥檙e at it, let鈥檚 review some of the uses of **tune2fs**:

Display information about the filesystem on top of **/dev/mapper/vg00-vol_backups**:

```
# tune2fs -l /dev/mapper/vg00-vol_backups
```

Set a filesystem volume name (Backups in this case):

```
# tune2fs -L Backups /dev/mapper/vg00-vol_backups
```

Change the check intervals and `/` or mount counts (use the `-c` option to set a number of mount counts and `/` or the  `-i` option to set a check interval, where **d=days, w=weeks, and m=months**).

```
# tune2fs -c 150 /dev/mapper/vg00-vol_backups # Check every 150 mounts
# tune2fs -i 6w /dev/mapper/vg00-vol_backups # Check every 6 weeks
```

All of the above options can be listed with the `--help` option, or viewed in the man page.

### Summary

Regardless of the method that you choose to invoke help for a given tool, knowing that they exist and how to use them will certainly come in handy in the exam. Do you know of any other tools that can be used to look up documentation? Feel free to share with the Tecmint community using the form below.

Questions and other comments are more than welcome as well.

--------------------------------------------------------------------------------

via: http://www.tecmint.com/linux-basic-shell-scripting-and-linux-filesystem-troubleshooting/

作者：[Gabriel Cánepa][a]
译者1：[tresspassing2](https://git.oschina.net/tresspassing2)
译者2：[译者ID](https://github.com/译者ID)
校对1：[tresspassing2](https://github.com/tresspassing2)
校对2：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://git.oschina.net/tresspassing2/TranslateProject) 原创翻译，[Linux中国](https://linux.cn/) 荣誉推出

[a]:http://www.tecmint.com/author/gacanepa/
[1]: http://www.tecmint.com/sed-command-to-create-edit-and-manipulate-files-in-linux/
[2]: http://www.tecmint.com/installing-network-services-and-configuring-services-at-system-boot/
[3]: http://www.tecmint.com/sed-command-to-create-edit-and-manipulate-files-in-linux/
[4]: http://www.tecmint.com/installing-network-services-and-configuring-services-at-system-boot/
[5]: http://www.tecmint.com/linux-directory-structure-and-important-files-paths-explained/
[6]: http://www.tecmint.com/configure-squid-server-in-linux/
[7]: http://www.tecmint.com/sed-command-to-create-edit-and-manipulate-files-in-linux/
[8]: 
