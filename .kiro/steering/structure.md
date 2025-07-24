# Project Structure & Organization

## Repository Organization

### Root Directory Structure
```
IDE双空间智能管理插件/
├── .kiro/                  # Kiro AI assistant configuration
│   └── steering/           # AI guidance rules
├── Code/                   # Source code implementation
├── Doc/                    # Project documentation (Chinese)
│   ├── PRD文档             # Product Requirements Document
│   ├── UI设计文档          # UI Design Specifications
│   ├── 可行性审查报告      # Feasibility Analysis Report
│   └── 技术设计文档        # Technical Design Document
└── README.md              # Project overview
```

### 详细的源代码目录结构 (Detailed Source Code Structure)
```
Code/
├── src/                           # TypeScript 源代码根目录
│   ├── extension.ts              # VSCode 扩展主入口点 (必须)
│   │
│   ├── core/                     # 核心业务逻辑层 (不依赖VSCode API)
│   │   ├── association/          # 关联关系管理模块
│   │   │   ├── AssociationManager.ts      # 关联关系管理器
│   │   │   ├── AssociationValidator.ts    # 关联关系验证器
│   │   │   ├── AssociationAnalyzer.ts     # 关联关系分析器
│   │   │   └── index.ts                   # 模块导出文件
│   │   ├── synchronization/      # 同步引擎模块
│   │   │   ├── SyncCoordinator.ts         # 同步协调器
│   │   │   ├── SyncStrategy.ts            # 同步策略接口
│   │   │   ├── RealTimeSyncStrategy.ts    # 实时同步策略
│   │   │   ├── BatchSyncStrategy.ts       # 批量同步策略
│   │   │   └── index.ts
│   │   ├── configuration/        # 配置管理模块
│   │   │   ├── ConfigManager.ts           # 配置管理器
│   │   │   ├── ConfigValidator.ts         # 配置验证器
│   │   │   ├── DefaultConfig.ts           # 默认配置
│   │   │   └── index.ts
│   │   └── index.ts              # 核心模块总导出
│   │
│   ├── providers/                # VSCode UI 提供器层
│   │   ├── webview/              # WebView 提供器
│   │   │   ├── DualSpaceProvider.ts       # 双空间主视图
│   │   │   ├── SettingsProvider.ts        # 设置页面提供器
│   │   │   └── index.ts
│   │   ├── tree/                 # 树视图提供器
│   │   │   ├── AssociationTreeProvider.ts # 关联关系树
│   │   │   ├── FileExplorerProvider.ts    # 文件浏览器
│   │   │   └── index.ts
│   │   ├── sidebar/              # 侧边栏提供器
│   │   │   ├── AIAssistantProvider.ts     # AI助手侧边栏
│   │   │   ├── SyncStatusProvider.ts      # 同步状态面板
│   │   │   └── index.ts
│   │   └── index.ts
│   │
│   ├── services/                 # 外部服务集成层
│   │   ├── ai-services/          # AI 服务模块
│   │   │   ├── AIServiceManager.ts        # AI服务管理器
│   │   │   ├── providers/                 # AI提供商实现
│   │   │   │   ├── OpenAIProvider.ts      # OpenAI 提供商
│   │   │   │   ├── ClaudeProvider.ts      # Claude 提供商
│   │   │   │   ├── LocalModelProvider.ts  # 本地模型提供商
│   │   │   │   └── index.ts
│   │   │   ├── AIResponseCache.ts         # AI响应缓存
│   │   │   └── index.ts
│   │   ├── file-services/        # 文件服务模块
│   │   │   ├── FileWatcher.ts             # 文件监听器
│   │   │   ├── FileAnalyzer.ts            # 文件分析器
│   │   │   ├── DocumentProcessor.ts       # 文档处理器
│   │   │   └── index.ts
│   │   ├── analysis-services/    # 代码分析服务
│   │   │   ├── CodeStructureAnalyzer.ts   # 代码结构分析
│   │   │   ├── LanguageAnalyzers/         # 语言特定分析器
│   │   │   │   ├── TypeScriptAnalyzer.ts  # TypeScript分析器
│   │   │   │   ├── JavaScriptAnalyzer.ts  # JavaScript分析器
│   │   │   │   ├── PythonAnalyzer.ts      # Python分析器
│   │   │   │   └── index.ts
│   │   │   └── index.ts
│   │   └── index.ts
│   │
│   ├── utils/                    # 工具函数层 (纯函数)
│   │   ├── file-utils/           # 文件操作工具
│   │   │   ├── FileOperations.ts          # 基础文件操作
│   │   │   ├── PathUtils.ts               # 路径处理工具
│   │   │   ├── FileTypeDetector.ts        # 文件类型检测
│   │   │   └── index.ts
│   │   ├── parsing-utils/        # 解析工具
│   │   │   ├── CodeParser.ts              # 代码解析器
│   │   │   ├── MarkdownParser.ts          # Markdown解析器
│   │   │   ├── ASTUtils.ts                # AST工具函数
│   │   │   └── index.ts
│   │   ├── validation-utils/     # 验证工具
│   │   │   ├── InputValidator.ts          # 输入验证
│   │   │   ├── ConfigValidator.ts         # 配置验证
│   │   │   ├── SchemaValidator.ts         # Schema验证
│   │   │   └── index.ts
│   │   ├── common/               # 通用工具
│   │   │   ├── Logger.ts                  # 日志工具
│   │   │   ├── EventEmitter.ts            # 事件发射器
│   │   │   ├── Debouncer.ts               # 防抖工具
│   │   │   ├── Cache.ts                   # 缓存工具
│   │   │   └── index.ts
│   │   └── index.ts
│   │
│   ├── types/                    # TypeScript 类型定义
│   │   ├── core/                 # 核心类型定义
│   │   │   ├── AssociationTypes.ts        # 关联关系类型
│   │   │   ├── SyncTypes.ts               # 同步相关类型
│   │   │   ├── ConfigTypes.ts             # 配置类型
│   │   │   └── index.ts
│   │   ├── services/             # 服务类型定义
│   │   │   ├── AIServiceTypes.ts          # AI服务类型
│   │   │   ├── FileServiceTypes.ts        # 文件服务类型
│   │   │   ├── AnalysisTypes.ts           # 分析服务类型
│   │   │   └── index.ts
│   │   ├── ui/                   # UI相关类型
│   │   │   ├── WebviewTypes.ts            # WebView类型
│   │   │   ├── TreeViewTypes.ts           # 树视图类型
│   │   │   ├── CommandTypes.ts            # 命令类型
│   │   │   └── index.ts
│   │   ├── common/               # 通用类型
│   │   │   ├── CommonTypes.ts             # 通用类型定义
│   │   │   ├── ErrorTypes.ts              # 错误类型
│   │   │   ├── EventTypes.ts              # 事件类型
│   │   │   └── index.ts
│   │   └── index.ts              # 所有类型的总导出
│   │
│   ├── commands/                 # VSCode 命令处理
│   │   ├── association/          # 关联相关命令
│   │   │   ├── CreateAssociationCommand.ts
│   │   │   ├── DeleteAssociationCommand.ts
│   │   │   ├── EditAssociationCommand.ts
│   │   │   └── index.ts
│   │   ├── synchronization/      # 同步相关命令
│   │   │   ├── SyncAllCommand.ts
│   │   │   ├── SyncFileCommand.ts
│   │   │   ├── ResolveSyncConflictCommand.ts
│   │   │   └── index.ts
│   │   ├── ai/                   # AI相关命令
│   │   │   ├── GenerateDocCommand.ts
│   │   │   ├── AnalyzeCodeCommand.ts
│   │   │   ├── OptimizeDocCommand.ts
│   │   │   └── index.ts
│   │   └── index.ts
│   │
│   ├── constants/                # 常量定义
│   │   ├── AppConstants.ts       # 应用常量
│   │   ├── ConfigConstants.ts    # 配置常量
│   │   ├── UIConstants.ts        # UI常量
│   │   ├── APIConstants.ts       # API常量
│   │   └── index.ts
│   │
│   └── index.ts                  # 主模块导出文件
│
├── media/                        # Web 资源文件
│   ├── webview/                  # WebView 资源
│   │   ├── dual-space/           # 双空间视图资源
│   │   │   ├── main.js           # 主要逻辑
│   │   │   ├── style.css         # 样式文件
│   │   │   ├── components/       # UI组件
│   │   │   │   ├── CodeEditor.js
│   │   │   │   ├── DocEditor.js
│   │   │   │   ├── AssociationPanel.js
│   │   │   │   └── StatusBar.js
│   │   │   └── utils/            # 前端工具函数
│   │   │       ├── dom-utils.js
│   │   │       ├── event-utils.js
│   │   │       └── api-client.js
│   │   ├── ai-assistant/         # AI助手视图资源
│   │   │   ├── assistant.js
│   │   │   ├── assistant.css
│   │   │   └── components/
│   │   └── settings/             # 设置页面资源
│   │       ├── settings.js
│   │       ├── settings.css
│   │       └── components/
│   ├── icons/                    # 图标资源
│   │   ├── light/                # 浅色主题图标
│   │   ├── dark/                 # 深色主题图标
│   │   └── common/               # 通用图标
│   └── assets/                   # 其他资源文件
│       ├── images/
│       ├── fonts/
│       └── templates/
│
├── test/                         # 测试文件
│   ├── unit/                     # 单元测试
│   │   ├── core/                 # 核心模块测试
│   │   │   ├── association/
│   │   │   │   ├── AssociationManager.test.ts
│   │   │   │   └── AssociationValidator.test.ts
│   │   │   ├── synchronization/
│   │   │   └── configuration/
│   │   ├── services/             # 服务模块测试
│   │   │   ├── ai-services/
│   │   │   ├── file-services/
│   │   │   └── analysis-services/
│   │   └── utils/                # 工具函数测试
│   ├── integration/              # 集成测试
│   │   ├── extension-integration.test.ts
│   │   ├── ai-service-integration.test.ts
│   │   └── file-sync-integration.test.ts
│   ├── e2e/                      # 端到端测试
│   │   ├── user-workflows/
│   │   │   ├── create-association.e2e.ts
│   │   │   ├── sync-documents.e2e.ts
│   │   │   └── ai-generation.e2e.ts
│   │   └── performance/
│   ├── fixtures/                 # 测试数据和模拟
│   │   ├── sample-projects/      # 示例项目
│   │   ├── mock-responses/       # 模拟响应
│   │   └── test-configs/         # 测试配置
│   └── helpers/                  # 测试辅助工具
│       ├── test-utils.ts
│       ├── mock-factories.ts
│       └── assertion-helpers.ts
│
├── docs/                         # 项目内部技术文档
│   ├── api/                      # API文档
│   ├── architecture/             # 架构文档
│   ├── development/              # 开发指南
│   └── deployment/               # 部署文档
│
├── scripts/                      # 构建和部署脚本
│   ├── build.js                  # 构建脚本
│   ├── test.js                   # 测试脚本
│   ├── package.js                # 打包脚本
│   └── deploy.js                 # 部署脚本
│
├── config/                       # 配置文件
│   ├── webpack.config.js         # Webpack配置
│   ├── tsconfig.json             # TypeScript配置
│   ├── jest.config.js            # Jest测试配置
│   ├── eslint.config.js          # ESLint配置
│   └── prettier.config.js        # Prettier配置
│
├── package.json                  # 扩展清单和依赖
├── package-lock.json             # 依赖锁定文件
├── .vscodeignore                 # VSCode打包忽略文件
├── .gitignore                    # Git忽略文件
├── .eslintrc.js                  # ESLint配置
├── .prettierrc                   # Prettier配置
└── README.md                     # 项目说明文档
```

### 详细的文档目录结构 (Detailed Documentation Structure)
```
Doc/                              # 项目文档根目录 (中文为主)
├── 01-需求文档/                   # 需求相关文档
│   ├── PRD-产品需求文档-v1.md      # 产品需求文档
│   ├── 用户故事-v1.md             # 用户故事集合
│   ├── 功能规格说明-v1.md          # 功能详细规格
│   ├── 非功能需求-v1.md           # 性能、安全等需求
│   └── 需求变更记录-v1.md          # 需求变更历史
│
├── 02-设计文档/                   # 设计相关文档
│   ├── UI设计规范-v1.md           # UI设计规范和标准
│   ├── 交互设计文档-v1.md          # 用户交互设计
│   ├── 视觉设计指南-v1.md          # 视觉设计规范
│   ├── 原型设计-v1.md             # 原型设计说明
│   └── 设计资源/                  # 设计资源文件
│       ├── mockups/              # 界面设计稿
│       ├── icons/                # 图标资源
│       └── style-guide/          # 样式指南
│
├── 03-技术文档/                   # 技术相关文档
│   ├── 技术架构设计-v1.md          # 整体技术架构
│   ├── 数据库设计-v1.md           # 数据库结构设计
│   ├── API设计文档-v1.md          # API接口设计
│   ├── 安全设计文档-v1.md          # 安全架构设计
│   ├── 性能优化方案-v1.md          # 性能优化策略
│   └── 技术选型报告-v1.md          # 技术栈选择说明
│
├── 04-开发文档/                   # 开发相关文档
│   ├── 开发环境搭建-v1.md          # 开发环境配置
│   ├── 编码规范-v1.md             # 代码编写规范
│   ├── 测试策略-v1.md             # 测试计划和策略
│   ├── 部署指南-v1.md             # 部署流程说明
│   ├── 故障排除指南-v1.md          # 常见问题解决
│   └── 开发工具配置-v1.md          # 开发工具设置
│
├── 05-项目管理/                   # 项目管理文档
│   ├── 项目计划-v1.md             # 项目时间计划
│   ├── 里程碑规划-v1.md           # 关键里程碑
│   ├── 风险评估报告-v1.md          # 项目风险分析
│   ├── 可行性审查报告-v1.md        # 可行性分析
│   ├── 资源分配计划-v1.md          # 人力资源规划
│   └── 项目进度报告/              # 定期进度报告
│       ├── 周报-2024-W01.md
│       ├── 月报-2024-01.md
│       └── 季度报告-2024-Q1.md
│
├── 06-测试文档/                   # 测试相关文档
│   ├── 测试计划-v1.md             # 整体测试计划
│   ├── 测试用例-v1.md             # 详细测试用例
│   ├── 自动化测试方案-v1.md        # 自动化测试策略
│   ├── 性能测试报告-v1.md          # 性能测试结果
│   ├── 安全测试报告-v1.md          # 安全测试结果
│   └── 测试环境配置-v1.md          # 测试环境说明
│
├── 07-用户文档/                   # 用户使用文档
│   ├── 用户手册-v1.md             # 完整用户手册
│   ├── 快速开始指南-v1.md          # 快速上手指南
│   ├── 功能使用教程-v1.md          # 功能详细教程
│   ├── 常见问题FAQ-v1.md          # 常见问题解答
│   ├── 故障排除指南-v1.md          # 用户故障处理
│   └── 视频教程/                  # 视频教程资源
│       ├── 基础使用教程.mp4
│       ├── 高级功能演示.mp4
│       └── 最佳实践分享.mp4
│
├── 08-运营文档/                   # 运营相关文档
│   ├── 产品发布计划-v1.md          # 产品发布策略
│   ├── 市场推广方案-v1.md          # 市场营销计划
│   ├── 用户反馈收集-v1.md          # 用户反馈机制
│   ├── 数据分析报告-v1.md          # 用户数据分析
│   └── 商业化策略-v1.md           # 商业模式规划
│
├── 09-法务文档/                   # 法务相关文档
│   ├── 软件许可协议-v1.md          # 软件使用许可
│   ├── 隐私政策-v1.md             # 用户隐私保护
│   ├── 服务条款-v1.md             # 服务使用条款
│   └── 知识产权声明-v1.md          # 知识产权说明
│
├── 10-版本文档/                   # 版本相关文档
│   ├── 版本发布说明/              # 各版本发布说明
│   │   ├── v1.0.0-发布说明.md
│   │   ├── v1.1.0-发布说明.md
│   │   └── v2.0.0-发布说明.md
│   ├── 变更日志-CHANGELOG.md      # 详细变更记录
│   ├── 升级指南-v1.md             # 版本升级指导
│   └── 兼容性说明-v1.md           # 版本兼容性
│
├── 11-国际化文档/                 # 多语言文档
│   ├── en/                       # 英文文档
│   │   ├── README.md
│   │   ├── USER_MANUAL.md
│   │   ├── API_REFERENCE.md
│   │   └── CONTRIBUTING.md
│   ├── ja/                       # 日文文档
│   └── ko/                       # 韩文文档
│
├── 12-会议记录/                   # 会议记录文档
│   ├── 需求评审会议/              # 需求评审记录
│   ├── 技术评审会议/              # 技术评审记录
│   ├── 项目例会记录/              # 定期例会记录
│   └── 决策会议记录/              # 重要决策记录
│
├── templates/                    # 文档模板
│   ├── 需求文档模板.md
│   ├── 技术设计模板.md
│   ├── 测试报告模板.md
│   ├── 会议记录模板.md
│   └── 发布说明模板.md
│
├── assets/                       # 文档资源文件
│   ├── images/                   # 图片资源
│   │   ├── architecture/         # 架构图
│   │   ├── ui-mockups/          # UI设计图
│   │   ├── flowcharts/          # 流程图
│   │   └── screenshots/         # 截图
│   ├── diagrams/                 # 图表文件
│   │   ├── architecture.drawio   # 架构图源文件
│   │   ├── database-erd.drawio   # 数据库关系图
│   │   └── user-flow.drawio      # 用户流程图
│   └── videos/                   # 视频资源
│
├── archive/                      # 归档文档
│   ├── deprecated/               # 已废弃文档
│   ├── old-versions/             # 旧版本文档
│   └── historical/               # 历史文档
│
├── README.md                     # 文档目录说明
├── 文档维护指南.md                # 文档维护规范
└── 文档审核流程.md                # 文档审核流程

# 配置目录结构 (.dual-space/)
.dual-space/
├── config/                       # 配置文件目录
│   ├── project-config.json       # 项目级配置
│   ├── user-preferences.json     # 用户偏好设置
│   ├── ai-provider-config.json   # AI服务提供商配置
│   ├── sync-strategy-config.json # 同步策略配置
│   └── ui-layout-config.json     # UI布局配置
├── data/                         # 数据存储目录
│   ├── associations.db           # 关联关系数据库
│   ├── sync-history.db           # 同步历史数据库
│   ├── user-analytics.db         # 用户行为分析数据
│   └── backup/                   # 数据备份目录
├── cache/                        # 缓存目录
│   ├── ai-responses/             # AI响应缓存
│   │   ├── openai/               # OpenAI响应缓存
│   │   ├── claude/               # Claude响应缓存
│   │   └── local/                # 本地模型缓存
│   ├── code-analysis/            # 代码分析缓存
│   │   ├── typescript/           # TypeScript分析缓存
│   │   ├── javascript/           # JavaScript分析缓存
│   │   └── python/               # Python分析缓存
│   ├── file-metadata/            # 文件元数据缓存
│   └── thumbnails/               # 缩略图缓存
├── logs/                         # 日志目录
│   ├── application.log           # 应用日志
│   ├── error.log                 # 错误日志
│   ├── sync.log                  # 同步操作日志
│   ├── ai-service.log            # AI服务调用日志
│   └── performance.log           # 性能监控日志
├── temp/                         # 临时文件目录
│   ├── processing/               # 处理中的临时文件
│   ├── uploads/                  # 上传临时文件
│   └── downloads/                # 下载临时文件
├── plugins/                      # 插件扩展目录
│   ├── custom-analyzers/         # 自定义分析器
│   ├── custom-providers/         # 自定义AI提供商
│   └── custom-themes/            # 自定义主题
├── migrations/                   # 数据迁移脚本
│   ├── v1.0.0-to-v1.1.0.sql
│   ├── v1.1.0-to-v1.2.0.sql
│   └── migration-history.json
├── security/                     # 安全相关文件
│   ├── api-keys.encrypted        # 加密的API密钥
│   ├── certificates/             # 证书文件
│   └── access-tokens.json        # 访问令牌
└── README.md                     # 配置目录说明
```

