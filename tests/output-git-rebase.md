# Knowledge Cards: Git 交互式 rebase 入门指南

Source: sample-git-rebase.md · 6 cards

## Card 1: rebase 的作用

- **Core knowledge:** `git rebase` 的作用是把一串提交"搬"到另一个基点上重新播放，最终得到一条线性历史。
- **Explanation:** 假设从 main 分支切出 feature 后，main 又前进了几个提交，执行 `git rebase main` 会把 feature 上的提交逐个重新应用到最新 main 之上，使 feature 的历史看起来像是基于最新 main 开发的一条直线。
- **Example / Self-test:** 在 feature 分支上执行 `git rebase main`，feature 上的提交就会被逐个重新应用到最新 main 之上。

## Card 2: rebase 与 merge 的区别

- **Core knowledge:** `git merge` 会生成一个合并提交并保留真实的分叉历史，而 rebase 重写历史、得到直线。
- **Explanation:** 两种方式都能让分支获得 main 上的新提交，但历史形态不同：merge 保留分支曾经分叉的事实，rebase 则通过重新播放提交消除分叉痕迹。
- **Example / Self-test:** 如果希望整合 main 的更新后得到一条线性历史、而不是保留分叉和合并提交，应该选择 merge 还是 rebase？

## Card 3: 启动交互式 rebase

- **Core knowledge:** 要整理最近的 N 个提交，执行 `git rebase -i HEAD~N`，Git 会打开编辑器，按时间从旧到新列出这些提交，每行前面是操作命令。
- **Explanation:** 编辑器中的提交列表就是 rebase 的执行清单：保存退出后 Git 按列表依次执行，因此整理历史的方式就是在打开编辑器时修改这个列表。
- **Example / Self-test:** 执行 `git rebase -i HEAD~3`，编辑器会按时间从旧到新列出最近的 3 个提交供整理。

## Card 4: 交互式 rebase 常用操作命令

- **Core knowledge:** 交互式列表中每行前的命令决定该提交如何处理：`pick` 保留不改，`reword` 保留改动但改提交信息，`squash` 合并进上一个提交并融合提交信息，`fixup` 类似 squash 但丢弃该提交自己的信息，`drop` 删除该提交；在编辑器里移动对应行即可调整提交顺序。
- **Explanation:** 这五个命令构成整理提交的完整操作集：保留、改信息、合并（保留或丢弃信息）、删除，配合移动行调整顺序；保存退出后 Git 按修改后的列表依次执行。
- **Example / Self-test:** 想把某个提交合并进上一个提交，并且直接丢弃这个提交自己的提交信息，应该使用哪个操作命令？

## Card 5: rebase 冲突处理

- **Core knowledge:** 重放过程中发生冲突时 rebase 会暂停；手动解决冲突并 `git add` 后，执行 `git rebase --continue` 继续；想放弃整个过程则执行 `git rebase --abort` 回到 rebase 前的状态。
- **Explanation:** 暂停后仓库停在冲突点，需要人工决定保留哪些内容，暂存后再让 rebase 续跑；如果无法解决或不想继续，abort 可以整体撤销本次 rebase 的影响。
- **Example / Self-test:** rebase 重放到一半发生冲突且无法快速解决，想要放弃整个过程、回到 rebase 开始前的状态，应该执行哪条命令？

## Card 6: rebase 的安全红线

- **Core knowledge:** 不要对已经推送到共享分支、可能被别人拉取的提交做 rebase；rebase 只应用于尚未分享的本地提交。
- **Explanation:** rebase 会生成全新的提交 ID，即使改动内容相同，提交在 Git 看来也是全新的对象；其他人本地基于旧提交的工作再推送时就会产生混乱。
- **Example / Self-test:** 为什么已经推送到共享分支的提交不应该做 rebase？
