# Ubuntu20.04 apt源安装Texlive
**TeX**是由Donald Knuth于20世纪70年代开发的排版系统，广泛用于生成高质量的科技文档、学术论文、书籍、演示文稿等。TeX Live通过提供各种各样的功能扩展，使用户能够轻松地创建复杂且具有专业外观的文档。而**TeX Live**是一个用于排版文档的自由软件发行版，主要基于**TeX系统**。它包含了许多用于创建高质量文档的**工具**、**字体**、**宏包**等。**TeX Live**的目标是为用户提供一个全面、稳定且易于安装的TeX环境。TeX Live支持多个操作系统，包括Windows、macOS和各种Linux发行版，它更新频繁，因此用户可以获得最新的宏包和功能。它被广泛用于学术界、出版业和各种技术写作领域。

**TeX Live**包括以下主要组件：
  1. **TeX引擎**：TeX Live支持不同的TeX引擎，如pdfTeX、XeTeX和LuaTeX等，每个引擎都具有不同的特性和功能。

  2. **宏包和字体**：TeX Live包含了许多宏包和字体，这些是扩展TeX系统功能的模块，使用户能够添加各种格式、排版样式和图形等。

  3. **工具**：TeX Live还包含了许多辅助工具，用于编译、预览和处理TeX文档，以及处理图像、字体等。

  4. **文档**：发行版附带了大量的文档，包括用户手册、教程、参考资料等，帮助用户更好地使用TeX和相关工具。


Linux可通过从apt源下载或从[官网](https://tug.org/texlive/quickinstall.html)进行源码编译安装。

## 1 apt源命令安装
安装完整版（默认2019版本），Ubuntu 安装源已经打包了 TEX Live。对于大部分的用户，源里面的 TEX Live 安装简单，稳定性通常也足够，所以可以直接安装。为了避免宏包依赖问题，推荐安装完整版（如果磁盘空间足够）：
```shell
sudo apt-get install texlive-full
```
然而，源里面的 TEX Live 相比于「源码版」，也有一些缺点：
- 相比于几乎每日都有更新的 CTAN，源里面的 TEX Live 更新较慢，频率接近每月一次，对于宏包开发者和有特殊需要的 TEX 用户是远远不够的。
- 不使用 TEX Live 自带的包管理器，也就不能通过 tlmgr 安装、管理和更新宏包（当然实际上跟上一条说的是一件事）。
## 2 复制所需字体文件至Letex字体库
将[字体目录](https://github.com/HSqure/gpt_academic/tree/master/Fonts)下的所有字体文件复制到Latex的字体目录`/usr/share/texlive/texmf-dist/fonts/truetype`
```
MSYHBD.TTC  SIMLI.TTF  msyh.ttc simfang.ttf  simkai.ttf  SIMYOU.TTF  msyhbd.ttc  simhei.ttf   simsun.ttc
```
## 3 使用`texhash`命令刷新字体，使第四步生效
执行：
```shell
sudo texhash
```
## 4 Hello World测试
可以编译一个简短的测试文件`hello.tex`：
```tex
% hello.tex
\documentclass[UTF8]{ctexart}
\begin{document}
欢迎来到 \TeX{} 世界！
\end{document}
```
用 `xelatex`,`pdflatex` 或 `lualatex`编译：
```shell
xelatex hello
lualatex hello
```
## 附:安装 Tex Studio
非必须安装项目，Tex Studio属于letex编译的IDE界面，安装后可单独编译Letex工程。

命令行中执行：
```shell
sudo apt install texstudio -y
```
或者可以通过官网 [TeXstudio - A LaTeX editor](https://texstudio.sourceforge.net/) 介绍的方法进行安装。
打开Tex Studio后在菜单栏中找到`Options`点击`Configure TeXstudio`，找到`Commands`进行相应的配置（也可以暂不配置，我没有在这里配置路径也依然可以正常编译预览并生成PDF文件）：

- 配置 `latex` `pdflatex` `xeflatex` `lualatex` 的路径。
- 配置 `bibtex` `bibtex8` `biber` 的路径。
- 这几个路径可以通过在终端中通过`which latex、which bibtex`等进行查找。

# 新版本修复补丁

## Temporary Solution
- Enter the tex project path `/home/user-name/arxiv_cache/2001.01568/workfolder`(for example). 
- Delete the temporary files (.aux, .bbl, .bcf). Rebuild the project such as:
  ```shell
  pdflatex merge_translate_zh.tex
  ``` 
> [!NOTE]
>  If the problems persist, make sure both biblatex and BIber are up to date and their versions match. See [troubleshooting for Biber](http://tex.stackexchange.com/q/286706/35864) for general advice on that matter. This solution works for me.

## Solution (For ubuntu)
It's just need to update biblatex and BIber version. How to update biblatex and BIber version?
- Step 1: 
  ```shell
  sudo apt-get install texlive-full
  sudo apt-get remove biber
  sudo apt-get remove biblatex
  ```
- Step 2:
  Download latest `biber` binary [biber-linux_x86_64.tar.gz](http://sourceforge.net/projects/biblatex-biber/).
  download  latest `biblatex` source [biblatex-3.20.tds.tgz](http://sourceforge.net/projects/biblatex/). 
  ```shell
  mkdir tempbiber
  mkdir tempbiblatex
  tar -zxvf biber-linux_x86_64.tar.gz -C tempbiber/
  tar -zxvf biblatex-2.8a.tds.tgz -C tempbiblatex/
  sudo rsync -azvv tempbiber/ /usr/bin/
  #sudo rsync -azvv tempbiblatex/ /usr/share/texlive/texmf-dist/
  sudo rsync -azvv tempbiblatex/ /usr/local/share/texmf/
  ```
- Step 3:
  Relaunch `gpt_academic`.
  ```shell
  python main.py
  ``` 
Sorry I can't verify if it's work on `Windows`, anyway it works fine on my `Ubuntu 20.04`. For reference try this out and have a good luck!

## Reference
> [How to upgrade 'biblatex' properly?](https://tex.stackexchange.com/questions/84624/how-to-upgrade-biblatex-properly)