## 严格的命名规范 (Strict Naming Conventions)

### TypeScript 文件命名
- **类和接口**: PascalCase，必须以功能描述开头
  - ✅ `AssociationManager.ts`, `AIServiceProvider.ts`, `SyncCoordinator.ts`
  - ❌ `manager.ts`, `ai.ts`, `sync.ts`
- **函数和变量**: camelCase，动词开头
  - ✅ `createAssociation()`, `syncContent()`, `validateInput()`
  - ❌ `association()`, `content()`, `input()`
- **常量**: UPPER_SNAKE_CASE，含义明确
  - ✅ `DEFAULT_SYNC_DELAY`, `MAX_ASSOCIATIONS_PER_FILE`, `AI_TIMEOUT_MS`
  - ❌ `DELAY`, `MAX`, `TIMEOUT`
- **枚举**: PascalCase，复数形式
  - ✅ `AssociationTypes`, `SyncStrategies`, `AIProviders`
  - ❌ `Type`, `Strategy`, `Provider`

### 目录和文件结构命名
- **目录名**: kebab-case，功能导向
  - ✅ `ai-services/`, `file-watchers/`, `ui-components/`
  - ❌ `services/`, `watchers/`, `components/`
- **测试文件**: 原文件名 + `.test.ts` 或 `.spec.ts`
  - ✅ `AssociationManager.test.ts`, `AIService.spec.ts`
  - ❌ `test-association.ts`, `ai-test.ts`
