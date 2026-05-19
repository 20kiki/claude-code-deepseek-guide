# 贡献指南

感谢你对本项目的关注！欢迎参与改进这份教程。

## 如何贡献

### 报告问题

1. 先在 [Issues](../../issues) 中搜索是否已有相同问题
2. 如果没有，使用 Bug 报告模板创建新的 Issue
3. 尽可能提供复现步骤和环境信息（操作系统、Claude Code 版本等）

### 提交改进

1. Fork 本项目
2. 创建功能分支：`git checkout -b feature/your-update`
3. 修改内容
4. 提交变更：`git commit -m "docs: 更新 XX 说明"`
5. 推送到分支：`git push origin feature/your-update`
6. 创建 Pull Request，描述你改了什么、为什么

### 内容规范

- 所有安装命令必须在对应平台上实际测试通过
- 截图放到 `images/` 目录，使用相对路径引用
- 中文为主，关键术语保留英文对照（如 `API Key`、`settings.json`）
- 提交信息遵循 [Conventional Commits](https://www.conventionalcommits.org/) 规范

## 提交信息规范

```
docs: 更新 XX 安装步骤
fix: 修正 XX 命令错误
feat: 新增 XX 平台说明
chore: 整理文件结构
```
