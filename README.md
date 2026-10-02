# cic

**简体中文** | [English](inst/doc/README.en.md)

个人常用的单细胞测序数据分析 R 工具包。当前版本收录绘图保存函数 `ggsave1()`。

## 安装

从 GitHub 安装最新开发版本：

```r
install.packages("remotes")
remotes::install_github("cicada478/cic")
```

安装固定的发布版本：

```r
remotes::install_github("cicada478/cic@v0.1.1")
```

也可以从 [Releases](https://github.com/cicada478/cic/releases) 下载 `cic_0.1.1.tar.gz`，然后安装（将路径替换为实际下载位置）：

```r
install.packages("ggplot2")
install.packages("path/to/cic_0.1.1.tar.gz", repos = NULL, type = "source")
```

下载或克隆仓库后，将 R / RStudio 的工作目录设为包根目录，然后运行：

```r
install.packages("ggplot2")
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

从 `v0.1.1` 起，非交互运行（例如 `Rscript`）在默认 `confirm = TRUE` 下遇到同名文件时，会自动添加 `_1`、`_2` 等编号并继续保存，跳过已有文件，不再因重名报错。例如 `scatter_20260911.pdf` 已存在时，保存为 `scatter_20260911_1.pdf`；若该文件也存在，则使用 `_2`，依此类推。

交互式 R 仍提供“覆盖 / 自动重命名 / 取消”菜单。任何运行方式下，显式设置 `confirm = FALSE` 都会允许覆盖。不存在的输出目录会自动创建；可通过 `saved_file <- ggsave1(...)` 获取实际保存路径。

`?cic::ggsave1` 可查看帮助。

## 开发

包版本为 `0.1.1`，目前仅依赖 `ggplot2` 及 R 自带的 `tools`、`utils`。维护者为 cicada478（cicada_7x@163.com）。

需要先安装 R 和 `ggplot2`。从包根目录运行以下终端命令（需要将 R 加入 PATH）：

```sh
R CMD build .
R CMD check --no-manual cic_0.1.1.tar.gz
```

`R CMD check` 会安装包，运行帮助示例及 `tests/ggsave1.R`。预期结果为 `Status: OK`；测试覆盖真实 PDF 保存、日期和编号后缀、非交互重名保护、显式覆盖及参数校验。

已验证环境：Windows、R 4.4.1、ggplot2 4.0.3。GitHub Actions 配置在 Windows 和 Ubuntu 的当前 R release 上执行包检查；具体通过情况以 [Actions](https://github.com/cicada478/cic/actions) 为准，不表示所有 R 版本均已测试。

英文文档放在 `inst/doc/`，会随后续构建的源码包一起分发。已发布的旧版本附件保持原样。

问题反馈请使用 [GitHub Issues](https://github.com/cicada478/cic/issues)，代码改进通过 Pull Request 提交。

## 许可证

本项目采用 MIT 许可证，完整正文见 [LICENSE.md](https://github.com/cicada478/cic/blob/main/LICENSE.md)。R 包保留 `MIT + file LICENSE` 声明及 `LICENSE` 中的年份、版权人信息。