- **类型定义文件**: 功能 + `Types.ts`
  - ✅ `AssociationTypes.ts`, `AIServiceTypes.ts`, `ConfigTypes.ts`
  - ❌ `types.ts`, `interfaces.ts`, `models.ts`

### 配置文件命名
- **JSON配置**: kebab-case + 功能描述
  - ✅ `dual-space-config.json`, `ai-provider-settings.json`
  - ❌ `config.json`, `settings.json`
- **环境配置**: `.env.{environment}`
  - ✅ `.env.development`, `.env.production`, `.env.test`
- **JSON属性**: camelCase，描述性
  - ✅ `autoSyncEnabled`, `aiProviderType`, `maxRetryAttempts`
  - ❌ `sync`, `provider`, `retry`

### 数据库命名规范
- **表名**: snake_case，复数形式
  - ✅ `code_associations`, `sync_histories`, `ai_cache_entries`
  - ❌ `association`, `history`, `cache`
- **列名**: snake_case，描述性
  - ✅ `source_file_path`, `target_doc_path`, `last_sync_timestamp`
  - ❌ `source`, `target`, `time`
- **索引名**: `idx_{table}_{column(s)}`
  - ✅ `idx_associations_source_file`, `idx_sync_histories_timestamp`

### 文档文件命名
- **中文用户文档**: 功能描述-版本号.md
  - ✅ `用户手册-v1.md`, `安装指南-v2.md`, `常见问题-v1.md`
  - ❌ `manual.md`, `install.md`, `faq.md`
- **英文技术文档**: UPPER_CASE.md
  - ✅ `API.md`, `CONTRIBUTING.md`, `CHANGELOG.md`, `ARCHITECTURE.md`
  - ❌ `api.md`, `contributing.md`, `changelog.md`
- **规范文档**: 规范类型-规范名称.md
  - ✅ `代码规范-TypeScript.md`, `设计规范-UI组件.md`
  - ❌ `typescript.md`, `ui.md`

## 严格的代码组织原则 (Strict Code Organization Principles)

### 分层架构规范 (Layered Architecture Standards)
1. **扩展层 (Extension Layer)**: 
   - 仅处理VSCode特定的集成和UI逻辑
   - 不包含业务逻辑，只做数据转换和事件处理
   - 文件必须以`Provider.ts`或`Command.ts`结尾

2. **核心业务层 (Core Business Layer)**:
   - 完全独立于VSCode API，可在其他环境运行
   - 包含所有业务逻辑和数据处理
   - 每个模块必须有对应的接口定义
   - 必须有完整的单元测试覆盖

3. **服务层 (Service Layer)**:
   - 处理外部集成（AI、文件系统、网络）
   - 实现依赖注入模式，便于测试和替换
   - 每个服务必须实现标准接口
   - 包含错误处理和重试机制

4. **数据层 (Data Layer)**:
   - 负责数据持久化和缓存
   - 使用Repository模式抽象数据访问
   - 支持数据迁移和版本管理
   - 实现数据验证和完整性检查

### 模块依赖规则 (Module Dependency Rules)
```
扩展层 → 核心业务层 → 服务层 → 数据层
   ↓         ↓         ↓       ↓
工具层 ← 工具层 ← 工具层 ← 工具层
```

**严格禁止的依赖关系**:
- ❌ 核心业务层不得依赖VSCode API
- ❌ 服务层不得直接访问数据层
- ❌ 工具层不得依赖业务逻辑
- ❌ 循环依赖绝对禁止

**推荐的依赖模式**:
- ✅ 使用依赖注入容器管理依赖
- ✅ 通过接口定义模块边界
- ✅ 使用事件总线解耦模块通信
- ✅ 工具函数保持纯函数特性

### 文件组织强制规范 (Mandatory File Organization Rules)

#### 每个目录必须包含的文件
```
任何功能目录/
├── index.ts          # 必须：模块导出文件
├── types.ts          # 必须：类型定义文件
├── constants.ts      # 可选：常量定义
├── README.md         # 必须：模块说明文档
└── __tests__/        # 必须：测试文件目录
    └── index.test.ts
```

#### 文件内容组织规范
```typescript
// 1. 文件头注释（必须）
/**
 * @fileoverview 文件功能描述
 * @author 作者信息
 * @since 版本信息
 */

// 2. 导入语句（按顺序）
// 2.1 Node.js 内置模块
import * as path from 'path';
import * as fs from 'fs';

// 2.2 第三方库
import * as vscode from 'vscode';
import axios from 'axios';

// 2.3 项目内部模块（按层级）
import { CoreModule } from '../core';
import { ServiceModule } from '../services';
import { UtilModule } from '../utils';

// 2.4 类型导入
import type { ConfigType, AssociationType } from '../types';

// 3. 常量定义
const DEFAULT_TIMEOUT = 5000;
const MAX_RETRY_ATTEMPTS = 3;

// 4. 类型定义（如果不在单独的types文件中）
interface LocalInterface {
  // ...
}

// 5. 主要实现代码
export class MainClass {
  // ...
}

// 6. 导出语句
export { MainClass };
export type { LocalInterface };
```

### 错误处理标准化 (Standardized Error Handling)

#### 错误类型定义
```typescript
// 必须使用标准化的错误类型
export enum ErrorCodes {
  // 系统错误 (1000-1999)
  SYSTEM_ERROR = 1000,
  FILE_NOT_FOUND = 1001,
  PERMISSION_DENIED = 1002,
  
  // 业务错误 (2000-2999)
  ASSOCIATION_NOT_FOUND = 2000,
  SYNC_CONFLICT = 2001,
  INVALID_CONFIGURATION = 2002,
  
  // AI服务错误 (3000-3999)
  AI_SERVICE_UNAVAILABLE = 3000,
  AI_QUOTA_EXCEEDED = 3001,
  AI_RESPONSE_INVALID = 3002,
  
  // 用户错误 (4000-4999)
  INVALID_INPUT = 4000,
  UNAUTHORIZED_ACCESS = 4001,
}

// 标准错误类
export class DualSpaceError extends Error {
  constructor(
    public code: ErrorCodes,
    message: string,
    public details?: any,
    public recoverable: boolean = true
  ) {
    super(message);
    this.name = 'DualSpaceError';
  }
}
```

