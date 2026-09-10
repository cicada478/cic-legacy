# cic

**简体中文** | [English](README.en.md)

个人常用的单细胞测序数据分析 R 工具包。当前版本收录绘图保存函数 `ggsave1()`，后续可在 `R/` 中逐步添加其他工具。

## 安装

从 GitHub 安装最新开发版本：

```r
install.packages("remotes") # 已安装则跳过
remotes::install_github("cicada478/cic")
```

安装固定的发布版本：

```r
remotes::install_github("cicada478/cic@v0.1.0")
```

也可以从 [Releases](https://github.com/cicada478/cic/releases) 下载 `cic_0.1.0.tar.gz`，然后安装（将路径替换为实际下载位置）：

```r
install.packages("ggplot2") # 已安装则跳过
install.packages("path/to/cic_0.1.0.tar.gz", repos = NULL, type = "source")
```

下载或克隆仓库后，将 R / RStudio 的工作目录设为包根目录，然后运行：

```r
install.packages("ggplot2") # 已安装则跳过
install.packages(".", repos = NULL, type = "source")
library(cic)
```

## 保存图形

```r
library(ggplot2)
library(cic)

p <- ggplot(mtcars, aes(wt, mpg)) + geom_point()

# 自动添加当天日期，如 figures/scatter_20260910.pdf
ggsave1("figures/scatter.pdf", p, width = 6, height = 4)

# 默认保存最后显示的图
ggsave1("last_plot.png", width = 6, height = 4, dpi = 300)

# 不添加日期
ggsave1("scatter.pdf", p, add_date = FALSE)

# 同样支持 ggsave 的 path 参数
ggsave1("scatter.pdf", p, path = "figures", width = 6, height = 4)

# 明确允许覆盖，适合批处理脚本
ggsave1("scatter.pdf", p, confirm = FALSE, add_date = FALSE)

# 返回最终文件名（不可见返回，可通过赋值获取）
saved_file <- ggsave1("result.pdf", p)
```

默认日期格式沿用原函数的 `_YYYYMMDD`。当最终文件名已存在时，交互式 R 会提供“覆盖 / 自动重命名 / 取消”菜单；自动重命名使用 `_1`、`_2` 等编号，跳过已存在的文件。取消会终止本次保存。

非交互运行（例如 `Rscript`）遇到同名文件时会报错；可以换一个文件名，或显式设置 `confirm = FALSE` 允许覆盖。不存在的输出目录会自动创建。

`?cic::ggsave1` 可查看帮助。包内实现位于 `R/plot_utils.R`，辅助函数不导出。

## 开发

包版本为 `0.1.0`，目前仅依赖 `ggplot2` 及 R 自带的 `tools`、`utils`。维护者为 Cicada（cicada_7x@163.com）。

在 `R/` 中按功能添加函数，在 `man/` 中维护帮助文档，并在 `tests/` 中添加必要的行为检查。新增公开函数需在 `NAMESPACE` 中导出。

```sh
R CMD build .
R CMD check --no-manual cic_0.1.0.tar.gz
```
