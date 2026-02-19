# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

Statusline Builder 是一个用于自定义 Claude Code 状态栏的可视化配置工具。用户可以通过浏览器界面选择不同的样式组合，一键生成并安装自定义状态栏脚本。

## 核心架构

项目是一个纯前端单页应用，所有代码集中在 `preview.html` 中：

- **HTML**: 页面布局（左侧固定预览面板 + 右侧滚动配置面板）
- **CSS**: 内联样式，使用 CSS 变量管理主题色
- **JavaScript**: 配置数据、状态管理、预览渲染、脚本生成

## 关键数据结构

### 配置选项 (JavaScript)

```javascript
progressStyles[]   // 6种进度条样式: classic, gradient, diamond, dot, arrow, emoji
colorPresets[]     // 10种配色方案，每个包含 ansi 颜色码和十六进制颜色
dirPrefixes[]      // 5种目录前缀
tokenFormats[]     // 3种 Token 显示格式
gitPrefixes[]      // 3种 Git 分支前缀
gitModes[]         // 2种 Git 状态模式
```

### 状态对象

```javascript
state = {
    progressStyle, colorPreset, dirPrefix, tokenFormat,
    previewMode, gitShow, gitPrefix, gitMode
}
```

## 核心函数

- `generateScript()`: 根据当前状态生成完整的 bash 脚本
- `updatePreview()`: 实时更新终端预览样式
- `copyInstallCmd()`: 将脚本 base64 编码后复制安装命令到剪贴板

## 安装机制

1. 用户点击复制按钮
2. `generateScript()` 生成 bash 脚本
3. 脚本被 base64 编码（避免 shell 引号问题）
4. 命令包含两个 base64 段：
   - 状态栏脚本 → `~/.claude/statusline.sh`
   - Python 配置脚本 → 更新 `~/.claude/settings.json`

## 前置要求

- 终端支持 256 色 (`xterm-256color`)
- 已安装 `jq`（用于解析 JSON）
- UTF-8 编码