#### 错误处理模式
```typescript
// 服务层错误处理
export class AIService {
  async generateDoc(request: DocRequest): Promise<Result<DocResponse, DualSpaceError>> {
    try {
      const response = await this.callAI(request);
      return Result.ok(response);
    } catch (error) {
      const dualSpaceError = new DualSpaceError(
        ErrorCodes.AI_SERVICE_UNAVAILABLE,
        'AI服务暂时不可用',
        error,
        true // 可恢复错误
      );
      
      // 记录错误日志
      this.logger.error('AI服务调用失败', { error: dualSpaceError, request });
      
      // 尝试降级处理
      if (this.fallbackService) {
        return this.fallbackService.generateDoc(request);
      }
      
      return Result.error(dualSpaceError);
    }
  }
}
```

### 日志记录规范 (Logging Standards)

#### 日志级别定义
```typescript
export enum LogLevel {
  ERROR = 0,    // 错误：系统错误，需要立即处理
  WARN = 1,     // 警告：潜在问题，需要关注
  INFO = 2,     // 信息：重要的业务流程信息
  DEBUG = 3,    // 调试：详细的调试信息
  TRACE = 4,    // 跟踪：最详细的执行跟踪
}
```

#### 日志格式标准
```typescript
// 必须包含的日志字段
interface LogEntry {
  timestamp: string;      // ISO 8601格式时间戳
  level: LogLevel;        // 日志级别
  message: string;        // 日志消息
  module: string;         // 模块名称
  function?: string;      // 函数名称
  userId?: string;        // 用户ID（如果适用）
  sessionId?: string;     // 会话ID
  correlationId?: string; // 关联ID（用于跟踪请求）
  metadata?: any;         // 额外的元数据
  error?: Error;          // 错误对象（如果是错误日志）
}
```

### 性能监控规范 (Performance Monitoring Standards)

#### 性能指标定义
```typescript
export interface PerformanceMetrics {
  // 响应时间指标
  responseTime: {
    ai_generation: number;      // AI生成响应时间
    file_sync: number;          // 文件同步时间
    association_creation: number; // 关联创建时间
  };
  
  // 资源使用指标
  resourceUsage: {
    memory_usage: number;       // 内存使用量(MB)
    cpu_usage: number;          // CPU使用率(%)
    disk_io: number;           // 磁盘IO次数
  };
  
  // 业务指标
  businessMetrics: {
    associations_created: number;  // 创建的关联数
    documents_synced: number;      // 同步的文档数
    ai_requests: number;           // AI请求次数
  };
}
```

#### 性能监控实现
```typescript
export class PerformanceMonitor {
  private metrics: Map<string, number[]> = new Map();
  
  // 记录性能指标
  recordMetric(name: string, value: number, tags?: Record<string, string>): void {
    const key = this.buildMetricKey(name, tags);
    const values = this.metrics.get(key) || [];
    values.push(value);
    
    // 只保留最近1000个数据点
    if (values.length > 1000) {
      values.shift();
    }
    
    this.metrics.set(key, values);
    
    // 检查是否超过阈值
    this.checkThresholds(name, value);
  }
  
  // 性能装饰器
  @performance('ai_generation')
  async generateDocument(request: DocRequest): Promise<DocResponse> {
    // 方法实现
  }
}
```

## Documentation Standards

### Code Documentation
- **JSDoc comments** for all public APIs
- **Inline comments** for complex business logic
- **README files** in each major directory
- **Type annotations** for all function parameters and returns

### User Documentation
- **Bilingual support**: Chinese primary, English secondary
- **Progressive disclosure**: Basic → Advanced → Expert levels
- **Visual aids**: Screenshots, diagrams, and video tutorials
- **Searchable format**: Structured for easy navigation

### Technical Documentation
- **Architecture Decision Records (ADRs)** for major technical choices
- **API documentation** auto-generated from code
- **Deployment guides** for different environments
- **Troubleshooting guides** for common issues

## 严格的开发工作流程 (Strict Development Workflow)

### Git 分支策略 (Git Branch Strategy)
```
main (生产分支)
├── develop (开发集成分支)
│   ├── feature/DUAL-001-association-manager (功能分支)
│   ├── feature/DUAL-002-ai-service-integration (功能分支)
│   ├── feature/DUAL-003-sync-coordinator (功能分支)
│   └── bugfix/DUAL-BUG-001-sync-race-condition (缺陷修复分支)
├── release/v1.0.0 (发布准备分支)
├── release/v1.1.0 (发布准备分支)
└── hotfix/DUAL-HOT-001-critical-sync-issue (热修复分支)
```

#### 分支命名强制规范
- **功能分支**: `feature/DUAL-{JIRA编号}-{简短描述}`
  - ✅ `feature/DUAL-001-association-manager`
  - ❌ `feature/association`, `feat/new-feature`

- **缺陷修复**: `bugfix/DUAL-BUG-{编号}-{简短描述}`
  - ✅ `bugfix/DUAL-BUG-001-sync-race-condition`
  - ❌ `bugfix/sync-fix`, `fix/bug`

- **热修复**: `hotfix/DUAL-HOT-{编号}-{简短描述}`
  - ✅ `hotfix/DUAL-HOT-001-critical-sync-issue`
  - ❌ `hotfix/urgent-fix`

- **发布分支**: `release/v{major}.{minor}.{patch}`
  - ✅ `release/v1.0.0`, `release/v1.2.3`
  - ❌ `release/1.0`, `rel/v1.0.0`

### 提交信息规范 (Commit Message Standards)
```
<type>(<scope>): <subject>

<body>

<footer>
```

