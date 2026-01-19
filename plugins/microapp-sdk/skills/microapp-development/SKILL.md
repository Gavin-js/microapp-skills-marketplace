---
name: microapp-development
description: 营销通微应用开发技能。使用当开发、调试或部署营销通微应用（抽奖、签到、问卷等）时。提供完整的 ES5 语法规范、API 参考和最佳实践。
---

# 营销通微应用开发 Skill (MicroApp Development)

此 Skill 专为营销通微应用开发设计，提供完整的开发规范、API 参考和最佳实践。

## 核心规范

### 1. 代码规范 - ES5 语法 ⚠️

微应用代码直接注入浏览器执行，**必须严格使用 ES5 语法**。

#### 禁止使用的 ES6+ 语法

```javascript
// ❌ 禁止使用
const/let              // 必须使用 var
() => {}              // 必须使用 function()
`${}`                  // 必须使用字符串拼接
class                 // 必须使用 function 构造器
async/await           // 必须使用 Promise.then()
?. / ??               // 可选链和空值合并
for...of / forEach    // 必须使用 for 循环
```

#### 正确写法示例

```javascript
// ✅ 变量声明
var userName = '张三';
var isLoggedIn = true;

// ✅ 函数定义
function createUser(name, email) {
  return {
    name: name,
    email: email,
    createdAt: Date.now()
  };
}

// ✅ Promise 使用
FsYxtMicroApp.getCurrentUser().then(function(user) {
  console.log('当前用户：', user.name);
}).catch(function(err) {
  console.error('获取用户失败：', err);
});
```

### 2. SDK 引入

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>微应用标题</title>

  <!-- 引入 Mock SDK（开发环境） -->
  <script src="https://ceshi112.fspage.com/ec/h5-landing/release/microAppMockSDK.js"></script>
</head>
<body>
  <script>
    // JavaScript 代码
  </script>
</body>
</html>
```

### 3. SDK 可用性检查

```javascript
function isSdkAvailable() {
  return typeof FsYxtMicroApp !== 'undefined';
}

function safeApiCall(apiFunc, fallbackValue) {
  if (isSdkAvailable() && apiFunc) {
    return apiFunc();
  }
  return Promise.resolve(fallbackValue);
}
```

## API 参考

### CRM 对象操作

```javascript
// 创建记录
FsYxtMicroApp.object.create(objectType, data);
FsYxtMicroApp.campaignMembers.create(data);

// 查询记录
FsYxtMicroApp.object.query(objectType, query);
FsYxtMicroApp.campaignMembers.query(query);

// 更新记录
FsYxtMicroApp.object.update(objectType, id, data);

// 删除记录
FsYxtMicroApp.object.delete(objectType, id);
```

### 用户与通讯录

```javascript
// 获取当前用户
FsYxtMicroApp.getCurrentUser().then(function(user) {
  console.log(user);
});

// 获取企业通讯录
FsYxtMicroApp.getAddressBook({
  departmentId: 'dept_sales',
  includeSubDept: true,
  fields: ['id', 'name', 'phone', 'department']
});
```

### 消息广播

```javascript
// 广播消息
FsYxtMicroApp.broadcast({
  type: 'lottery_result',
  data: { winner: '张三' },
  to: 'all'
});

// 监听消息
var unsubscribe = FsYxtMicroApp.onMessage(function(message) {
  if (message.type === 'lottery_result') {
    console.log('中奖者：', message.data.winner);
  }
});
```

### 数据存储

```javascript
// 应用私有数据（localStorage）
FsYxtMicroApp.storeAppData('key', value);
var value = FsYxtMicroApp.getAppData('key');

// 数据库存储（跨设备同步）
FsYxtMicroApp.db.create('tableName', data);
FsYxtMicroApp.db.query('tableName', query);
FsYxtMicroApp.db.update('tableName', id, data);
FsYxtMicroApp.db.delete('tableName', id);
```

## 数据模型

### 活动成员 (CampaignMembersObj)

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `name` | String | ✅ | 姓名 |
| `phone` | String | ✅ | 手机号 |
| `email` | String | ❌ | 邮箱 |
| `department` | String | ❌ | 部门 |
| `status` | String | ❌ | 状态：未报名/已报名/已签到/已中奖 |
| `campaignId` | String | ✅ | 活动 ID |
| `contactId` | String | ❌ | 联系人 ID |
| `signUpTime` | Number | ❌ | 报名时间戳 |
| `signInTime` | Number | ❌ | 签到时间戳 |
| `isWinner` | Boolean | ❌ | 是否中奖 |
| `prizeType` | String | ❌ | 奖项类型 |

### 中奖记录 (lottery_winners)

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `winnerId` | String | ✅ | 中奖者 ID |
| `winnerName` | String | ✅ | 中奖者姓名 |
| `winnerPhone` | String | ❌ | 中奖者手机号 |
| `winnerDepartment` | String | ❌ | 中奖者部门 |
| `prizeType` | String | ✅ | 奖项类型 |
| `prizeName` | String | ✅ | 奖品名称 |
| `prizeValue` | Number | ❌ | 奖品价值 |
| `campaignId` | String | ❌ | 活动 ID |
| `round` | Number | ✅ | 抽奖轮次 |
| `drawTime` | Number | ✅ | 抽奖时间戳 |

## 设计风格

### 风格 A：赛博霓虹脉冲 (cyber)

```css
:root {
  --bg-color: #0a0e27;
  --neon-pink: #ff00ff;
  --neon-cyan: #00ffff;
  --neon-yellow: #ffff00;
}

body {
  background: var(--bg-color);
}

.neon-text {
  color: var(--neon-cyan);
  text-shadow: 0 0 10px var(--neon-cyan), 0 0 20px var(--neon-cyan);
}

