# PC_Time_Tracker_windows
轻量开源的本地电脑时间追踪工具，核心亮点是支持按测试（5 分钟）、小时、每日三种自定义频率，自动将应用 / 窗口使用时长报告发送至个人邮箱；所有数据本地 SQLite 存储无第三方上传，可自定义工作 / 娱乐等使用分类规则，工具轻量静默运行且支持开机自启，告别商业监控软件隐私风险，清晰量化电脑时间消耗。

## 使用说明

## 文件结构

```
pc_monitor/
├── config.py        ← 所有配置（邮件、模式、分类规则）
├── db.py            ← SQLite 存储
├── collector.py     ← 数据采集（进程/窗口/键鼠）
├── reporter.py      ← 报告生成 + 邮件发送
├── main.py          ← 入口，启动采集 + 调度
└── requirements.txt
```

## 快速开始

### 1. 安装依赖

```bash
pip install psutil pywin32 pynput
```

### 2. 配置邮件（config.py）

- `sender_password` 填 **QQ邮箱授权码**（非登录密码）
  - 获取路径：QQ邮箱 → 设置 → 账户 → POP3/IMAP → 生成授权码

### 3. 切换发送模式（config.py）

```python
"send_mode": "test"    # 每 5 分钟发一次（调试）
"send_mode": "hourly"  # 每小时整点发一次
"send_mode": "daily"   # 每天 22:00 发一次（配合 daily_send_time）
```

### 4. 启动

```bash
cd pc_monitor
python main.py
```

## 分类规则自定义

在 `config.py` 的 `categories` 字典里，按类别添加**进程名**或**窗口标题关键词**（不区分大小写）：

```python
"工作": ["code", "excel", "notion", "你的公司系统名"],
"娱乐": ["bilibili", "steam", "你常玩的游戏进程名"],
```

## 三阶段使用路线

| 阶段     | send_mode | 目的                                  |
| -------- | --------- | ------------------------------------- |
| 调试期   | `test`    | 5分钟内收到邮件，验证采集和发送都正常 |
| 观察期   | `hourly`  | 每小时一份，确认分类准确              |
| 正式使用 | `daily`   | 每天一份，看全天累积数据              |

## 后台静默运行（开机自启）

1. 在开始菜单搜索"任务计划程序"
2. 创建基本任务 → 触发器选"登录时" → 操作选"启动程序"
3. 程序填 `pythonw.exe`（无黑窗口），参数填 `main.py` 的完整路径

或者简单用 `pythonw.exe` 直接运行：

```python
pythonw main.py
```
