---
icon: lucide/monitor
---

# 我的开发环境搭建指南

:material-calendar: 2026-03-05 · :material-tag: 工具, 开发环境

---

工欲善其事，必先利其器。这篇文章分享我的开发环境配置，帮助你从零搭建一个高效舒适的开发环境。

## 编辑器

### VS Code

我的主力编辑器是 [VS Code](https://code.visualstudio.com/)，轻量但功能强大。

!!! info "推荐扩展"

    - **Python** — 官方 Python 扩展，必装
    - **GitLens** — 增强 Git 功能
    - **Error Lens** — 行内显示错误信息
    - **Todo Tree** — 高亮 TODO 注释

核心配置：

``` json title="settings.json"
{
    "editor.fontSize": 14,
    "editor.fontFamily": "JetBrains Mono, Consolas",
    "editor.fontLigatures": true,
    "editor.formatOnSave": true,
    "editor.minimap.enabled": false,
    "files.autoSave": "afterDelay",
    "terminal.integrated.fontSize": 13
}
```

## 终端

=== "Windows"

    推荐使用 **Windows Terminal** + **PowerShell 7**：

    ``` powershell
    # 安装 PowerShell 7
    winget install Microsoft.PowerShell

    # 安装 Oh My Posh（美化提示符）
    winget install JanDeDobbeleer.OhMyPosh
    ```

=== "macOS"

    推荐使用 **iTerm2** + **Zsh**：

    ``` bash
    # 安装 Oh My Zsh
    sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

    # 安装 Powerlevel10k 主题
    git clone --depth=1 https://github.com/romkatv/powerlevel10k.git \
        ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
    ```

=== "Linux"

    大多数发行版自带 Zsh，直接安装 Oh My Zsh：

    ``` bash
    # 安装 zsh
    sudo apt install zsh

    # 设置为默认 shell
    chsh -s $(which zsh)

    # 安装 Oh My Zsh
    sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
    ```

## Python 环境管理

### uv — 现代化的 Python 包管理器

我目前使用 [uv](https://docs.astral.sh/uv/) 来管理 Python 环境和依赖，它比传统工具快 10-100 倍。

``` bash title="uv 基本用法"
# 安装 uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# 创建项目
uv init my-project
cd my-project

# 添加依赖
uv add requests flask

# 运行脚本
uv run python main.py
```

!!! tip "为什么选择 uv？"

    - :zap: **极速** — 基于 Rust 编写，安装依赖飞快
    - :package: **一体化** — 管理 Python 版本、虚拟环境、依赖
    - :lock: **可靠** — 自动生成锁文件，确保环境可复现

## Git 配置

``` bash title="Git 基础配置"
# 用户信息
git config --global user.name "Your Name"
git config --global user.email "your@email.com"

# 常用别名
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.lg "log --oneline --graph --decorate"
```

??? example "我的 .gitignore 模板"

    ``` gitignore
    # Python
    __pycache__/
    *.py[cod]
    .venv/
    dist/
    *.egg-info/

    # 编辑器
    .vscode/
    .idea/
    *.swp

    # 系统
    .DS_Store
    Thumbs.db

    # 环境变量
    .env
    .env.local
    ```

## 总结

一个好的开发环境能极大提升工作效率和编码体验。关键原则：

1. **选择趁手的工具** — 不必追求最热门的，适合自己的才是最好的
2. **保持配置可迁移** — 用 dotfiles 仓库管理配置文件
3. **持续优化** — 发现痛点就解决，不要忍受低效的工作流

希望这篇文章对你有所启发！:rocket:
