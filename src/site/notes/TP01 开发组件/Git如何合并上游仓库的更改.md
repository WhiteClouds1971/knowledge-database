---
{"dg-publish":true,"permalink":"/TP01 开发组件/Git如何合并上游仓库的更改/","dgPassFrontmatter":true,"created":"2024-11-25T18:06:03.240+08:00","updated":"2024-11-25T18:08:57.078+08:00"}
---

1. **添加上流仓库**：

```zsh
git remote add upstream <上流仓库URL>
```

2. **拉取上流仓库的最新更改**：

```zsh
git fetch upstream
git checkout main
git merge upstream/main --allow-unrelated-histories
```

3. **解决冲突（如有）**： 

  解决任何合并冲突后，提交更改。

4. **推送更改到你的远程仓库**：

```zsh
git push origin main
```