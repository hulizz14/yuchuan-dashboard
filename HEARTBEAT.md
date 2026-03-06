# HEARTBEAT.md - 定时任务配置 v2.0

> 重构版：增加会话触发检查和一次性提醒支持

---

## 🔔 提醒机制说明

本文件包含三种提醒方式：
1. **Cron定时任务**: 固定周期执行（如每天8点日报）
2. **会话触发检查**: 每次对话开始时主动检查 REMINDER.md
3. **事件触发**: 状态变更时触发（由代码逻辑控制）

---

## ⏰ 周期性任务 (Cron)

# 每天定时提醒任务（8:30/14:30/18:00）
- task: 每日重要事项提醒-上午
  schedule: "30 8 * * *"
  action: |
    1. 读取所有部门2026年工作计划看板提取今日及近期关键节点
    2. 检查资产看板（财富中心、天屿花城）预警指标
    3. 汇总今日重要事项：
       - 工程部：3月10日燃气方案确定（1#10#楼）、3月14日围墙门头供应商、3/22-3/28 5#6#楼封顶
       - 成本部：33项结算计划进度、3/30节点风险
       - 开发部：6/10总证办理里程碑
       - 财务部：3月融资回款、5/31土增税清算
       - 法务部：6157万+债权追收、3/15-3/30关键诉讼
    4. 生成预警事项清单（红色预警）
    5. 通过飞书发送上午提醒（8:30）给老大
  enabled: true
  recipient: ou_27f0fe412eed068ebd912269bd996fa7

- task: 每日重要事项提醒-中午
  schedule: "30 14 * * *"
  action: |
    1. 检查上午任务完成情况
    2. 提醒下午关键会议/截止事项：
       - 工程部：本周任务进展跟踪
       - 物业公司：三公司费用收缴进度
       - 成本部：结算审核待办
       - 开发部：证件办理进度
    3. 更新预警状态（黄色/橙色预警）
    4. 通过飞书发送中午提醒（14:30）给老大
  enabled: true
  recipient: ou_27f0fe412ued068ebd912269bd996fa7

- task: 每日重要事项提醒-晚上
  schedule: "0 18 * * *"
  action: |
    1. 汇总全天工作完成情况
    2. 检查明日待办事项：
       - 工程部：明日施工计划、安全检查
       - 财务部：回款核对、资金计划
       - 法务部：合同审查、诉讼进展
       - 人力行政：招聘进度、费用预算执行
    3. 生成明日重点提醒清单
    4. 统计今日预警闭环情况
    5. 通过飞书发送晚上提醒（18:00）给老大
  enabled: true
  recipient: ou_27f0fe412eed068ebd912269bd996fa7

# 自我改进代理 - 每日反思
- task: 每日反思与优化
  schedule: "0 23 * * *"
  action: |
    1. 读取当日 memory/YYYY-MM-DD.md 工作记录
    2. 分析今日任务完成情况：
       - 完成任务数统计
       - 用户反馈和修改次数
       - 响应时间和准确率评估
    3. 识别改进机会：
       - 重复出现的问题
       - 用户偏好变化
       - 流程优化空间
    4. 更新 MEMORY.md 用户画像
    5. 生成明日优化目标
    6. 记录到 SELF_IMPROVING.md 反思日志
  enabled: true

# 自我改进代理 - 周度总结
- task: 周度工作总结与优化
  schedule: "0 22 * * 0"
  action: |
    1. 汇总本周所有工作记录 (memory/)
    2. 生成周度数据报告：
       - 对话次数统计
       - 任务完成率分析
       - 技能使用频率排名
    3. 识别高频任务模式
    4. 优化下周工作流程
    5. 生成改进建议并通知用户
    6. 更新 SELF_IMPROVING.md 周度总结
  enabled: true

# 自我改进代理 - 月度进化
- task: 月度能力评估与规划
  schedule: "0 21 28-31 * *"
  action: |
    1. 深度分析月度工作数据
    2. 评估技能使用效果和价值
    3. 识别能力缺口和学习方向
    4. 规划下月技能提升计划
    5. 更新 AGENTS.md 能力档案
    6. 生成分月度进化报告
  enabled: true
  condition: "is_last_day_of_month"