#### 提交类型 (Commit Types)
- **feat**: 新功能 (feature)
- **fix**: 缺陷修复 (bug fix)
- **docs**: 文档更新 (documentation)
- **style**: 代码格式调整 (formatting, missing semi colons, etc)
- **refactor**: 代码重构 (refactoring production code)
- **test**: 测试相关 (adding missing tests, refactoring tests)
- **chore**: 构建过程或辅助工具的变动 (updating grunt tasks etc)
- **perf**: 性能优化 (performance improvements)
- **ci**: CI/CD相关 (continuous integration)
- **build**: 构建系统相关 (build system or external dependencies)

#### 作用域 (Scopes)
- **core**: 核心业务逻辑
- **ai**: AI服务相关
- **sync**: 同步功能
- **ui**: 用户界面
- **config**: 配置管理
- **test**: 测试相关
- **docs**: 文档相关
- **build**: 构建相关

#### 提交信息示例
```bash
# 功能提交
feat(ai): add Claude AI provider support

- Implement ClaudeProvider class with API integration
- Add configuration options for Claude API key
- Include fallback mechanism when OpenAI is unavailable
- Add unit tests for Claude provider

Closes DUAL-002

# 缺陷修复提交
fix(sync): resolve race condition in file watching

The file watcher was triggering multiple sync operations
simultaneously, causing data corruption. Added debouncing
mechanism and proper locking to prevent concurrent syncs.

Fixes DUAL-BUG-001

# 文档更新提交
docs(readme): update installation instructions

- Add prerequisites section
- Update VSCode version requirements
- Include troubleshooting section for common issues
- Add screenshots for installation steps
```

### 代码审查流程 (Code Review Process)

#### 审查检查清单 (Review Checklist)
**功能性检查**:
- [ ] 代码实现符合需求规格
- [ ] 所有边界条件都已处理
- [ ] 错误处理机制完整
- [ ] 性能影响已评估

**代码质量检查**:
- [ ] 遵循项目编码规范
- [ ] 函数和类的职责单一
- [ ] 变量和函数命名清晰
- [ ] 代码复杂度在可接受范围内

**测试覆盖检查**:
- [ ] 单元测试覆盖率 ≥ 80%
- [ ] 集成测试覆盖关键路径
- [ ] 测试用例包含正常和异常场景
- [ ] 性能测试通过基准要求

**文档完整性检查**:
- [ ] 公共API有完整的JSDoc注释
- [ ] 复杂逻辑有内联注释说明
- [ ] README文档已更新
- [ ] 变更日志已记录

#### 审查角色和职责
1. **开发者自审 (Self Review)**:
   - 提交前必须自己审查代码
   - 运行所有测试确保通过
   - 检查代码格式和规范
   - 确保提交信息规范

2. **同行审查 (Peer Review)**:
   - 至少一名同级开发者审查
   - 重点关注代码逻辑和实现
   - 检查测试覆盖率和质量
   - 验证功能是否符合需求

3. **技术负责人审查 (Tech Lead Review)**:
   - 架构变更必须经过技术负责人审查
   - 评估对整体系统的影响
   - 确保技术方案的一致性
   - 把控代码质量标准

4. **安全审查 (Security Review)**:
   - 涉及安全的代码必须安全审查
   - 检查输入验证和权限控制
   - 评估潜在的安全风险
   - 确保敏感信息不泄露

### 持续集成流程 (CI/CD Pipeline)

#### 自动化检查流程
```yaml
# .github/workflows/ci.yml
name: Continuous Integration

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]

jobs:
  code-quality:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Lint check
        run: npm run lint
      
      - name: Type check
        run: npm run type-check
      
      - name: Format check
        run: npm run format:check
  
  testing:
    runs-on: ubuntu-latest
    steps:
      - name: Unit tests
        run: npm run test:unit
      
      - name: Integration tests
        run: npm run test:integration
      
      - name: E2E tests
        run: npm run test:e2e
      
      - name: Coverage report
        run: npm run test:coverage
        
      - name: Upload coverage
        uses: codecov/codecov-action@v3
  
  security:
    runs-on: ubuntu-latest
    steps:
      - name: Security audit
        run: npm audit --audit-level high
      
      - name: Dependency check
        run: npm run security:check
  
  performance:
    runs-on: ubuntu-latest
    steps:
      - name: Bundle size check
        run: npm run build:analyze
      
      - name: Performance benchmarks
        run: npm run test:performance
```

#### 质量门禁 (Quality Gates)
**代码合并前必须满足**:
- ✅ 所有自动化测试通过
- ✅ 代码覆盖率 ≥ 80%
- ✅ ESLint检查无错误
- ✅ TypeScript类型检查通过
- ✅ 安全扫描无高危漏洞
- ✅ 性能测试通过基准
- ✅ 至少一名审查者批准

**发布前额外要求**:
- ✅ 所有功能测试通过
- ✅ 用户验收测试完成
- ✅ 性能回归测试通过
- ✅ 安全渗透测试通过
- ✅ 文档更新完整
- ✅ 发布说明准备完毕

## Quality Assurance

### Testing Strategy
- **Unit tests**: 80%+ coverage for core business logic
- **Integration tests**: End-to-end workflow testing
- **Performance tests**: Memory usage and response time benchmarks
- **User acceptance tests**: Real user scenario validation

### Code Quality Gates
- **ESLint**: No linting errors allowed
- **TypeScript**: Strict type checking enabled
- **Prettier**: Consistent code formatting
- **Security**: Dependency vulnerability scanning

### Performance Monitoring
- **Bundle size**: Monitor extension package size
- **Memory usage**: Track memory consumption patterns
- **API usage**: Monitor AI service usage and costs
- **User metrics**: Track feature usage and performance

## Deployment & Distribution

