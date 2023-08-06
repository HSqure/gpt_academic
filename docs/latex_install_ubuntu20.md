# Ubuntu20.04 apt源安装Texlive

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
## 附:安装 Tex Studio（属于letex编译的IDE界面，非必须）
命令行中执行：
```shell
sudo apt install texstudio -y
```
或者可以通过官网 [TeXstudio - A LaTeX editor](https://texstudio.sourceforge.net/) 介绍的方法进行安装。
打开Tex Studio后在菜单栏中找到Options点击Configure TeXstudio找到Commands进行相应的配置（也可以暂不配置，我没有在这里配置路径也依然可以正常编译预览并生成PDF文件）：

- 配置 `latex` `pdflatex` `xeflatex` `lualatex` 的路径。
- 配置 `bibtex` `bibtex8` `biber` 的路径。
- 这几个路径可以通过在终端中通过`which latex、which bibtex`等进行查找。

