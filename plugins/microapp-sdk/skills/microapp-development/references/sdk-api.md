# SDK And API Reference

## Contents

- ES5 约束
- 页面骨架
- SDK 保护写法
- 常用 API 选型

## ES5 约束

微应用代码直接在浏览器中执行，严格使用 ES5。

禁止：

- `const` / `let`
- 箭头函数
- 模板字符串
- `class`
- `async/await`
- 可选链和空值合并
- `forEach` / `for...of`

使用：

- `var`
- `function`
- 字符串拼接
- `Promise.then().catch()`
- 经典 `for` 循环

## 页面骨架

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>微应用</title>
  <script src="https://ceshi112.fspage.com/ec/h5-landing/release/microAppMockSDK.js"></script>
</head>
<body>
  <div id="app"></div>
  <script>
    (function() {
      // ES5 code here
    })();
  </script>
</body>
</html>
```

说明：

- 开发态可以引入 Mock SDK。
- 生产态移除 Mock SDK，依赖平台注入真实 SDK。

## SDK 保护写法

```javascript
function hasSdk() {
  return typeof FsYxtMicroApp !== 'undefined';
}

function safeResolve(value) {
  return Promise.resolve(value);
}

function getPlatform() {
  if (hasSdk() && FsYxtMicroApp.platform) {
    return FsYxtMicroApp.platform;
  }
  return window.innerWidth < 768 ? 'mobile' : 'web';
}
```

## 常用 API 选型

### 自建表存储：`FsYxtMicroApp.db.*`

适用：

- 签到记录
- 抽奖池
- 中奖记录
- 问卷提交

```javascript
FsYxtMicroApp.db.create('campaign_members', data);
FsYxtMicroApp.db.query('campaign_members', query);
FsYxtMicroApp.db.update('campaign_members', id, patch);
FsYxtMicroApp.db.delete('campaign_members', id);
```

规则：

- 同一业务对象始终读写同一表名。
- 在代码顶部声明表名常量，避免写入 `signed_users`、读取 `campaign_members` 这类不一致问题。

### CRM 对象：`FsYxtMicroApp.object.*`

适用：

- 用户明确要求落到 CRM 业务对象
- 已经提供对象 `apiName` 和字段定义

```javascript
FsYxtMicroApp.object.create('CampaignMembersObj', data);
FsYxtMicroApp.object.query('CampaignMembersObj', query);
FsYxtMicroApp.object.update('CampaignMembersObj', id, patch);
```

选择原则：

- 不确定时优先 `db.*`。
- 只有在对象结构明确且需要与 CRM 其他模块共享时才选 `object.*`。

### 应用级配置：`storeAppData()` / `getAppData()`

适用：

- 奖项配置
- 中奖偏好
- 轻量主题配置

```javascript
FsYxtMicroApp.storeAppData('lottery_prizes', prizes);
FsYxtMicroApp.getAppData('lottery_prizes').then(function(value) {
  console.log(value);
});
```

### 当前用户：`getCurrentUser()`

适用：

- 自动填充签到信息
- 判断员工、会员、访客身份

```javascript
FsYxtMicroApp.getCurrentUser().then(function(user) {
  if (user && user.name) {
    console.log(user.name);
  }
});
```

如果拿不到用户：

- 提供手动输入降级方案
- 不要让页面卡死在加载态

### 会议签到：`conferenceSignIn()`

适用：

- 用户明确要求走系统会议签到能力
- 当前页面存在 `marketingEventId`

```javascript
FsYxtMicroApp.conferenceSignIn({ contact: '13800138000' });
```

### 参会人员查询：`queryConferenceParticipants()`

适用：

- 使用系统参会成员列表作为抽奖池
- 实时查看已报名或已签到名单

```javascript
FsYxtMicroApp.queryConferenceParticipants({
  marketingEventId: FsYxtMicroApp.getParam('marketingEventId'),
  pageNum: 1,
  pageSize: 100
});
```

### 双端同步：`broadcast()` / `onMessage()` / `startMessagePolling()`

适用：

- 移动端签到后通知 PC 端刷新
- 移动端发送互动消息
- PC 端开奖后通知移动端

```javascript
FsYxtMicroApp.broadcast({
  type: 'user_signin',
  data: { name: '张三' }
});

FsYxtMicroApp.onMessage(function(message) {
  console.log(message.type);
});
```

规则：

- 先持久化，再广播。
- 高频刷新场景由 PC 端按需开启 `startMessagePolling()`。

### CTA：`FsYxt.CtaSDK`

适用：

- 参与抽奖或签到前必须先完成 CTA 引导

```javascript
var cta = FsYxt.CtaSDK({
  ctaId: FsYxtMicroApp.getParam('ctaId'),
  autoExecuteOnReady: true
});

cta.onActionComplete(function() {
  console.log('cta done');
});
```

规则：

- 如果用户没有要求 CTA，不要默认强加。
- 如果需要 CTA，但 `ctaId` 未硬编码，优先从业务参数读取。

### 埋点：`track()`

```javascript
FsYxtMicroApp.track(2035);
FsYxtMicroApp.track(2036, '签到成功');
```

默认建议：

- 页面可用后记录 `2035`
- 用户完成参与动作后记录 `2036`

### URL 参数

优先使用 SDK 的 `getParam()`；没有时再回退到 `URLSearchParams`。

```javascript
function getParam(name) {
  if (hasSdk() && FsYxtMicroApp.getParam) {
    return FsYxtMicroApp.getParam(name);
  }
  return new URLSearchParams(window.location.search).get(name) || '';
}
```

## 可选能力

`FsYxtMicroApp.llm()` 只在用户明确要求 AI 生成、总结或智能问答能力时再接入；不要默认把大模型放进普通签到、抽奖或问卷页面。
