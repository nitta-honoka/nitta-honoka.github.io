---
title: 闲谈 git merge 与 git rebase 的区别
date: 2016-04-28 23:21:36
categories: 编程技术
tags: Git

---

相信大部分使用 Git 的朋友都会遇见相同的疑问，并且也从网上搜索了不少资料。那么，为什么我还要写这篇文章呢？因为我想尝试从自己的角度解释这个问题，如果能给到大家灵光一闪的感悟，便善莫大焉啦。估计点进来的朋友也对 merge 和 rebase 有了一定了解，所以我也就不浪费篇幅再去详细介绍 merge 和 rebase，让我们直入主题吧。

<!--more-->

## merge 与 rebase 的区别

### merge(以下说明都基于 merge 的默认操作)

现在假设我们有一个主分支 master 及一个开发分支 deve，仓库历史就像这样：

![初始仓库历史][1]

现在如果在 master 分支上 `git merge deve`：Git 会自动根据两个分支的共同祖先即 `e381a81` 这个 commit 和两个分支的最新提交即 `8ab7cff` 和 `696398a` 进行一个三方合并，然后将**合并中修改的内容生成一个新的 commit**，即下图的 `78941cb`。

![merge 合并图][2]

### rebase

rebase 是什么情况呢？还是一个初始的仓库历史图：

![rebase初始仓库历史][3]

如果是在 master 分支上 `git rebase deve`：Git 会从两个分支的共同祖先 `3311ba0` 开始提取 master 分支（当前所在分支）上的修改，即 `85841be`、`a016f64` 与 `e53ec51`，再将 master 分支指向 deve 的最新提交（目标分支）即 `35b6708` 处，然后将刚刚提取的修改依次应用到这个最新提交后面。操作会舍弃 master 分支上提取的 commit，同时**不会像 merge 一样生成一个合并修改内容的 commit，相当于把 master 分支（当前所在分支）上的修改在 deve 分支（目标分支）上原样复制了一遍**,操作完成后的版本历史就像这样：

![rebase 合并图][4]

可以看见 master 分支从 deve 分支最新提交 `35b6708` 开始依次提交了自己的三个 commit（由于是提取修改后重新依次提交，故 commit 的 hash 码与上面的`85841be`、`a016f64`、`e53ec51` 不同）。

### rebase -i

rebase 操作加上 `-i` 选项可以更直观的看见被提取的 commit 信息。

仍然在 master 分支上 rebase deve 分支，不过这次要加上 `-i` 选项，即 `git rebase -i deve`，然后我们可以得到这样一个文本信息框

![rebase -i信息][5]

  - A 区域内的信息说明了这次 rebase 操作提取了哪些 commit 记录（`f9a7673` 与 `edb2ba2`），会连接到目标分支的哪个 commit （`9c86a5c`）后面。可以根据 B 区域中的命令说明修改 `pick` 为其他命令，对该次提取出来的 commit 做额外的操作
  - B 区域内说明了本次 rebase 操作可以选用的命令
  - 通过 `:wq` 保存退出后，就会按照刚刚在 A 区域内设定的命令处理 commit 并 rebase。

### 冲突处理策略的不同

- merge 遇见冲突后会直接停止，等待手动解决冲突并重新提交 commit 后，才能再次 merge
- rebase 遇见冲突后会暂停当前操作，开发者可以选择手动解决冲突，然后 `git rebase --continue` 继续，或者 `--skip` 跳过（注意此操作中当前分支的修改会直接覆盖目标分支的冲突部分），亦或者 `--abort` 直接停止该次 rebase 操作

## 总结：选择 merge 还是 rebase？

- merge 是一个合并操作，会将两个分支的修改合并在一起，默认操作的情况下会提交合并中修改的内容
- merge 的提交历史忠实地记录了实际发生过什么，关注点在真实的提交历史上面
- rebase 并没有进行合并操作，只是提取了当前分支的修改，将其复制在了目标分支的最新提交后面
- rebase 的提交历史反映了项目过程中发生了什么，关注点在开发过程上面
- merge 与 rebase 都是非常强大的分支整合命令，没有优劣之分，使用哪一个应由项目和团队的开发需求决定。但是在大部分情况下，个人建议使用 rebase + merge --ff-only 的组合，这样可以使整个版本历史更加简洁，在大型项目中的效果可以参考 GO 的版本历史。
- merge 和 rebase 还有很多强大的选项，可以使用 `git help <command>` 查看


## 最后：一些注意点

- 使用 merge 时应考虑采用默认操作，还是 `--no-ff` 或 `--ff-only` 的方式
- rebase 操作会丢弃当前分支已提交的 commit，故不要在已经 push 到远程，和其他人正在协作开发的分支上执行 rebase 操作
- 与远程仓库同步时，使用 pull 命令默认进行了 `git fetch + git merge` 两个操作，可以通过加上 `--rebase` 命令将 fetch 后的 merge 操作改为 rebase 操作，或者仅仅 'git fetch remoteName',然后才思考采取哪种整合策略 `git merge(or rebase) origin/master`
- 开发与 commit 时注意自己此时在哪个分支上
- 当有修改未 commit 时，不能进行 rebase 操作，此时可以考虑先用 `git stash` 命令暂存


## 参考
- [ProGit 2nd Edition](https://git-scm.com/book/zh/v2)
- [Stackoverflow:Is there a difference between git rebase and git merge --ff-only](http://stackoverflow.com/questions/28140434/is-there-a-difference-between-git-rebase-and-git-merge-ff-only)
- [Git分支管理策略](http://www.ruanyifeng.com/blog/2012/07/git.html)


[1]: http://7xinvi.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-04-26%2021.58.22.png
  [2]: http://7xinvi.com1.z0.glb.clouddn.com/hexo%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-04-26%2022.08.28.png
  [3]: http://7xinvi.com1.z0.glb.clouddn.com/hexo%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-04-26%2022.20.15.png
  [4]: http://7xinvi.com1.z0.glb.clouddn.com/hexo%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-04-26%2022.26.23.png
  [5]: http://7xinvi.com1.z0.glb.clouddn.com/C48C3A61-64FD-4BD5-BD42-2CBF11937B4F.png
  [6]: /img/bVvcsQ
  [7]: /img/bVvcu7
