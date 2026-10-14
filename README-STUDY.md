# 我的 LLM 学习仓库

直接在原章节 notebook 中修改代码、添加注释和 Markdown 理解、补充实验。整体学习路线与章节总结放在 Obsidian；有独立实现需求时再在对应章节增加文件。

## 分支与远程

- `upstream`：作者仓库，只用于获取更新，推送已禁用。
- `origin`：`DreamKongcheng/LLMs-from-scratch`，保存自己的学习版本。
- `main`：官方基线，跟踪 `upstream/main`。
- `study`：日常学习，跟踪 `origin/study`。

## 保存学习改动

在 `study` 上检查差异，仅暂存本次要保存的文件，然后提交、推送：

```bash
git status
git diff
# git add <本次修改的具体文件>
git commit -m "study: describe this learning change"
git push
```

书的 PDF、模型权重、大数据和环境目录保留本地，提交前检查暂存内容。

## 获取作者更新

先保存当前学习改动，确认工作区干净，再运行：

```bash
git fetch upstream
git log --oneline HEAD..upstream/main
git diff --stat HEAD...upstream/main
git merge upstream/main
# 检查受影响的章节，处理冲突后再推送
git push
```

需要更新个人 fork 的 main 时，在干净工作区运行：

```bash
git switch main
git merge --ff-only upstream/main
git push origin main
git switch study
```

双方修改同一单元格时仍需判断如何合并。独立的 Markdown 或实验单元格更便于保留自己的内容，减少无关的移动与重排。

## Notebook 差异与合并

保留 `nbdime` 的结构化 diff/merge，不自动清理原 notebook 输出。作者原文件已有输出，统一过滤会制造大量无关差异；提交前自行检查代码、说明、输出及执行状态。

当前 Mac Studio 的工具环境位于 `~/Code/llm/.git-tools`。新电脑或新克隆需要重新安装并设置仓库本地配置，在仓库根目录执行：

```bash
uv venv --python 3.13 ~/Code/llm/.git-tools
uv pip install --python ~/Code/llm/.git-tools/bin/python nbdime==4.0.4
git config --local diff.jupyternotebook.command "$HOME/Code/llm/.git-tools/bin/git-nbdiffdriver diff"
git config --local merge.jupyternotebook.driver "$HOME/Code/llm/.git-tools/bin/git-nbmergedriver merge %O %A %B %L %P"
git config --local merge.jupyternotebook.name "jupyter notebook merge driver"
```

这些配置只作用于本仓库。工具环境目录不在仓库内；完整硬件实测与环境版本记录保留在 Obsidian。
