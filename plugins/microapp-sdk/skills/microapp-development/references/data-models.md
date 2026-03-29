# Data Models And Persistence

## Contents

- 持久化原则
- 推荐表结构
- 推荐应用配置键
- 生成后自查

## 持久化原则

- 先定义数据结构，再写交互逻辑。
- 关键业务状态必须可重建；刷新页面后不能丢。
- 内存数组只作为缓存或视图模型，不作为唯一数据源。
- 中奖广播、签到通知、提交成功提示，都应建立在持久化成功之后。

## 推荐表结构

### `campaign_members`

适用：

- 会议签到
- 签到抽奖
- 报名后进入抽奖池

建议字段：

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `name` | String | 姓名 |
| `phone` | String | 手机号或稳定联系标识 |
| `email` | String | 邮箱，可选 |
| `department` | String | 部门，可选 |
| `campaignId` | String | 活动 ID |
| `status` | String | `registered` / `signed` / `won` |
| `userType` | String | `employee` / `member` / `visitor` |
| `signUpTime` | Number | 报名时间 |
| `signInTime` | Number | 签到时间 |
| `lastMessageTime` | Number | 最近互动时间，可选 |

规则：

- 用稳定键防重；优先手机号、邮箱或系统用户 ID。
- 查询和更新都使用同一张表。

### `lottery_winners`

适用：

- 所有抽奖场景

建议字段：

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `winnerId` | String | 中奖用户唯一标识 |
| `winnerName` | String | 中奖人姓名 |
| `winnerPhone` | String | 手机号，可选 |
| `prizeId` | String | 奖项 ID |
| `prizeName` | String | 奖项名称 |
| `round` | Number | 抽奖轮次 |
| `drawTime` | Number | 开奖时间 |
| `campaignId` | String | 活动 ID，可选 |

规则：

- 先写 `lottery_winners`，再更新前端展示和广播消息。
- 如果需要“一人只能中奖一次”，开奖前先按 `winnerId` 去重校验。

### `survey_responses`

适用：

- 问卷和反馈收集

建议字段：

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `respondentId` | String | 提交人标识 |
| `respondentName` | String | 提交人姓名，可选 |
| `answers` | Object | 题目答案集合 |
| `submittedAt` | Number | 提交时间 |
| `campaignId` | String | 活动 ID，可选 |

规则：

- 如果限制每人一次提交，就在提交前按 `respondentId` 查询。
- 如果支持修改，明确保存版本或更新时间。

### 其他自建表

只有在场景明确需要时再新建，例如：

- `lottery_participants`
- `checkin_messages`
- `survey_questions`

保持命名稳定，不要同一份代码里出现多个同义表名。

## 推荐应用配置键

适合放在 `storeAppData()` 中的数据：

| Key | 用途 |
| --- | --- |
| `lottery_prizes` | 奖项配置 |
| `lottery_preferences` | 奖项中奖偏好 |
| `ui_theme` | 当前主题配置 |

规则：

- 配置项用应用数据。
- 业务记录用数据库表。
- 不要把高频增长的记录列表长期塞进应用数据键里。

## 生成后自查

至少核对以下事项：

- 已声明表名常量和应用数据键常量
- 读写使用同一命名
- 签到成功后确实写入签到记录
- 抽奖前已加载奖项配置
- 抽奖后确实落库中奖记录
- 问卷提交后确实落库答案
- 页面刷新后核心状态仍能恢复

常见错误：

- 写入 `signed_users`，读取 `campaign_members`
- 写入 `winners`，读取 `lottery_winners`
- 只改内存中的 `prizes` 数组，没有持久化
- 只在消息通知里更新 UI，没有回源查询或落库
