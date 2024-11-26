---
{"dg-publish":true,"permalink":"/TP01 开发组件/Git的Pull Request功能/","dgPassFrontmatter":true,"created":"2024-11-25T18:02:31.151+08:00","updated":"2024-11-25T18:06:25.991+08:00"}
---

**Pull Request (PR)** 是一种机制，允许开发者提议将自己对项目所做的更改合并到主仓库中。它通常包括代码审查和讨论过程，以确保代码质量。

**例子：**
1. **Fork 和 Clone**：Alice 发现了一个项目的 Bug，她 Fork 了该项目到自己的 GitHub 账户，并克隆到本地。
2. **创建分支**：Alice 在本地创建一个新分支 `fix-bug`。
3. **提交更改**：Alice 修复了 Bug，并将更改提交到她的 `fix-bug` 分支。
4. **推送更改**：Alice 将更改推送到她的远程仓库。
5. **发起 PR**：Alice 在 GitHub 上从她的 `fix-bug` 分支向项目的主分支发起一个 Pull Request，并描述了她的更改。
6. **审查和合并**：项目维护者 Bob 审查了 Alice 的 PR，提出了反馈，Alice 做了必要的修改后，Bob 合并了 PR。

这样，Alice 的修复就被合并到了主项目中。