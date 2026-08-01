# Multica Fork 改造与上游同步规范

本仓库是 [multica-ai/multica](https://github.com/multica-ai/multica) 的 fork,
用于本地改造,同时需要持续跟上上游更新。

## 分支模型(核心原则)

- **`main` 永远与上游一致,禁止直接在上面改造。**
- 所有改造都在独立分支上:从 main 切出,命名 `my/<描述>` 或自定义前缀。
- 改造尽量集中在独立文件/目录,少改上游原有文件,减少 merge 冲突。

```bash
# 开始一项改造
git checkout main
git pull origin main          # 确保 main 最新
git checkout -b my/feature-x  # 切改造分支
# ... 改造、提交 ...
```

## 拉取上游更新(常规流程)

```bash
git checkout main
git fetch upstream            # 拉上游所有更新
git merge upstream/main       # 合并到本地 main
git push origin main          # 同步到自己的 GitHub fork
```

> 上游有更新时,先同步 main,再把改造分支合并/变基到最新 main:
> ```bash
> git checkout my/feature-x
> git merge main              # 或 git rebase main
> ```

## 冲突处理

只有"上游改了某文件,而你也改了同一文件的同一区域"才会冲突。
- 大多数更新是零冲突的(上游没碰你改的文件)。
- 冲突时:打开文件找 `<<<<<<<` / `=======` / `>>>>>>>` 标记手动合并,
  解决后 `git add <文件>` + `git commit` 完成合并。
- 搞不定就回滚:`git merge --abort`。
- 推荐开启 rerere,让 Git 记住同类冲突的解法:

```bash
git config --global rerere.enabled true
```

## 提交规范

- 改造分支的提交用清晰前缀:`feat:` / `fix:` / `refactor:` / `docs:`。
- 不要改动上游原有提交历史;用 `git pull --rebase` 或 `git rebase` 保持线性。
- 只提交与本次改造相关的文件。

## 远程

```bash
origin     # https://github.com/chengxuyuanluyu/multica.git  你的 fork
upstream   # https://github.com/multica-ai/multica.git       上游原仓库
```

## 注意事项

- 上游 LICENSE 是自定义许可(非标准 MIT/Apache),发布/商用改版前先读 `LICENSE` 与 `NOTICE`。
- 上游改动较频繁,建议每周同步一次,避免积压太多冲突。
