安装Git的两种方法：
 1. 通过Linux发行版的包管理器安装已经编译好的二进制格式的Git软件包
 2. 从Git源码开始安装

### Linux下安装Git
#### 包管理器方式安装
Ubantu10.10（maverick）及以上、Debian（squeeze）及以上：
```shell
$ sudo aptitude install git
$ sudo aptitude install git-doc git-svn git-email gitk
```
其中git软件包包含了大部分Git命令，是必装的软件包。git-svn、git-email和gitk本来也是Git软件包的一部分，但是因为有着不一样的软件包依赖（如更多perl模组、tk等），所以单独作为软件包发布

RHEL、Fedora、CentOS等版本：
```shell
$ sudo install git
$ sudo install git-svn git-email gitk
```
#### 从源码开始安装
访问Git的官方网站：`http://git-scm.com/`，下载Git源码包，例如：`git-2.19.0.tar.gz`

展开源码包，并进入到相应的目录中。
```shell
$ tar -jxvf git-2.19.0.tar.gz
$ cd git-2.19.0
```
进入该目录可以找到一个INSTALL文档，所有的编译命令都可以在其中找到，参照其中的指令提示完成安装。

下面的命令将Git安装在`/user/local/bin`中。
```shell
$ make prefix=/user/local all
$ sudo make prefix=/user/local install
```
安装Git文档（可选）
```shell
$ make prefix=/user/local doc info
$ sudo make prefix=/user/local install-doc install-html install-info
```
#### 命令补齐
Linux的shell环境（bash）通过bash-completion软件包提供命令补齐功能，能够实现在录入命令参数时按一下或两下`TAB`键，实现参数的自动补齐或提示。例如输入 `git com`后按下`TAB`键，会自动补齐为`git commit`。
将Git源码包中的命令补齐脚本复制到bash-completion对应的目录中:
```shell
$ cp contrib/completion/git-completion.bash /etc/bash_completion.d/
```
重新加载自动补齐脚本，使之在当前shell中生效：
```shell
$ . /etc/bash_completion
```
为了能够在终端开启时自动加载`bash_completion`脚本，需要在本地配置文件 `~/.bash_profile`或全局文件`/etc/bashrc`文件中添加下面的内容：
```
if [ -f /etc/bash_completion ]; then
. /etc/bash_completion
fi
```
### Windows下安装Git
目前 Git 提供的 Windows 安装包自带 MinGW (Minimalist GNU for Windows，最简GNU工具集)， 在安装后MinGW 提供了一个bash提供的shell环境 (Git Bash) 以及其他相关工具软件，组成了一个最简系统（Minimal SYStem），这样在 Git Bash 中，Git的使用和在Linux下使用完全一致。
#### 安装Git
1. 到 [https://git-scm.com/download/win](https://git-scm.com/download/win) 下载 Windows 安装包，例如：Git-2.30.1-64-bit.exe，执行开始安装，如下图：![[Pasted image 20210308113106.png]]
2. 选择一些必要的组件，开源的 git-lfs 存在一些问题，建议把勾选去掉
3. 在安装过程中会询问是否修改环境变量。建议选择“Use Git Bash Only”，即只在 MinGW 提供的shell环境中使用Git，不修改 PATH 环境变量，避免 Git 自带的工具与 Windows 下已有的产生冲突。(如果不清楚 PATH，可以参考[https://en.wikipedia.org/wiki/PATH\_%28variable%29](https://en.wikipedia.org/wiki/PATH_(variable)) ，简单来讲，就是你输出一条命令的时候，系统会从 PATH 这个配置中寻找实现这条命令的程序在哪里，找到后就启动程序。)
4. 其它后续提示可以都采用缺省配置，进行安装过程。安装完成后，我们可以在 Windows 任意目录下，右键单击选中 "Git Bash" 启动 Git Bash:![[Pasted image 20210308114330.png]]，可以执行`git version`查看安装的git的版本信息
#### 安装TortoiseGit
在Windows下安装和使用Git有两个不同的方案, 除了刚刚的 Git 安装包，再有一个就是基于msysGit的图形界面工具——TortoiseGit。
##### TortoiseGit简介
TortoiseGit提供了Git和Windows资源管理器的整合，提供了Git的图形化操作界面。
像其他Tortoise系列产品（TortoiseCVS、TortoiseSVN）一样，Git工作区的目录和文件的图标附加了标识版本控制状态的图像，可以非常直观的看到哪些文件被更改了需要提交。通过对右键菜单的扩展，可以非常方便的在资源管理器中操作Git版本库。

##### TortoiseGit安装
安装TortoiseGit非常简单，访问网站 http://code.google.com/p/tortoisegit/ ，下载安装包，然后根据提示完成安装。

安装过程中会询问要使用的SSH客户端，缺省使用内置的TortoisePLink（来自PuTTY项目）作为SSH客户端。TortoisePLink和TortoiseGit的整合性更好，可以直接通过对话框设置SSH私钥（PuTTY格式），而无需再到字符界面去配置SSH私钥和其他配置文件。

如果你的本地同时安装了命令行的 Git 版本，可以通过TortoiseGit的设置对话框选中 Git 提供的 ssh 客户端，这样在下载 ssh 协议的代码仓库的时候，通过命令行与 TortoiseGit 图形界面都可以使用同一套公钥和密钥。
### Git的配置
Git有三种配置，分别以文件的形式存放在三个不同的地方。可以在命令行中使用git config工具查看这些变量。
>注：每一个级别的配置都会覆盖上层的相同配置，例如 .git/config 里的配置会覆盖 %Git%/etc/gitconfig 中的同名变量。
1. 系统配置（对所有用户都适用）：
存放在git的安装目录下：`%Git%/etc/gitconfig`；若使用`git config`时用`--system`选项，读写的就是这个文件：
```shell
git config --system core.autocrlf
```
2. 用户配置（只适用于该用户）：
存放在用户目录下。例如linux存放在：`~/.gitconfig`；若使用`git config`时用 `--global`选项，读写的就是这个文件：
```shell
git config --global user.name
```
3. 仓库配置（只对当前项目有效）：
当前仓库的配置文件（也就是工作目录中的`.git/config`文件）；若使用`git config`时用`--local`选项，读写的就是这个文件：
```shell
git config --local remote.origin.url
```
#### 配置个人身份
```shell
git config --global user.name “Your Name”
git config --global user.email  email@example.com
```
这个配置信息会在Git仓库中提交的修改信息中体现，但与Git服务器认证使用的密码或者公钥密码无关。
#### 文本换行符配置
假如你正在Windows上写程序，又或者你正在和其他人合作，他们在Windows上编程，而你却在其他系统上，在这些情况下，你可能会遇到行尾 结束符问题。 这是因为Windows使用回车和换行两个字符来结束一行，而Mac和Linux只使用换行一个字符。 虽然这是小问题，但它会极大地扰乱跨平台协作。

Git可以在你提交时自动地把行结束符CRLF转换成LF，而在签出代码时把LF转换成CRLF。用core.autocrlf来打开此项功能， 如果是在Windows系统上，把它设置成true，这样当签出代码时，LF会被转换成CRLF：
```shell
$ git config --global core.autocrlf true
```
Linux或Mac系统使用LF作为行结束符，因此你不想Git在签出文件时进行自动的转换；当一个以CRLF为行结束符的文件不小心被引入时你肯定想进行修正， 把core.autocrlf设置成input来告诉Git在提交时把CRLF转换成LF，签出时不转换：
```shell
$ git config --global core.autocrlf input
```
这样会在Windows系统上的签出文件中保留CRLF，会在Mac和Linux系统上，包括仓库中保留LF。

如果你是Windows程序员，且正在开发仅运行在Windows上的项目，可以设置false取消此功能，把回车符记录在库中：
```shell
$ git config --global core.autocrlf false
```
#### 文本编码配置
为了避免在提交及查看代码时出现由编码不一致导致的乱码问题或者中文乱码问题，建议将编码配置都设置为UTF-8
>i18n.commitEncoding 选项：用来让git commit log存储时，采用的编码，默认UTF-8
>
>i18n.logOutputEncoding 选项：查看git log时，显示采用的编码，建议设置为UTF-8
```shell
# 中文编码支持
git config --global gui.encoding utf-8
git config --global i18n.commitencoding utf-8
git config --global i18n.logoutputencoding utf-8

# 显示路径中的中文：
git config --global core.quotepath false
```
#### 与服务器的认证配置
两种常见的认证方式：
- HTTP/HTTPS协议认证：
设置口令缓存（用于HTTP方式，使Git记住上一次认证成功的结果）：
```shell
git config --global credential.helper store
```
添加https证书信任（用于HTTPS方式，让服务端公钥自动成为一个可信结果）：
```shell
git config http.sslverify false
```
- SSH协议认证（推荐）：
SSH协议是一种非常常用的Git仓库访问协议，使用公钥认证、无需输入密码，加密传输，操作便利又保证安全
>SSH认证的配置过程：
>1. 生成公钥：
>运行 Git Bash，在弹出的客户端命令行界面中输入下面提示的命令：
(比如你的邮箱是 zhangsan1123\@Huawei.com)
>```shell
>$ ssh-keygen -t rsa –C zhangsan1123@huawei.com
>```
>点击回车后提示输入，输入要在其中保存密钥的文件。此时我们按 Enter，这将接受默认文件位置，即保存在C/Users/username/.ssh/id_rsa中。
>
>然后又有提示输入，输入一个密码，该密码为SSH密钥安全密码。
>
>为什么我们需要这个密码呢，因为“使用 ssh 密钥, 如果有人访问您的计算机, 他们还可以访问使用该密钥的每个系统。若要添加额外的安全层, 可以向 ssh 密钥添加密码。您可以使用安全地保存密码, 这样就不必重新输入密码。”
>
>详情请参看官方说明https://help.github.com/articles/working-with-ssh-key-passphrases/。
>
>对于我们个人使用来说，完全没必要添加安全密码，因此只要按Enter即可。
>效果大致如图所示：
>![[Pasted image 20210308154638.png]]
>![[Pasted image 20210308154121.png]]
>
>2. 添加公钥到代码平台
>我们需要先获取密钥内容，有两种方式，其一为找到该文件，用文本编辑器打开全选复制内容到剪切板，另一种方法为在Git Bash中输入
>```shell
>clip < ~/.ssh/id_rsa.pub
>```
>即可将文件内容复制到剪切板，当然如果你之前选择的不是该默认文件保存密钥，你需要修改上述代码。
>然后回到GitHub在个人头像处进入Settings页面
>![[Pasted image 20210308154900.png]]
>![[Pasted image 20210308154938.png]]
>![[Pasted image 20210308154944.png]]
>其中，title为描述性标签，“在 "标题" 字段中, 为新键添加描述性标签。例如, 如果您使用的是个人 mac, 则可以将此密钥称为 "个人 macbook air"。”，key内容则是刚才我们复制的密钥，输入完成后点击添加。
>![[Pasted image 20210308155006.png]]
>之后，我们也可以通过访问该页面来查看并修改密钥。
并且我们可以测试是否成功创建密钥。同样是在Git Bash中，输入
>
>```shell
>ssh -T git@github.com
>```
>如果输出的最后一句话为You’ve successfully authenticated, but GitHub does
not provide shell access，就表明你已成功创建并连接到你的GitHub。