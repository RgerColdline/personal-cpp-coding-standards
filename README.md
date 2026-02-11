
# 🌐 C++ 编码规范 (C++ Coding Standards)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Clang-Tidy](https://img.shields.io/badge/Clang--Tidy-14%2B-blue)](https://clang.llvm.org/extra/clang-tidy/)
[![Clang-Format](https://img.shields.io/badge/Clang--Format-14%2B-blue)](https://clang.llvm.org/docs/ClangFormat.html)

> **统一代码风格 · 捕获潜在缺陷 · 提升团队效率**  
> 本仓库包含团队 C++ 项目的**强制规范**与**工具配置**，开箱即用。

---

## 🎯 核心价值
| 工具            | 解决什么问题                   | 你的收益                        |
| --------------- | ------------------------------ | ------------------------------- |
| `.clang-format` | 代码缩进/括号/空格不一致       | **告别格式争论**，PR 只关注逻辑 |
| `.clang-tidy`   | 潜在 Bug / 安全漏洞 / 过时实践 | **提前拦截 80% 低级错误**       |
| 配套指南        | 配置复杂、踩坑多               | **5 分钟集成**，专注写代码      |

---

## 🚀 三步启用（任选其一）
### ✅ 方案1：作为 Git 子模块（推荐）
```bash
git submodule add https://github.com/RgerColdline/cpp-coding-standards.git
# 自动同步规范更新
```

### ✅ 方案2：直接复制文件
```bash
cp -r cpp-coding-standards/.clang* cpp-coding-standards/*.md your-project/
```

---

## 📚 文档导航

| 文档                                                        | 内容                                | 适合谁          |
| ----------------------------------------------------------- | ----------------------------------- | --------------- |
| [clang-tidy-guide.md](clang-tidy/clang-tidy-guide.md)       | Clang-Tidy 集成/配置/避坑指南       | 所有 C++ 开发者 |
| [clang-format-guide.md](clang-format/clang-format-guide.md) | Clang-Format 规则详解与自定义       | 关注代码风格者  |
| [.clang-tidy](clang-tidy/.clang-tidy)                       | **核心规则文件**（含安全/性能检查） | CI/工具链维护者 |
| [.clang-format](clang-format/.clang-format)                 | **核心格式文件**（缩进/括号/行宽）  | 所有提交代码者  |

---

## 🔍 规则速览
### ⚠️ Clang-Tidy 重点检查（安全优先）
```yaml
Checks: >
  bugprone-*,       # 悬空指针、未初始化等
  cert-*,           # 安全漏洞（缓冲区溢出等）
  cppcoreguidelines-*, # 现代 C++ 实践
  modernize-use-nullptr, # NULL → nullptr
  readability-identifier-naming # 命名规范
```
> 💡 **原则**：安全 > 性能 > 可读性（详细规则：[查看 .clang-tidy](clang-tidy/.clang-tidy)）

---

## ❓ 常见问题
### Q：规则太严格，影响开发效率？
> **A**：规范聚焦**安全与可维护性**，非个人偏好。  
> - 若遇合理场景需豁免：在代码中添加 `// NOLINT` 注释  
> - 规则调整需团队评审（[贡献指南](#-贡献指南)）

### Q：如何验证配置生效？
> **A**：  
> ```bash
> # 检查单个文件
> clang-tidy src/main.cpp -- -Isrc
> 
> # 格式化预览（不修改）
> clang-format --dry-run -Werror src/main.cpp
> ```

---

## 🤝 贡献指南
发现规则不合理？欢迎改进！
1. 🍴 Fork 本仓库
2. 🌿 创建分支 (`git checkout -b fix/rule-name`)
3. ✏️ 修改 `.clang-tidy` / `.clang-format` + 更新文档
4. 📤 提交 PR（附修改理由与示例）
5. 👥 团队评审后合并

> 📌 **原则**：  
> - 安全规则修改需提供漏洞案例  
> - 风格规则修改需团队共识  
> - 所有 PR 需通过 CI 检查

---

## 📜 许可证
本规范采用 [MIT License](LICENSE) — 欢迎 fork 与改进！

---

## 💬 反馈与支持
- 🐞 发现问题？[创建 Issue](https://github.com/yourteam/cpp-coding-standards/issues)
- 💡 有改进建议？直接提 PR！
- 📧 紧急问题？联系 [@规范维护者]（替换为实际负责人）

> **规范因讨论而完善，代码因规范而健壮** ✨
