---
name: microapp-development
description: 营销通微应用开发技能。用于设计、生成、调试或重构营销通微应用（签到、签到抽奖、抽奖、问卷等），以及处理 FsYxtMicroApp SDK、ES5 单文件 HTML、CTA 接入、双端联动、数据表设计、持久化和大屏/移动端 UI 规范相关任务。
---

# Microapp Development

## Overview

- 先确认需求，再生成代码。
- 默认输出单个 HTML 文件；开发态引入 Mock SDK，生产态移除。
- 严格使用 ES5；不要生成浏览器直注执行不了的 ES6+ 语法。
- 先设计持久化结构，再写交互逻辑。
- 把关键业务状态持久化；不要只保存在内存数组里。

## Execute Workflow

1. 确认需求。
   - 询问 `appType`、`platform`、`designStyle`、数据存储方式，以及场景特有信息。
   - 对签到类默认推荐双端；对问卷和其他场景按需求选择端形态。
   - 不要把 `ea`、`appId`、`marketingEventId` 这类 URL/业务参数当成前置必问项；优先从运行环境读取。
   - 需要详细提问清单时，读 [workflow.md](references/workflow.md)。
2. 先做方案，再写代码。
   - 确定使用 `db.*`、`object.*` 还是 `storeAppData()/getAppData()`。
   - 明确哪些数据是表记录，哪些是应用配置，哪些只是临时 UI 状态。
   - 对签到/抽奖双端场景，先定义移动端、PC 端和消息同步职责。
3. 生成完整实现。
   - 输出完整 HTML，不要只给片段。
   - 加入平台判断、SDK 可用性保护、错误处理和埋点。
   - 根据所选风格完成视觉实现，不要生成廉价模板。
4. 生成后自查。
   - 检查 ES5 合规、关键流程闭环、持久化完整性、API 调用正确性、UI 质量和移动端降级。

## Non-Negotiables

- 不要跳过需求确认直接写代码。
- 不要用 `const`、`let`、箭头函数、模板字符串、`class`、`async/await`、`forEach`、可选链等 ES6+ 语法。
- 不要把奖项配置、中奖结果、签到结果、问卷提交等关键数据只放在内存变量中。
- 不要显式要求用户提供本可从 URL 或业务参数读取的信息，除非当前方案确实无法推断。
- 不要把 PC 端大屏和移动端逻辑拆成互不通信的孤岛；需要联动时使用广播、消息监听或轮询机制。

## Reference Map

- 读 [workflow.md](references/workflow.md)：按应用类型确认需求、选择默认能力、收敛问题清单。
- 读 [sdk-api.md](references/sdk-api.md)：查 ES5 约束、SDK 接入方式和常用 API。
- 读 [data-models.md](references/data-models.md)：设计自建表、应用配置键和持久化检查项。
- 读 [ui-design.md](references/ui-design.md)：落实视觉基线、性能约束和四种风格方向。
- 读 [example-architecture.md](references/example-architecture.md)：参考签到抽奖双端联动的职责划分与事件流。

## Output Contract

- 默认提供：
  - 完整 HTML 代码
  - 简短使用说明
  - 需要用户补充的配置项或上线注意事项
- 如果用户让你修复现有微应用：
  - 先指出缺失的持久化、平台分支或 API 约束，再直接补齐实现。
- 如果用户只问方案：
  - 仍然给出建议的数据结构、端职责和 API 选型，不要只给泛泛描述。
