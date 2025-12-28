# Rdx.js - 极简响应式前端框架

> Rdx.js 是一个仅11KB的轻量级响应式前端框架，无需构建步骤，通过 `<script>` 标签直接引入即可使用。它提供了现代化的响应式数据绑定、组件系统和路由功能，专为快速开发和简化前端工程化而设计。

## ✨ 特性亮点

- **⚡ 极致轻量** - 压缩后仅11KB，加载速度快
- **🚀 零配置起步** - 无需构建工具，直接HTML引入
- **🔄 响应式系统** - 简洁的 `ref()` 和 `reactive()` API
- **🧩 组件化** - 支持组件开发，自带样式隔离
- **🔧 丰富指令** - `r-if`、`r-for`、`r-model`、`r-click` 等
- **🗺️ 内置路由** - 轻量级客户端路由系统
- **📱 移动友好** - 完全响应式设计

## 🚀 快速开始

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cxfjh.cn/js/rdx/min.0.0.1.js"></script>
</head>
<body>
  <h1>计数器: {{ count }}</h1>
  <button r-click="count.value++">+1</button>
  
  <script>
    const count = ref(0);
  </script>
</body>
</html>
```

### 环境要求
- 现代浏览器（Chrome ≥ 79、Firefox ≥ 72、Edge ≥ 79、Safari ≥ 14）
- 无需 Node.js 环境，纯前端运行

### 引入框架
- [完整版本](https://cxfjh.cn/js/rdx/0.0.1.js)
- [压缩版本](https://cxfjh.cn/js/rdx/min.0.0.1.js)

## 📚 文档

- [API 参考](docs/USAGE.md)
- [更新日志](docs/CHANGELOG.md)

## 🎯 适用场景

- 快速原型开发
- 教学演示项目
- 小型 Web 应用
- 传统项目现代化改造
- 初学者学习框架原理

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！请查看 [贡献指南](CONTRIBUTING.md)。

## 📄 许可证

本项目基于 [MIT License](https://opensource.org/licenses/MIT) 开源，允许个人和商业项目自由使用、修改和分发。