.neon-button {
  background: linear-gradient(90deg, var(--neon-pink), var(--neon-cyan));
  box-shadow: 0 0 20px var(--neon-pink);
}
```

### 风格 B：东方水墨流动 (ink)

```css
:root {
  --bg-color: #f5f0e6;
  --ink-red: #c8362e;
  --ink-gold: #d4af37;
}

body {
  background: var(--bg-color);
}

.ink-card {
  background: #fff;
  border-radius: 8px;
  border: 1px solid var(--ink-gold);
}
```

### 风格 C：晶体几何折射 (crystal)

```css
:root {
  --bg-color: #ffffff;
  --glass-bg: rgba(255, 255, 255, 0.2);
  --prism-gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.glass-card {
  background: var(--glass-bg);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.3);
}
```

### 风格 D：庆典纸屑狂欢 (celebration)

```css
:root {
  --bg-color: #fff5f5;
  --festive-red: #e63946;
  --gold-gradient: linear-gradient(135deg, #f5d061, #e6a04e);
}

.festive-button {
  background: var(--gold-gradient);
  color: #fff;
  border-radius: 25px;
  box-shadow: 0 4px 15px rgba(230, 57, 70, 0.3);
}
```

## 平台判断

```javascript
function getPlatform() {
  if (typeof FsYxtMicroApp !== 'undefined' && FsYxtMicroApp.platform) {
    return FsYxtMicroApp.platform;
  }
  return window.innerWidth < 768 ? 'mobile' : 'web';
}

var platform = getPlatform();

if (platform === 'mobile') {
  // 移动端代码
} else {
  // PC 端代码
}
```

## 完整示例：签到功能

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>活动签到</title>
  <script src="https://ceshi112.fspage.com/ec/h5-landing/release/microAppMockSDK.js"></script>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    .container { max-width: 400px; margin: 0 auto; }
    button { padding: 12px 24px; font-size: 16px; cursor: pointer; }
    #result { margin-top: 20px; }
  </style>
</head>
<body>
  <div class="container">
    <h1>活动签到</h1>
    <div id="user-info"></div>
    <button id="signin-btn">签到</button>
    <div id="result"></div>
  </div>

  <script>
    (function() {
      var userInfoDiv = document.getElementById('user-info');
      var resultDiv = document.getElementById('result');
      var signinBtn = document.getElementById('signin-btn');
      var currentUser = null;

      function loadUser() {
        if (typeof FsYxtMicroApp !== 'undefined' && FsYxtMicroApp.getCurrentUser) {
          FsYxtMicroApp.getCurrentUser().then(function(user) {
            if (user) {
              currentUser = user;
              userInfoDiv.innerHTML = '<p>欢迎：' + user.name + '</p>';
              checkSignInStatus();
            }
          });
        }
      }

      function checkSignInStatus() {
        if (typeof FsYxtMicroApp !== 'undefined' && FsYxtMicroApp.campaignMembers) {
          FsYxtMicroApp.campaignMembers.query({
            filter: { phone: currentUser.phone, status: '已签到' }
          }).then(function(result) {
            if (result.data && result.data.length > 0) {
              signinBtn.disabled = true;
              signinBtn.textContent = '已签到';
              resultDiv.innerHTML = '<p>签到时间：' + new Date(result.data[0].signInTime).toLocaleString() + '</p>';
            }
          });
        }
      }

      signinBtn.addEventListener('click', function() {
        if (!currentUser) {
          alert('请先获取用户信息');
          return;
        }

        if (typeof FsYxtMicroApp !== 'undefined' && FsYxtMicroApp.campaignMembers) {
          FsYxtMicroApp.campaignMembers.create({
            name: currentUser.name,
            phone: currentUser.phone,
            email: currentUser.email || '',
            department: currentUser.department || '',
            status: '已签到',
            campaignId: 'campaign_001',
            contactId: currentUser.id,
            signUpTime: Date.now(),
            signInTime: Date.now()
          }).then(function(result) {
            signinBtn.disabled = true;
            signinBtn.textContent = '已签到';
            resultDiv.innerHTML = '<p>签到成功！</p>';
            FsYxtMicroApp.track('signin_success', { userId: currentUser.id });
          }).catch(function(err) {
            resultDiv.innerHTML = '<p>签到失败：' + err.message + '</p>';
          });
        }
      });

      loadUser();
    })();
  </script>
</body>
</html>
```

## Skill 工作流程

当用户请求开发微应用时，按以下步骤进行：

### Step 1: 确认需求

询问用户以下信息：
1. 应用类型（抽奖/签到/问卷/其他）
2. 目标平台（移动端/PC 端/双端）
3. 设计风格偏好（cyber/ink/crystal/celebration）
4. 特殊功能需求

### Step 2: 生成代码

根据用户需求，生成符合规范的完整 HTML 文件：
- 严格 ES5 语法
- 包含 Mock SDK 引入
- 实现 SDK 可用性检查
- 使用正确的 API 调用
- 选择合适的设计风格

### Step 3: 代码验证

检查生成的代码是否符合：
- ✅ ES5 语法规范
- ✅ SDK 可用性检查
- ✅ API 调用正确性
- ✅ 错误处理完善

### Step 4: 输出结果

提供：
1. 完整的 HTML 代码
2. 使用说明
3. 注意事项

## 注意事项

1. **生产环境部署**：移除 Mock SDK 引用，使用平台注入的真实 SDK
2. **跨标签通信**：使用 `FsYxtMicroApp.broadcast()` 和 `onMessage()`
3. **数据持久化**：重要数据使用 `FsYxtMicroApp.db.*` 存储
4. **埋点上报**：关键操作使用 `FsYxtMicroApp.track()` 记录
5. **平台兼容**：始终使用 `FsYxtMicroApp.platform` 判断运行平台
