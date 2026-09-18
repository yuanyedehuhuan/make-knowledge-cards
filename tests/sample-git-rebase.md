# Git 交互式 rebase 入门指南

## rebase 做了什么

`git rebase` 的作用是把一串提交"搬"到另一个基点上重新播放。假设你从 main 分支切出 feature 后，main 又前进了几个提交，执行 `git rebase main` 会把 feature 上的提交逐个重新应用到最新 main 之上，最终得到一条线性历史。这与 `git merge` 不同：merge 会生成一个合并提交并保留真实的分叉历史，而 rebase 重写历史、得到直线。

## 启动交互式 rebase

要整理最近的 N 个提交，执行：

```
git rebase -i HEAD~3
```

Git 会打开编辑器，按时间从旧到新列出这 3 个提交，每行前面是操作命令。

## 常用操作命令

- `pick`：保留该提交，不做改动。
- `reword`：保留改动，但修改提交信息。
- `squash`：把该提交合并进上一个提交，并融合两者的提交信息。
- `fixup`：与 squash 类似，但直接丢弃该提交自己的信息。
- `drop`：删除该提交。

调整提交顺序只需在编辑器里移动对应行。保存退出后 Git 按列表依次执行。

## 冲突处理

重放过程中如果发生冲突，rebase 会暂停。手动解决冲突并 `git add` 后，执行 `git rebase --continue` 继续；想放弃整个过程、回到 rebase 前的状态，执行 `git rebase --abort`。

## 一条安全红线

不要对已经推送到共享分支、可能被别人拉取的提交做 rebase。因为 rebase 会生成全新的提交 ID，其他人本地基于旧提交的工作再推送时会产生混乱。rebase 只应用于尚未分享的本地提交。
