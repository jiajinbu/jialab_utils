# jialab_utils

jialab_utils 是本课题组的 **公共代码与脚本仓库**，用于集中管理大家经常复用的生信分析工具（脚本、Snakemake workflow、小函数库等）。

目标很简单：

- 避免同样的事情，每个人各写一份脚本、各踩一遍坑；
- 让常用任务（下载、比对、计数、QC、画图等）都有一份「组内推荐实现」；
- 让新同学入组时，可以先看这里，而不是从零开始找 / 写代码。

目前仓库仍在建设阶段，欢迎大家先把自己 **经常使用、觉得值得复用** 的脚本整理进来。

---

## 仓库结构（初步）

建议的基础结构如下，后续会逐步演化：

```text
jialab-utils/
├── README.md              # 仓库说明、使用指南、贡献规则（简要）
├── CONTRIBUTING.md        # 更详细的贡献说明
├── scripts/               # 各种可直接运行的脚本和小项目
│   ├── snakemake/
│   ├── python/
│   ├── r/
│   └── bash/
├── lib/                   # 可复用模块（import 用的 Python / R 包代码）
│   ├── python/
│   └── r/
└── docs/                  # 使用说明、教程、示例
```

---

## 适合放进这里的东西

可以考虑放进来的包括（但不限于）：

- **批量下载类脚本**
  - 根据 SRR / ERR / DRR / GSA / ENA 列表批量下载并转 fastq；
  - 根据 GEO / SRA 号抓取 meta 信息并整理成样本表。

- **标准分析步骤的小工具**
  - 整理 featureCounts / htseq-count 输出；
  - 合并多样本 counts、生成 DESeq2 输入矩阵；
  - 常用差异分析 / 可视化脚本（DESeq2、edgeR、limma 等）的小封装。

- **画图工具**
  - 常用火山图、热图、PCA、UMAP 的封装脚本；
  - 与论文图一致的「组内标准画法」脚本。

- **集群相关脚本**
  - 提交 job 的封装（qsub / sbatch 等）；
  - 监控内存 / CPU / 磁盘的小工具；
  - 帮助定位慢 job、炸 job 的脚本。

- **Snakemake workflow 模板**
  - 标准的 RNA-seq 流程骨架；
  - SRA 下载 + QC + 比对的统一模板；
  - 其他常见流程（ATAC-seq、ChIP-seq、单细胞等）的骨架。

简单判断标准：

> 如果你发现某个脚本自己已经用过 ≥ 2 次，  
> 或者你觉得「如果别人也能直接用，会节省很多时间」，  
> 那它基本就值得放进这里。

---

## 使用方法（简要）

### 1. 克隆仓库

```bash
git clone git@github.com:jialab/jialab_utils.git
cd jialab_utils
```

（地址以后根据实际仓库修改）

### 2. 查找已有脚本

- 直接浏览 `scripts/` 下面的目录，按语言 / 功能查找；
- 或在 GitHub 网页上用搜索（例如：`fasterq-dump`、`fastqc`、`deseq2` 等关键词）。

### 3. 按说明运行

- 大部分脚本会在开头注释或对应的 `docs/*.md` 中说明：
  - 这个脚本解决什么问题；
  - 输入文件格式 / 参数；
  - 输出是什么；
  - 一个最简单的「运行示例命令」。

如果说明不够，请在组内论坛 / Discussion / Issues 里 @ 原作者 或提问，顺便一起补文档。

---

## 如何贡献自己的脚本

> 第一次贡献不需要太正式，先保证脚本「能跑、有人能看懂」即可。

### 步骤 1：先在组内发帖做个简单说明

在组内的 GitHub Discussion / 论坛里，简单写一下：

- 脚本做什么；
- 现在放在哪里；
- 使用频率（经常 / 偶尔 / 一次性）；
- 你觉得是否适合整理进公共库。

便于大家一起判断优先级，避免重复劳动。

### 步骤 2：在本仓库中放置脚本

建议：

- **Python 脚本**：放在 `scripts/python/` 下面合适的子目录，例如：
  - `scripts/python/sra_download/`
  - `scripts/python/rnaseq/`
  - `scripts/python/qc/`
- **R 脚本**：放在 `scripts/R/…`；
- **bash 脚本**：放在 `scripts/bash/…`。

如果是 Snakemake 流程，可以建一个子目录，例如：

```text
scripts/snakemake/sra_download/
  ├── Snakefile
  ├── config.yaml.example
  └── README.md    # 或 docs/sra_download.md
```

### 步骤 3：给脚本加一个最小可用的说明

至少做到：

- 在脚本开头写清楚：
  - 解决什么问题；
  - 输入 / 输出（文件名模式、大致格式）；
  - 所依赖的软件（例如 sra-tools ≥ 3.0、samtools、R 包列表等）。
- 最好再写一个 `docs/xxx.md`，给出一两个完整的使用示例。

越清楚，越容易被别人用起来，也越容易被你自己几年后看懂。

### 步骤 4：提交修改

- 如果你熟悉 Git：新建分支 → 提交 → 发 Pull Request；
- 如果你不熟悉 Git：可以临时 push 到一个 `dev-你的名字` 分支，或者把脚本发给熟悉 Git 的同学帮你提 PR。

更详细的提交流程和规范见本仓库的 [CONTRIBUTING.md](CONTRIBUTING.md)。

---

## 一些小约定（初步）

为了方便大家使用和维护，建议遵守以下约定 —— 不必一开始就做到完美，但可以作为目标：

1. **不在仓库提交大数据文件**  
   - 不提交 fastq / bam / 大型中间文件（> 10 MB 尽量不要进仓库）；
   - 如需示例数据，可使用极小的 toy 数据（几百行），或提供下载链接。

2. **路径尽量不要写死**  
   - 使用相对路径和配置文件（YAML / TOML / JSON）；
   - 例如在 Snakemake 里通过 `config.yaml` 管理数据路径，而不是在脚本里写绝对路径 `/share/home/...`。

3. **脚本命名清晰**  
   - `download_sra_batch.py` 比 `test.py` 好；
   - `plot_volcano_deseq2.R` 比 `plot.R` 好；
   - 名字里尽量体现「做了什么 + 针对什么数据」。

4. **注明依赖环境**  
   - 在脚本或 README 中说明：需要的 Python/R 版本、主要依赖的包和第三方软件；
   - 对于复杂环境，可以提供 conda 环境文件（`env.yaml`）或 Dockerfile。

5. **避免把个人隐私 / 敏感路径写进脚本**  
   - 比如个人 home 目录、真实用户名等，尽量用占位符或变量。

---

## TODO（规划中）

- [ ] 整理一份「组内常用脚本清单」，标记优先需要重构 / 共享的脚本；
- [ ] 添加第一个 Snakemake 示例（例如 SRA 下载 + fastq 转换）；
- [ ] 添加标准 RNA-seq Snakemake 流程骨架，并与 bulib / 参考基因组管理对接；
- [ ] 完善 `CONTRIBUTING.md`，规范提交流程；
- [ ] 根据使用情况，逐步将常用功能抽取到 `lib/python/jialabtools/`、`lib/R/jialabtools/` 中。
