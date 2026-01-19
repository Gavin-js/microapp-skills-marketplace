# 微应用 SDK Plugin for Claude Code

这是营销通微应用开发的 Claude Code 插件，提供 AI 辅助开发营销微应用的能力。

## 功能特性

- 🎯 **ES5 语法规范**：严格遵循 ES5 标准，确保代码兼容性
- 📱 **双端支持**：支持移动端和 PC 端开发
- 🎨 **多种设计风格**：赛博霓虹、东方水墨、晶体折射、庆典纸屑
- 🔧 **完整 API 参考**：CRM 对象操作、用户通讯录、消息广播、数据存储
- 📊 **数据模型**：活动成员、中奖记录等标准数据结构

## 安装方式

### 方式一：添加 Marketplace（推荐）

```bash
/plugin marketplace add your-org/microapp-skills-marketplace
```

### 方式二：直接安装 Plugin

```bash
/plugin install microapp-sdk@microapp-skills-marketplace
```

## 使用示例

### 开发抽奖应用

```
帮我开发一个抽奖微应用：
- 双端支持（移动端 + PC 端）
- 赛博霓虹风格
- 一等奖1名、二等奖3名、三等奖10名
```

### 开发签到应用

```
开发一个活动签到页面：
- 仅移动端
- 庆典纸屑风格
- 显示签到人数统计
```

### 开发调研问卷

```
开发一个调研问卷：
- 移动端
- 东方水墨风格
- 包含5个问题
```

## 技术规范

### 代码规范

微应用代码必须使用 ES5 语法：

```javascript
// ✅ 正确
var userName = '张三';
function createUser(name, email) {
  return { name: name, email: email };
}
FsYxtMicroApp.getCurrentUser().then(function(user) {
  console.log(user.name);
});

// ❌ 错误
const userName = '张三';
const createUser = (name, email) => ({ name, email });
```

### SDK 引入

```html
<!-- 开发环境：引入 Mock SDK -->
<script src="https://ceshi112.fspage.com/ec/h5-landing/release/microAppMockSDK.js"></script>

<!-- 生产环境：移除 Mock SDK，平台自动注入真实 SDK -->
```

## API 文档

### CRM 对象操作

```javascript
// 创建记录
FsYxtMicroApp.campaignMembers.create({
  name: '张三',
  phone: '13800138000',
  status: '已签到'
});

// 查询记录
FsYxtMicroApp.campaignMembers.query({
  filter: { status: '已签到' }
});

// 更新记录
FsYxtMicroApp.campaignMembers.update(id, { status: '已中奖' });
```

### 用户操作

```javascript
// 获取当前用户
FsYxtMicroApp.getCurrentUser().then(function(user) {
  console.log('当前用户：', user.name);
});

// 获取通讯录
FsYxtMicroApp.getAddressBook({
  departmentId: 'dept_sales',
  includeSubDept: true
});
```

### 消息广播

```javascript
// 发送广播
FsYxtMicroApp.broadcast({
  type: 'lottery_result',
  data: { winner: '张三' },
  to: 'all'
});

// 监听消息
FsYxtMicroApp.onMessage(function(message) {
  console.log('收到消息：', message);
});
```

## 数据模型

### 活动成员对象

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| name | String | ✅ | 姓名 |
| phone | String | ✅ | 手机号 |
| email | String | ❌ | 邮箱 |
| department | String | ❌ | 部门 |
| status | String | ❌ | 状态 |
| campaignId | String | ✅ | 活动 ID |
| signInTime | Number | ❌ | 签到时间戳 |
| isWinner | Boolean | ❌ | 是否中奖 |
| prizeType | String | ❌ | 奖项类型 |

## 设计风格

### 赛博霓虹 (cyber)
- 深色背景 + 霓虹渐变
- 适合科技感活动

### 东方水墨 (ink)
- 米色背景 + 红金点缀
- 适合传统文化活动

### 晶体折射 (crystal)
- 白色背景 + 玻璃质感
- 适合现代商务活动

### 庆典纸屑 (celebration)
- 浅色背景 + 金色渐变
- 适合喜庆活动

## 支持

如有问题，请联系：
- 技术支持：[联系方式]
- 问题反馈：[GitHub Issues]

## 许可证

MIT License
