# 营销通微应用 Skills Marketplace

这是一个 Claude Code 插件市场，提供营销通微应用开发的 AI 技能。

## 快速开始

### 安装 Marketplace

```bash
/plugin marketplace add your-org/microapp-skills-marketplace
```

### 安装微应用 SDK Skill

```bash
/plugin install microapp-sdk@microapp-skills-marketplace
```

## 功能

### 微应用 SDK (microapp-sdk)

提供完整的营销通微应用开发能力：

- ✅ ES5 语法规范和代码生成
- ✅ 完整的 API 参考文档
- ✅ 抽奖、签到、问卷等应用模板
- ✅ 多种设计风格（赛博霓虹、东方水墨、晶体折射、庆典纸屑）
- ✅ 双端支持（移动端、PC 端）

## 使用示例

安装后，可以直接向 Claude 请求开发微应用：

```
帮我开发一个抽奖微应用：
- 双端支持
- 赛博霓虹风格
- 一等奖1名、二等奖3名、三等奖10名
```

```
开发一个签到页面：
- 移动端
- 庆典纸屑风格
```

## 项目结构

```
microapp-skills-marketplace/
├── .claude-plugin/
│   └── marketplace.json          # 市场配置文件
├── plugins/
│   └── microapp-sdk/             # 微应用 SDK 插件
│       ├── .claude-plugin/
│       │   └── plugin.json       # 插件配置
│       ├── skills/
│       │   └── microapp-development/
│       │       └── SKILL.md      # 微应用开发技能
│       └── README.md             # 插件说明
└── README.md                     # 本文件
```

## 本地测试

```bash
# 添加本地 marketplace
/plugin marketplace add ~/workspace/microapp-skills-marketplace

# 安装插件
/plugin install microapp-sdk@microapp-skills-marketplace

# 测试技能
```

## 部署到 GitHub

1. 将此仓库推送到 GitHub
2. 用户可以通过以下方式安装：

```bash
/plugin marketplace add your-org/microapp-skills-marketplace
```

## 版本历史

| 版本 | 日期 | 变更说明 |
|------|------|----------|
| 1.0.1 | 2026-02-28 | 更新 API：数据库操作改为 db.*，埋点上报具体值（2035/2036），新增双端联动示例，完善工作流程 |
| 1.0.0 | 2026-01-19 | 初始版本，支持抽奖、签到等基础功能 |

## 许可证

MIT License

## 联系方式

- 技术支持：[联系方式]
- 问题反馈：[GitHub Issues]
