# 贡献指南（CONTRIBUTING）

感谢你愿意为 jialab-utils 做贡献！本仓库的目标是逐步沉淀课题组内 **可复用的生信分析工具**，包括脚本、Snakemake 工作流和函数库。

下面是一个尽量简洁、可操作的贡献流程和一些约定。

---

## 1. 贡献前的准备

### 1.1 Git 基本流程

如果你已经熟悉 Git/GitHub，可以直接跳到「2. 提交步骤」。

如果不熟悉，建议简单了解以下几个命令（可以让同学帮忙演示一次）：

- `git clone`：把远程仓库克隆到本地；
- `git status`：查看当前修改状态；
- `git add`：选择要提交的文件；
- `git commit`：提交修改（附上一句说明）；
- `git push`：推送到远程仓库；
- `git pull`：从远程更新最新代码到本地。

暂时不会用也没关系，可以先把脚本发给熟悉 Git 的同学，由他/她帮你提交。

### 1.2 本地目录结构

建议把仓库放在一个固定位置，例如：

```bash
cd ~/code
git clone git@github.com:jialab/jialab-utils.git
cd jialab-utils
```

---

## 2. 提交脚本 / 工具的一般步骤

### 步骤 1：选择合适的放置位置

根据脚本类型，放到对应目录：

- Python 脚本：`scripts/python/` 下合适的子目录：
  - 例如：`scripts/python/sra_download/`、`scripts/python/rnaseq/` 等。
- R 脚本：`scripts/R/…`
- bash 脚本：`scripts/bash/…`
- Snakemake 流程：为这个流程建一个子目录，例如：
  ```text
  scripts/python/sra_download/
    ├── Snakefile
    ├── config.yaml.example
    └── README.md
  ```

对于会被多处复用的函数，可以考虑放到：

- `lib/python/jialabtools/` 下做成模块；
- `lib/R/jialabtools/` 下做成 R 函数集合。

### 步骤 2：补充最小说明文档

不要求一开始就写得很复杂，但至少包含：

- 这个脚本/流程 **解决什么问题**；
- **输入** 是什么（路径、文件名模式、列名等）；
- **输出** 是什么；
- 一个 **最简单的运行示例**：

  ```bash
  # 示例：
  python scripts/python/sra_download/download_sra_batch.py \
      --accessions accessions.txt \
      --outdir fastq
  ```

可以写在：

- 脚本开头的注释中；
- 或一个对应的 Markdown 文档中（例如：`docs/sra_download.md`）。

### 步骤 3：本地测试

在本地或测试目录里 **实际跑一遍**：

- 确保脚本本身能直接运行；
- 对于 Snakemake 流程，至少 `snakemake -n` 和一小部分真实任务能正常执行；
- 使用尽可能小的数据做 smoke test。

### 步骤 4：提交修改

如果你熟悉 Git，推荐以下流程：

```bash
# 1. 创建新分支（可选，但推荐）
git checkout -b feature/your_script_name

# 2. 添加文件
git add scripts/python/your_script.py docs/your_script.md

# 3. 提交
git commit -m "Add script: your_script_name for XXX"

# 4. 推送
git push origin feature/your_script_name
```

然后在 GitHub 上发起 Pull Request（PR），简单说明：

- 这个 PR 做了什么；
- 是否已经本地测试；
- 是否会影响现有脚本/流程。

如果你不熟悉 PR 流程，可以：

- 直接 push 到一个 `dev-你的名字` 分支；
- 或者把脚本/说明发给仓库维护者，由他/她帮你整理并提交。

---

## 3. 代码风格与命名约定

不强制很严格的风格，但建议尽量做到「自解释」：

1. **文件命名清晰**

   - 好的例子：
     - `download_sra_batch.py`
     - `plot_volcano_deseq2.R`
     - `run_rnaseq_snakemake.sh`
   - 不推荐的例子：
     - `test.py`
     - `tmp.R`
     - `new1.sh`

2. **变量命名尽量语义化**

   - 比如 `sample_table`、`count_matrix`、`bam_files`，而不是 `a`, `b`, `c`。

3. **适当加注释**

   - 对于不那么直观的步骤（特别是生信里参数含义复杂的地方），写 1～2 行中文或英文注释说明原因。

4. **避免硬编码个人路径**

   - 不要在脚本中直接写 `/home/yourname/...` 或集群上特定用户目录；
   - 用配置文件 / 命令行参数传入路径。

---

## 4. 环境与依赖

为方便复现，建议：

- 在脚本 / README 中写清楚依赖的软件版本，例如：
  - `sra-tools >= 3.0`
  - `samtools >= 1.15`
  - `python >= 3.9`、`R >= 4.2`
- 对于依赖较多的脚本或 Snakemake 流程，建议提供：
  - `envs/xxx.yaml`（conda 环境文件）；或
  - `requirements.txt` / `renv.lock`；或
  - Dockerfile / Singularity recipe。

例：conda 环境文件：

```yaml
name: sra-download
channels:
  - bioconda
  - conda-forge
dependencies:
  - sra-tools
  - pigz
```

---

## 5. 不要提交的内容

为了保持仓库轻量和可维护性，请 **不要** 提交：

- 大于 10 MB 的数据文件（fastq/bam/matrix 等）；
- 真实的敏感数据（患者信息、未公开的合作数据等）；
- 包含硬编码密码、token、个人账号的配置文件。

如果需要示例数据，可以：

- 使用公开的、极小的 toy 数据；
- 提供外部下载链接和说明。

---

## 6. 讨论与改进

- 对于你想引入的新工具/脚本，可以先在组内论坛 / GitHub Discussions 里开一个主题，简单描述需求和思路；
- 对现有脚本有改进建议（例如 bug 修复、性能优化、功能扩展），欢迎：
  - 直接提 PR；或
  - 提 Issue，简单描述问题和预期行为。

---

## 7. 维护者与联系方式

- 仓库维护者：**（请填写）**
- 如有不确定的地方，可以在 Issue / Discussion 中 @ 维护者，或在组内群里直接沟通。

谢谢你的贡献，让我们一起把「大家都在用的脚本」逐渐变成「大家都敢用、都看得懂的脚本」。
