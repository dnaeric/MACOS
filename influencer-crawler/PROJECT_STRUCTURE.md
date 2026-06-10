# 项目文件说明

## 目录结构

```
influencer-crawler/
├── README.md              # 项目文档
├── requirements.txt       # Python 依赖
├── config.yaml           # 配置文件
├── main.py              # 应用入口
├── install.sh           # 安装脚本
│
├── core/                # 核心模块
│   ├── crawler/        # 爬虫模块
│   │   ├── base.py     # 基础爬虫类
│   │   ├── douyin.py   # 抖音爬虫
│   │   ├── kuaishou.py # 快手爬虫
│   │   ├── xiaohongshu.py # 小红书爬虫
│   │   └── bilibili.py # B站爬虫
│   │
│   ├── filter/        # 过滤模块
│   │   └── filter.py  # 过滤引擎
│   │
│   ├── messaging/     # 消息模块
│   │   └── queue.py   # 消息队列
│   │
│   └── db/           # 数据库模块
│       └── models.py # 数据模型
│
├── ui/              # UI 模块（PyQt6）
│   ├── main_window.py # 主窗口
│   ├── tabs/         # 标签页
│   └── dialogs/      # 对话框
│
├── utils/           # 工具模块
│   ├── logger.py    # 日志工具
│   ├── proxy.py     # 代理管理
│   └── helpers.py   # 辅助函数
│
├── tests/           # 测试模块
│   ├── test_crawler.py
│   └── test_filter.py
│
└── data/            # 数据目录（自动创建）
    └── influencers.db # SQLite 数据库
```

## 核心模块说明

### core/crawler/
网红数据采集模块
- **base.py**: 所有爬虫的基类，定义了通用接口
- **douyin.py**: 抖音爬虫实现
- **kuaishou.py**: 快手爬虫实现
- **xiaohongshu.py**: 小红书爬虫实现
- **bilibili.py**: B站爬虫实现

### core/filter/
数据过滤和筛选模块
- **filter.py**: 高级过滤引擎，支持多条件组合过滤

### core/messaging/
消息发送模块
- **queue.py**: 消息队列管理，支持批量发送和延迟控制

### core/db/
数据库模块
- **models.py**: SQLAlchemy 数据模型，定义了数据表结构

### ui/
用户界面模块（基于 PyQt6）
- **main_window.py**: 应用主窗口
- **tabs/**: 各个功能标签页
- **dialogs/**: 各种对话框

### utils/
工具函数模块
- **logger.py**: 日志配置和管理
- **proxy.py**: 代理池管理
- **helpers.py**: 各种辅助函数

## 配置文件说明

编辑 `config.yaml` 配置应用：

```yaml
# 平台账号
platforms:
  douyin:
    username: "你的抖音用户名"
    password: "你的抖音密码"

# 数据库
database:
  type: "sqlite"
  sqlite_path: "./data/influencers.db"

# 爬虫设置
crawler:
  timeout: 30
  retry_times: 3

# 私信设置
messaging:
  max_per_day: 50
  delay_between_messages: 60
```

## 使用流程

1. **安装依赖**
   ```bash
   ./install.sh
   ```

2. **配置账号**
   - 编辑 `config.yaml`
   - 输入各平台账号信息

3. **运行应用**
   ```bash
   python main.py
   ```

4. **采集数据**
   - 在 UI 中选择平台和搜索条件
   - 点击"开始采集"

5. **过滤数据**
   - 应用筛选条件
   - 选择目标网红

6. **发送私信**
   - 选择消息模板
   - 设置发送参数
   - 启动发送

## 开发指南

### 添加新平台

1. 在 `core/crawler/` 创建新文件（如 `new_platform.py`）
2. 继承 `BaseCrawler` 类
3. 实现必要方法：
   - `login()`: 登录逻辑
   - `search()`: 搜索逻辑
   - `get_influencer_info()`: 获取用户信息
   - `get_influencer_posts()`: 获取用户发布内容

### 扩展筛选条件

编辑 `core/filter/filter.py`，在 `FilterEngine` 类中添加新的过滤逻辑。

### 自定义消息模板

在 `core/messaging/queue.py` 的 `MessageTemplate` 类中添加新模板。

## 常见问题

### Q: 如何添加新的平台支持？
A: 参考"开发指南"部分的"添加新平台"

### Q: 如何修改消息模板？
A: 编辑 `core/messaging/queue.py` 或在 UI 中创建新模板

### Q: 如何导出数据？
A: 在数据管理页面点击"导出"按钮，选择格式和路径

## 许可证

MIT License