# 每天早上8点汇报所有学习情况和学习进度
- task: 每日学习进度汇报
  schedule: "0 8 * * *"
  action: |
    1. 读取MEMORY.md获取用户信息和进行中的任务
    2. 检查昨日工作记录 (memory/YYYY-MM-DD.md)
    3. 汇总所有学习进展：
       - 技术学习（新skills、能力提升等）
       - 项目进展（禹川集团资产看板等）
       - 能力提升（数据分析、预警系统等）
    4. 生成所有已安装skills的汇总评价：
       - 按类别统计skills数量
       - 评估每个skill的使用频率和价值
       - 检查是否有重复/冗余的skills
       - 识别缺失的能力领域
    5. 给出skills优化建议：
       - 推荐卸载低价值skills
       - 推荐安装的新skills（基于能力缺口）
       - 提出skills整合方案
    6. 总结昨日完成的工作
    7. 汇报今日计划
    8. 通过飞书发送完整进度报告给老曾
  enabled: true
  recipient: ou_27f0fe412eed068ebd912269bd996fa7

# 每天自动创建/更新工作记录
- task: 自动记录工作日志
  schedule: "0 23 * * *"
  action: |
    1. 将learning/工作进展记录.md复制到memory/YYYY-MM-DD.md
    2. 确保每日工作被完整记录
  enabled: true

# 技能提升定时任务（自主能力提升）
- task: 每10分钟安装测试新技能
  schedule: "*/10 * * * *"  # 每10分钟执行一次
  action: |
    1. 使用clawhub搜索可用技能（按评分/下载量排序）
    2. 选择1个未安装的高价值技能
    3. 检查技能安全性（作者/评分/是否需要上传数据）
    4. 安装技能到本地
    5. 运行技能测试验证功能正常
    6. 记录安装结果到skills_install_log.md
    7. 如安装失败记录失败原因
  enabled: true
  condition: "no_user_task_assigned"  # 仅当用户未分配任务时执行

- task: 每小时汇报技能安装成果
  schedule: "0 * * * *"  # 每小时整点执行
  action: |
    1. 统计过去1小时安装的skills
    2. 测试每个skill的核心功能
    3. 生成测试报告（成功/失败/功能验证）
    4. 汇总已安装skills总数
    5. 通过飞书发送1小时成果汇报
  enabled: true
  condition: "no_user_task_assigned"

# 安全系统定时任务
- task: 自动备份关键文件
  schedule: "0 2 * * *"
  action: |
    1. 备份 MEMORY.md、HEARTBEAT.md 等关键文件
    2. 备份 memory/ 和 learning/ 目录
    3. 生成备份清单 manifest
    4. 记录审计日志
  enabled: true
  script: "python3 ~/.openclaw/workspace/security/auto_backup.py"

- task: 清理旧备份
  schedule: "0 3 * * *"
  action: |
    1. 清理7天前的旧备份文件
    2. 保留最近7天的备份
    3. 释放存储空间
  enabled: true
  script: "python3 -c 'from security.auto_backup import AutoBackup; AutoBackup().cleanup_old_backups(keep_days=7)'"

- task: 安全状态检查
  schedule: "0 */6 * * *"  # 每6小时检查一次
  action: |
    1. 检查审计日志是否有异常
    2. 验证命令白名单状态
    3. 确认备份完整性
    4. 如有异常立即通知
  enabled: true

# 会话触发检查（新增）
- task: 提醒事项检查
  trigger: on_session_start  # 每次会话开始时触发
  action: |
    1. 读取 REMINDER.md 获取所有待办提醒
    2. 检查是否有 overdue 或即将到期的提醒
    3. 如有未处理提醒，立即告知用户
    4. 更新提醒状态
  enabled: true

---

## 📝 使用指南

### 创建一次性提醒
在 REMINDER.md 中添加：
```yaml
- reminder:
    id: R001
    type: once
    datetime: 2026-03-01 19:00
    message: 提醒内容
    status: pending
```

### 创建周期性任务
在上面的 Cron 任务区域添加新的 schedule

### 提醒状态流转
pending → triggered → done/cancelled