### Build Artifacts
- **Extension package**: `.vsix` file for VSCode Marketplace
- **Source maps**: For debugging production issues
- **Documentation**: Generated API docs and user guides
- **Changelog**: Detailed release notes

### Release Process
1. **Version bump**: Update version in package.json
2. **Changelog**: Document all changes since last release
3. **Testing**: Full regression test suite
4. **Package**: Create extension package
5. **Publish**: Deploy to VSCode Marketplace
6. **Monitor**: Track installation and usage metrics

### Environment Configuration
- **Development**: Local development with hot reload
- **Staging**: Pre-production testing environment
- **Production**: VSCode Marketplace distribution
- **Beta**: Early access for testing new features
#
# 文档维护和更新规范 (Documentation Maintenance Standards)

### 文档版本控制规范
- **版本号格式**: `v{major}.{minor}.{patch}`
  - Major: 重大内容变更或结构调整
  - Minor: 功能增加或重要内容更新
  - Patch: 错误修正或小幅内容调整

- **文档状态标识**:
  - `[草稿]`: 正在编写中的文档
  - `[审核中]`: 等待审核的文档
  - `[已发布]`: 正式发布的文档
  - `[已废弃]`: 不再维护的文档

### 文档更新触发条件
**必须更新文档的情况**:
- ✅ 新功能开发完成
- ✅ API接口变更
- ✅ 配置项增加或修改
- ✅ 用户界面重大变更
- ✅ 安装或部署流程变更
- ✅ 已知问题修复

**文档更新责任人**:
- **功能文档**: 功能开发者负责
- **API文档**: 后端开发者负责
- **用户文档**: 产品经理和开发者共同负责
- **技术文档**: 技术负责人负责

### 文档质量检查标准
**内容质量要求**:
- [ ] 信息准确无误
- [ ] 步骤清晰可操作
- [ ] 示例代码可运行
- [ ] 截图和图表清晰
- [ ] 链接有效可访问

**格式规范要求**:
- [ ] 使用统一的Markdown格式
- [ ] 标题层级结构合理
- [ ] 代码块语法高亮正确
- [ ] 表格格式规范
- [ ] 图片尺寸适中

## 项目维护和演进规范 (Project Maintenance Standards)

### 代码重构规范
**重构触发条件**:
- 代码复杂度超过阈值 (圈复杂度 > 10)
- 代码重复率超过 15%
- 性能指标下降超过 20%
- 技术债务积累过多

**重构流程**:
1. **评估阶段**: 分析重构范围和影响
2. **计划阶段**: 制定详细的重构计划
3. **实施阶段**: 分步骤执行重构
4. **验证阶段**: 确保功能和性能不受影响
5. **文档阶段**: 更新相关技术文档

### 依赖管理规范
**依赖更新策略**:
- **安全更新**: 立即更新
- **主要版本**: 谨慎评估后更新
- **次要版本**: 定期批量更新
- **补丁版本**: 自动更新

**依赖选择原则**:
- 优先选择活跃维护的库
- 避免引入过多小型依赖
- 评估许可证兼容性
- 考虑包大小对性能的影响

### 技术债务管理
**技术债务分类**:
- **设计债务**: 架构设计不合理
- **代码债务**: 代码质量问题
- **测试债务**: 测试覆盖不足
- **文档债务**: 文档缺失或过时

**债务处理优先级**:
1. **高优先级**: 影响系统稳定性和安全性
2. **中优先级**: 影响开发效率和维护性
3. **低优先级**: 代码美观和规范性问题

## 强制执行机制 (Enforcement Mechanisms)

### 自动化检查工具
```json
// package.json scripts
{
  "scripts": {
    "lint": "eslint src/ --ext .ts,.tsx",
    "lint:fix": "eslint src/ --ext .ts,.tsx --fix",
    "type-check": "tsc --noEmit",
    "format": "prettier --write src/",
    "format:check": "prettier --check src/",
    "test:unit": "jest --testPathPattern=unit",
    "test:integration": "jest --testPathPattern=integration",
    "test:e2e": "jest --testPathPattern=e2e",
    "test:coverage": "jest --coverage",
    "security:audit": "npm audit --audit-level high",
    "security:check": "snyk test",
    "build:analyze": "webpack-bundle-analyzer dist/",
    "docs:generate": "typedoc src/",
    "docs:serve": "docsify serve docs/",
    "pre-commit": "lint-staged",
    "pre-push": "npm run test:unit && npm run type-check"
  }
}
```

### Git Hooks 配置
```bash
# .husky/pre-commit
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

# 代码格式检查
npm run format:check

# 代码质量检查
npm run lint

# 类型检查
npm run type-check

# 单元测试
npm run test:unit

# .husky/commit-msg
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

# 提交信息格式检查
npx commitlint --edit $1

# .husky/pre-push
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

# 完整测试套件
npm run test

# 安全检查
npm run security:audit
```

### IDE 配置文件
```json
// .vscode/settings.json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.organizeImports": true
  },
  "typescript.preferences.importModuleSpecifier": "relative",
  "files.exclude": {
    "**/node_modules": true,
    "**/dist": true,
    "**/.dual-space/cache": true
  },
  "search.exclude": {
    "**/node_modules": true,
    "**/dist": true,
    "**/.dual-space": true
  }
}

// .vscode/extensions.json
{
  "recommendations": [
    "ms-vscode.vscode-typescript-next",
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "ms-vscode.test-adapter-converter",
    "hbenl.vscode-test-explorer"
  ]
}
```

这些严格的规范将确保项目的一致性、可维护性和高质量，为后续的开发和维护提供坚实的基础。所有团队成员都必须严格遵守这些规范，任何违反都将在代码审查和自动化检查中被发现和纠正。