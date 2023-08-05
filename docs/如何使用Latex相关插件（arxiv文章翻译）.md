# Windows操作系统（推荐）

1. 使用方法一（见README.md），安装本项目
2. 安装Texlive
3. 确保pdflatex在PATH环境变量中（一般会第2步自动完成）
4. 确保系统中安装了宋体、黑体、楷体等中文字体

# 其他操作系统（方法1-推荐）

1. 使用docker-compose.yml中的方案4运行本项目


# 其他操作系统（方法2-非常熟悉linux的大师级用户）

1. 使用任意方法安装本项目
2. 安装Texlive
3. 确保pdflatex在PATH环境变量中（一般会第2步自动完成）
4. 请想办法合法获取以下字体，并复制到Latex的字体目录`/usr/local/texlive/2023/texmf-dist/fonts/truetype`
```
MSYHBD.TTC  SIMLI.TTF  msyh.ttc simfang.ttf  simkai.ttf  SIMYOU.TTF  msyhbd.ttc  simhei.ttf   simsun.ttc
```
5. 使用`texhash`命令刷新字体，使第四步生效
