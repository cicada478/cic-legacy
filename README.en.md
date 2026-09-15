# cic

[简体中文](README.md) | **English**

A personal R package for single-cell sequencing data analysis utilities. The current version provides `ggsave1()` for convenient plot export. 

## Installation

Install the latest development version from GitHub:

```r
install.packages("remotes")
remotes::install_github("cicada478/cic")
```

Install a specific release:

```r
remotes::install_github("cicada478/cic@v0.1.1")
```

Alternatively, download `cic_0.1.1.tar.gz` from [Releases](https://github.com/cicada478/cic/releases), then install it using the actual download path:

```r
install.packages("ggplot2")
install.packages("path/to/cic_0.1.1.tar.gz", repos = NULL, type = "source")
```

To install from a downloaded or cloned repository, set your R / RStudio working directory to the package root and run:

```r
install.packages("ggplot2")
install.packages(".", repos = NULL, type = "source")
library(cic)
```

## Saving plots

```r
library(ggplot2)
library(cic)

p <- ggplot(mtcars, aes(wt, mpg)) + geom_point()

# Append today's date, e.g. figures/scatter_20260910.pdf
ggsave1("figures/scatter.pdf", p, width = 6, height = 4)

# Save the last displayed plot by default
ggsave1("last_plot.png", width = 6, height = 4, dpi = 300)

# Omit the date suffix
ggsave1("scatter.pdf", p, add_date = FALSE)

# Use ggsave's path argument
ggsave1("scatter.pdf", p, path = "figures", width = 6, height = 4)

# Explicitly allow overwriting, useful in batch scripts
ggsave1("scatter.pdf", p, confirm = FALSE, add_date = FALSE)

# Capture the final filename, which is returned invisibly
saved_file <- ggsave1("result.pdf", p)
```

The default date suffix is `_YYYYMMDD`. If the final destination already exists, interactive R offers an overwrite / auto-rename / cancel menu. Auto-renaming adds `_1`, `_2`, and so on, skipping existing files. Cancellation stops the save operation with an error.

Starting with `v0.1.1`, non-interactive sessions such as `Rscript` automatically append `_1`, `_2`, and so on when the destination exists and `confirm = TRUE` (the default). Saving continues with the first unused filename instead of raising an error for the collision. For example, if `scatter_20260911.pdf` exists, the plot is saved as `scatter_20260911_1.pdf`; if that also exists, `_2` is used, and so on.

Interactive R still offers the overwrite / auto-rename / cancel menu. Explicitly setting `confirm = FALSE` allows overwriting in either mode. Missing output directories are created automatically. Capture the actual output path with `saved_file <- ggsave1(...)`.

Use `?cic::ggsave1` to view the help page. 

## Development

Package version: `0.1.1`. Dependencies: `ggplot2` and R's bundled `tools` and `utils` packages. Maintainer: cicada478 (cicada_7x@163.com).
