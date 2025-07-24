# IDE双空间智能管理插件 - 技术设计文档 v1.0

## 文档信息

- **文档版本**: v1.0
- **创建日期**: 2024年
- **最后更新**: 2024年
- **文档状态**: [已发布]
- **负责人**: 技术团队

## 1. 项目概述

IDE双空间智能管理插件是一个基于AI驱动的VSCode扩展，提供代码与文档的智能双空间管理功能。插件采用严格的分层架构设计，遵循现代软件工程最佳实践，确保代码的可维护性、可扩展性和高质量。

### 1.1 核心价值主张

- **双空间架构**: 在熟悉的IDE环境中提供统一的代码和文档工作空间
- **AI驱动智能**: 自动代码-文档关联和同步功能
- **零学习成本**: 原生IDE集成，保持开发者熟悉的工作流程
- **实时同步**: 智能检测和更新代码变更时的文档

### 1.2 技术目标

- 高性能：响应时间 < 200ms
- 高可用：99.9% 可用性
- 可扩展：支持多种编程语言和AI提供商
- 安全性：本地数据处理，API密钥加密存储

## 2. 技术架构设计

### 2.1 整体架构

基于 VSCode Extension API 的严格分层架构设计，遵循依赖倒置原则：

```
IDE双空间智能管理插件/
├── Code/                           # 源代码实现
│   ├── src/                        # TypeScript 源代码根目录
│   │   ├── extension.ts           # VSCode 扩展主入口点 (必须)
│   │   │
│   │   ├── core/                  # 核心业务逻辑层 (不依赖VSCode API)
│   │   │   ├── association/       # 关联关系管理模块
│   │   │   │   ├── AssociationManager.ts      # 关联关系管理器
│   │   │   │   ├── AssociationValidator.ts    # 关联关系验证器
│   │   │   │   ├── AssociationAnalyzer.ts     # 关联关系分析器
│   │   │   │   └── index.ts                   # 模块导出文件
│   │   │   ├── synchronization/   # 同步引擎模块
│   │   │   │   ├── SyncCoordinator.ts         # 同步协调器
│   │   │   │   ├── SyncStrategy.ts            # 同步策略接口
│   │   │   │   ├── RealTimeSyncStrategy.ts    # 实时同步策略
│   │   │   │   ├── BatchSyncStrategy.ts       # 批量同步策略
│   │   │   │   └── index.ts
│   │   │   ├── configuration/     # 配置管理模块
│   │   │   │   ├── ConfigManager.ts           # 配置管理器
│   │   │   │   ├── ConfigValidator.ts         # 配置验证器
│   │   │   │   ├── DefaultConfig.ts           # 默认配置
│   │   │   │   └── index.ts
│   │   │   └── index.ts           # 核心模块总导出
│   │   │
│   │   ├── providers/             # VSCode UI 提供器层
│   │   │   ├── webview/           # WebView 提供器
│   │   │   │   ├── DualSpaceProvider.ts       # 双空间主视图
│   │   │   │   ├── SettingsProvider.ts        # 设置页面提供器
│   │   │   │   └── index.ts
│   │   │   ├── tree/              # 树视图提供器
│   │   │   │   ├── AssociationTreeProvider.ts # 关联关系树
│   │   │   │   ├── FileExplorerProvider.ts    # 文件浏览器
│   │   │   │   └── index.ts
│   │   │   ├── sidebar/           # 侧边栏提供器
│   │   │   │   ├── AIAssistantProvider.ts     # AI助手侧边栏
│   │   │   │   ├── SyncStatusProvider.ts      # 同步状态面板
│   │   │   │   └── index.ts
│   │   │   └── index.ts
│   │   │
│   │   ├── services/              # 外部服务集成层
│   │   │   ├── ai-services/       # AI 服务模块
│   │   │   │   ├── AIServiceManager.ts        # AI服务管理器
│   │   │   │   ├── providers/                 # AI提供商实现
│   │   │   │   │   ├── OpenAIProvider.ts      # OpenAI 提供商
│   │   │   │   │   ├── ClaudeProvider.ts      # Claude 提供商
│   │   │   │   │   ├── LocalModelProvider.ts  # 本地模型提供商
│   │   │   │   │   └── index.ts
│   │   │   │   ├── AIResponseCache.ts         # AI响应缓存
│   │   │   │   └── index.ts
│   │   │   ├── file-services/     # 文件服务模块
│   │   │   │   ├── FileWatcher.ts             # 文件监听器
│   │   │   │   ├── FileAnalyzer.ts            # 文件分析器
│   │   │   │   ├── DocumentProcessor.ts       # 文档处理器
│   │   │   │   └── index.ts
│   │   │   ├── analysis-services/ # 代码分析服务
│   │   │   │   ├── CodeStructureAnalyzer.ts   # 代码结构分析
│   │   │   │   ├── LanguageAnalyzers/         # 语言特定分析器
│   │   │   │   │   ├── TypeScriptAnalyzer.ts  # TypeScript分析器
│   │   │   │   │   ├── JavaScriptAnalyzer.ts  # JavaScript分析器
│   │   │   │   │   ├── PythonAnalyzer.ts      # Python分析器
│   │   │   │   │   └── index.ts
│   │   │   │   └── index.ts
│   │   │   └── index.ts
│   │   │
│   │   ├── utils/                 # 工具函数层 (纯函数)
│   │   │   ├── file-utils/        # 文件操作工具
│   │   │   │   ├── FileOperations.ts          # 基础文件操作
│   │   │   │   ├── PathUtils.ts               # 路径处理工具
│   │   │   │   ├── FileTypeDetector.ts        # 文件类型检测
│   │   │   │   └── index.ts
│   │   │   ├── parsing-utils/     # 解析工具
│   │   │   │   ├── CodeParser.ts              # 代码解析器
│   │   │   │   ├── MarkdownParser.ts          # Markdown解析器
│   │   │   │   ├── ASTUtils.ts                # AST工具函数
│   │   │   │   └── index.ts
│   │   │   ├── validation-utils/  # 验证工具
│   │   │   │   ├── InputValidator.ts          # 输入验证
│   │   │   │   ├── ConfigValidator.ts         # 配置验证
│   │   │   │   ├── SchemaValidator.ts         # Schema验证
│   │   │   │   └── index.ts
│   │   │   ├── common/            # 通用工具
│   │   │   │   ├── Logger.ts                  # 日志工具
│   │   │   │   ├── EventEmitter.ts            # 事件发射器
│   │   │   │   ├── Debouncer.ts               # 防抖工具
│   │   │   │   ├── Cache.ts                   # 缓存工具
│   │   │   │   └── index.ts
│   │   │   └── index.ts
│   │   │
│   │   ├── types/                 # TypeScript 类型定义
│   │   │   ├── core/              # 核心类型定义
│   │   │   │   ├── AssociationTypes.ts        # 关联关系类型
│   │   │   │   ├── SyncTypes.ts               # 同步相关类型
│   │   │   │   ├── ConfigTypes.ts             # 配置类型
│   │   │   │   └── index.ts
│   │   │   ├── services/          # 服务类型定义
│   │   │   │   ├── AIServiceTypes.ts          # AI服务类型
│   │   │   │   ├── FileServiceTypes.ts        # 文件服务类型
│   │   │   │   ├── AnalysisTypes.ts           # 分析服务类型
│   │   │   │   └── index.ts
│   │   │   ├── ui/                # UI相关类型
│   │   │   │   ├── WebviewTypes.ts            # WebView类型
│   │   │   │   ├── TreeViewTypes.ts           # 树视图类型
│   │   │   │   ├── CommandTypes.ts            # 命令类型
│   │   │   │   └── index.ts
│   │   │   ├── common/            # 通用类型
│   │   │   │   ├── CommonTypes.ts             # 通用类型定义
│   │   │   │   ├── ErrorTypes.ts              # 错误类型
│   │   │   │   ├── EventTypes.ts              # 事件类型
│   │   │   │   └── index.ts
│   │   │   └── index.ts           # 所有类型的总导出
│   │   │
│   │   ├── commands/              # VSCode 命令处理
│   │   │   ├── association/       # 关联相关命令
│   │   │   │   ├── CreateAssociationCommand.ts
│   │   │   │   ├── DeleteAssociationCommand.ts
│   │   │   │   ├── EditAssociationCommand.ts
│   │   │   │   └── index.ts
│   │   │   ├── synchronization/   # 同步相关命令
│   │   │   │   ├── SyncAllCommand.ts
│   │   │   │   ├── SyncFileCommand.ts
│   │   │   │   ├── ResolveSyncConflictCommand.ts
│   │   │   │   └── index.ts
│   │   │   ├── ai/                # AI相关命令
│   │   │   │   ├── GenerateDocCommand.ts
│   │   │   │   ├── AnalyzeCodeCommand.ts
│   │   │   │   ├── OptimizeDocCommand.ts
│   │   │   │   └── index.ts
│   │   │   └── index.ts
│   │   │
│   │   ├── constants/             # 常量定义
│   │   │   ├── AppConstants.ts    # 应用常量
│   │   │   ├── ConfigConstants.ts # 配置常量
│   │   │   ├── UIConstants.ts     # UI常量
│   │   │   ├── APIConstants.ts    # API常量
│   │   │   └── index.ts
│   │   │
│   │   └── index.ts               # 主模块导出文件
│   │
│   ├── media/                     # Web 资源文件
│   │   ├── webview/               # WebView 资源
│   │   │   ├── dual-space/        # 双空间视图资源
│   │   │   │   ├── main.js        # 主要逻辑
│   │   │   │   ├── style.css      # 样式文件
│   │   │   │   ├── components/    # UI组件
│   │   │   │   │   ├── CodeEditor.js
│   │   │   │   │   ├── DocEditor.js
│   │   │   │   │   ├── AssociationPanel.js
│   │   │   │   │   └── StatusBar.js
│   │   │   │   └── utils/         # 前端工具函数
│   │   │   │       ├── dom-utils.js
│   │   │   │       ├── event-utils.js
│   │   │   │       └── api-client.js
│   │   │   ├── ai-assistant/      # AI助手视图资源
│   │   │   │   ├── assistant.js
│   │   │   │   ├── assistant.css
│   │   │   │   └── components/
│   │   │   └── settings/          # 设置页面资源
│   │   │       ├── settings.js
│   │   │       ├── settings.css
│   │   │       └── components/
│   │   ├── icons/                 # 图标资源
│   │   │   ├── light/             # 浅色主题图标
│   │   │   ├── dark/              # 深色主题图标
│   │   │   └── common/            # 通用图标
│   │   └── assets/                # 其他资源文件
│   │       ├── images/
│   │       ├── fonts/
│   │       └── templates/
│   │
│   ├── test/                      # 测试文件
│   ├── config/                    # 配置文件
│   ├── package.json               # 扩展清单和依赖
│   └── README.md                  # 项目说明文档
│
├── .dual-space/                   # 插件配置目录
│   ├── config/                    # 配置文件目录
│   │   ├── project-config.json    # 项目级配置
│   │   ├── user-preferences.json  # 用户偏好设置
│   │   ├── ai-provider-config.json # AI服务提供商配置
│   │   └── sync-strategy-config.json # 同步策略配置
│   ├── data/                      # 数据存储目录
│   │   ├── associations.db        # 关联关系数据库
│   │   ├── sync-history.db        # 同步历史数据库
│   │   └── backup/                # 数据备份目录
│   ├── cache/                     # 缓存目录
│   │   ├── ai-responses/          # AI响应缓存
│   │   ├── code-analysis/         # 代码分析缓存
│   │   └── file-metadata/         # 文件元数据缓存
│   └── logs/                      # 日志目录
│       ├── application.log        # 应用日志
│       ├── error.log              # 错误日志
│       └── sync.log               # 同步操作日志
│
└── Doc/                           # 项目文档
    ├── 03-技术文档/               # 技术相关文档
    │   ├── 技术架构设计-v1.md      # 本文档
    │   ├── 数据库设计-v1.md       # 数据库结构设计
    │   ├── API设计文档-v1.md      # API接口设计
    │   └── 安全设计文档-v1.md      # 安全架构设计
    └── ...                        # 其他文档目录
```

### 2.2 分层架构规范

#### 2.2.1 架构层次定义

**扩展层 (Extension Layer)**:
- 仅处理VSCode特定的集成和UI逻辑
- 不包含业务逻辑，只做数据转换和事件处理
- 文件必须以`Provider.ts`或`Command.ts`结尾

**核心业务层 (Core Business Layer)**:
- 完全独立于VSCode API，可在其他环境运行
- 包含所有业务逻辑和数据处理
- 每个模块必须有对应的接口定义
- 必须有完整的单元测试覆盖

**服务层 (Service Layer)**:
- 处理外部集成（AI、文件系统、网络）
- 实现依赖注入模式，便于测试和替换
- 每个服务必须实现标准接口
- 包含错误处理和重试机制

**工具层 (Utility Layer)**:
- 提供纯函数工具，无副作用
- 可被任何层级调用
- 包含通用的辅助功能

#### 2.2.2 模块依赖规则

```
扩展层 → 核心业务层 → 服务层
   ↓         ↓         ↓
工具层 ← 工具层 ← 工具层
```

**严格禁止的依赖关系**:
- ❌ 核心业务层不得依赖VSCode API
- ❌ 服务层不得直接访问数据层
- ❌ 工具层不得依赖业务逻辑
- ❌ 循环依赖绝对禁止

### 2.3 技术栈选择

#### 2.3.1 核心技术栈

**插件开发**:
- **主要语言**: TypeScript (严格类型检查)
- **IDE集成**: VSCode Extension API (主要), 计划支持 Cursor 等
- **运行时**: Node.js 18+
- **UI框架**: Vue.js + Electron (复杂界面)
- **样式**: CSS + VSCode主题集成

**后端服务**:
- **核心引擎**: Node.js + TypeScript
- **数据库**: SQLite (本地存储), JSON (配置)
- **文件系统**: 原生 Node.js fs + VSCode workspace APIs
- **AI集成**: 多提供商 (OpenAI, Claude, 本地模型)

#### 2.3.2 AI & ML 集成

**AI服务提供商**:
- **主要AI**: OpenAI GPT 模型 (REST API)
- **备选AI**: Anthropic Claude, 本地模型 (Ollama)
- **降级策略**: 基于模板的离线文档生成
- **缓存机制**: 本地响应缓存减少 API 成本

#### 2.3.3 数据存储

**配置存储**: JSON 文件 (`.dual-space/` 目录)
**关联数据**: SQLite 数据库 (关系映射)
**缓存存储**: 文件系统缓存 (AI 响应和分析结果)
**用户设置**: VSCode workspace/user settings 集成

#### 2.3.4 构建系统 & 开发

**包管理**:
```bash
# 安装依赖
npm install

# 开发构建 (热重载)
npm run dev

# 生产构建
npm run build

# 打包扩展
npm run package

# 运行测试
npm run test
```

**关键脚本**:
- `npm run dev` - 开发模式 (热重载)
- `npm run build` - 生产构建
- `npm run test` - 单元和集成测试
- `npm run lint` - ESLint 代码质量检查
- `npm run package` - 创建 .vsix 扩展包
- `npm run publish` - 发布到 VSCode Marketplace

**依赖管理**:
- **VSCode API**: 核心扩展功能
- **sqlite3**: 本地关联数据库
- **axios**: AI API HTTP 客户端
- **markdown-it**: Markdown 解析和渲染
- **monaco-editor**: 代码编辑器集成
- **webpack**: 模块打包和构建

## 3. 核心模块设计

### 3.1 类型系统设计

#### 3.1.1 核心类型定义

```typescript
// src/types/core/AssociationTypes.ts
export interface Association {
    id: string;
    sourceFile: string;
    targetFile: string;
    sourceRange?: vscode.Range;
    targetRange?: vscode.Range;
    type: AssociationType;
    description?: string;
    lastSyncTime: number;
    isActive: boolean;
    metadata?: AssociationMetadata;
}

export enum AssociationType {
    FUNCTION = 'function',
    CLASS = 'class',
    MODULE = 'module',
    MANUAL = 'manual',
    AUTO_DETECTED = 'auto_detected'
}

export interface AssociationMetadata {
    confidence: number;
    aiGenerated: boolean;
    tags: string[];
    priority: AssociationPriority;
}

export enum AssociationPriority {
    LOW = 1,
    MEDIUM = 2,
    HIGH = 3,
    CRITICAL = 4
}
```

```typescript
// src/types/core/SyncTypes.ts
export interface SyncOperation {
    id: string;
    associationId: string;
    type: SyncOperationType;
    status: SyncStatus;
    startTime: number;
    endTime?: number;
    error?: SyncError;
    changes: SyncChange[];
}

export enum SyncOperationType {
    CODE_TO_DOC = 'code_to_doc',
    DOC_TO_CODE = 'doc_to_code',
    BIDIRECTIONAL = 'bidirectional',
    VALIDATION = 'validation'
}

export enum SyncStatus {
    PENDING = 'pending',
    IN_PROGRESS = 'in_progress',
    COMPLETED = 'completed',
    FAILED = 'failed',
    CANCELLED = 'cancelled'
}

export interface SyncChange {
    filePath: string;
    changeType: ChangeType;
    oldContent?: string;
    newContent: string;
    range?: vscode.Range;
}

export enum ChangeType {
    INSERT = 'insert',
    UPDATE = 'update',
    DELETE = 'delete',
    MOVE = 'move'
}
```

#### 3.1.2 错误处理类型

```typescript
// src/types/common/ErrorTypes.ts
export enum ErrorCodes {
    // 系统错误 (1000-1999)
    SYSTEM_ERROR = 1000,
    FILE_NOT_FOUND = 1001,
    PERMISSION_DENIED = 1002,
    INVALID_CONFIGURATION = 1003,
    
    // 业务错误 (2000-2999)
    ASSOCIATION_NOT_FOUND = 2000,
    SYNC_CONFLICT = 2001,
    INVALID_ASSOCIATION = 2002,
    DUPLICATE_ASSOCIATION = 2003,
    
    // AI服务错误 (3000-3999)
    AI_SERVICE_UNAVAILABLE = 3000,
    AI_QUOTA_EXCEEDED = 3001,
    AI_RESPONSE_INVALID = 3002,
    AI_TIMEOUT = 3003,
    
    // 用户错误 (4000-4999)
    INVALID_INPUT = 4000,
    UNAUTHORIZED_ACCESS = 4001,
    OPERATION_CANCELLED = 4002
}

export class DualSpaceError extends Error {
    constructor(
        public code: ErrorCodes,
        message: string,
        public details?: any,
        public recoverable: boolean = true,
        public userMessage?: string
    ) {
        super(message);
        this.name = 'DualSpaceError';
    }
}

export interface ErrorContext {
    operation: string;
    module: string;
    timestamp: number;
    userId?: string;
    sessionId?: string;
    additionalData?: Record<string, any>;
}
```

### 3.2 核心业务模块

#### 3.2.1 关联关系管理器

```typescript
// src/core/association/AssociationManager.ts
/**
 * @fileoverview 关联关系管理器 - 负责代码与文档关联关系的创建、管理和维护
 * @author 技术团队
 * @since v1.0.0
 */

import * as vscode from 'vscode';
import { Association, AssociationType, AssociationMetadata } from '../../types/core/AssociationTypes';
import { DualSpaceError, ErrorCodes } from '../../types/common/ErrorTypes';
import { Logger } from '../../utils/common/Logger';
import { EventEmitter } from '../../utils/common/EventEmitter';
import { AssociationValidator } from './AssociationValidator';
import { AssociationAnalyzer } from './AssociationAnalyzer';

export class AssociationManager {
    private associations: Map<string, Association> = new Map();
    private logger: Logger;
    private eventEmitter: EventEmitter;
    private validator: AssociationValidator;
    private analyzer: AssociationAnalyzer;

    constructor(
        private context: vscode.ExtensionContext,
        private databasePath: string
    ) {
        this.logger = new Logger('AssociationManager');
        this.eventEmitter = new EventEmitter();
        this.validator = new AssociationValidator();
        this.analyzer = new AssociationAnalyzer();
        
        this.initializeDatabase();
        this.loadExistingAssociations();
    }

    /**
     * 创建新的关联关系
     */
    public async createAssociation(
        sourceFile: string,
        targetFile: string,
        options: Partial<Association> = {}
    ): Promise<Association> {
        this.logger.info('创建关联关系', { sourceFile, targetFile, options });

        try {
            // 验证输入参数
            await this.validator.validateAssociationInput(sourceFile, targetFile, options);

            // 检查是否已存在相同关联
            const existingId = this.generateAssociationId(sourceFile, targetFile);
            if (this.associations.has(existingId)) {
                throw new DualSpaceError(
                    ErrorCodes.DUPLICATE_ASSOCIATION,
                    `关联关系已存在: ${sourceFile} -> ${targetFile}`,
                    { sourceFile, targetFile },
                    true,
                    '该关联关系已经存在，请检查后重试'
                );
            }

            // 创建关联对象
            const association: Association = {
                id: existingId,
                sourceFile,
                targetFile,
                type: options.type || AssociationType.MANUAL,
                description: options.description,
                sourceRange: options.sourceRange,
                targetRange: options.targetRange,
                lastSyncTime: Date.now(),
                isActive: true,
                metadata: options.metadata || {
                    confidence: 1.0,
                    aiGenerated: false,
                    tags: [],
                    priority: AssociationPriority.MEDIUM
                }
            };

            // 保存到数据库
            await this.saveAssociationToDatabase(association);
            
            // 缓存到内存
            this.associations.set(association.id, association);

            // 发送事件通知
            this.eventEmitter.emit('associationCreated', association);

            this.logger.info('关联关系创建成功', { associationId: association.id });
            return association;

        } catch (error) {
            this.logger.error('创建关联关系失败', { error, sourceFile, targetFile });
            throw error;
        }
    }

    /**
     * 获取文件的所有关联关系
     */
    public async getAssociationsByFile(filePath: string): Promise<Association[]> {
        const associations = Array.from(this.associations.values()).filter(
            assoc => assoc.isActive && (
                assoc.sourceFile === filePath || 
                assoc.targetFile === filePath
            )
        );

        this.logger.debug('获取文件关联关系', { filePath, count: associations.length });
        return associations;
    }

    /**
     * 更新关联关系
     */
    public async updateAssociation(
        id: string, 
        updates: Partial<Association>
    ): Promise<Association> {
        const association = this.associations.get(id);
        if (!association) {
            throw new DualSpaceError(
                ErrorCodes.ASSOCIATION_NOT_FOUND,
                `关联关系不存在: ${id}`,
                { associationId: id },
                false,
                '找不到指定的关联关系'
            );
        }

        // 验证更新数据
        await this.validator.validateAssociationUpdate(association, updates);

        // 应用更新
        const updatedAssociation = { 
            ...association, 
            ...updates,
            lastSyncTime: Date.now()
        };

        // 保存到数据库
        await this.saveAssociationToDatabase(updatedAssociation);
        
        // 更新内存缓存
        this.associations.set(id, updatedAssociation);

        // 发送事件通知
        this.eventEmitter.emit('associationUpdated', updatedAssociation);

        this.logger.info('关联关系更新成功', { associationId: id });
        return updatedAssociation;
    }

    /**
     * 删除关联关系
     */
    public async deleteAssociation(id: string): Promise<void> {
        const association = this.associations.get(id);
        if (!association) {
            this.logger.warn('尝试删除不存在的关联关系', { associationId: id });
            return;
        }

        try {
            // 从数据库删除
            await this.deleteAssociationFromDatabase(id);
            
            // 从内存删除
            this.associations.delete(id);

            // 发送事件通知
            this.eventEmitter.emit('associationDeleted', association);

            this.logger.info('关联关系删除成功', { associationId: id });

        } catch (error) {
            this.logger.error('删除关联关系失败', { error, associationId: id });
            throw error;
        }
    }

    /**
     * 分析文件以发现潜在关联
     */
    public async analyzeFileForAssociations(filePath: string): Promise<Association[]> {
        this.logger.info('分析文件关联关系', { filePath });

        try {
            const suggestions = await this.analyzer.analyzeFile(filePath);
            
            this.logger.info('文件分析完成', { 
                filePath, 
                suggestionsCount: suggestions.length 
            });

            return suggestions;

        } catch (error) {
            this.logger.error('文件分析失败', { error, filePath });
            throw new DualSpaceError(
                ErrorCodes.SYSTEM_ERROR,
                `文件分析失败: ${error.message}`,
                { filePath, originalError: error },
                true,
                '文件分析过程中出现错误，请稍后重试'
            );
        }
    }

    /**
     * 事件监听器注册
     */
    public on(event: string, listener: (...args: any[]) => void): void {
        this.eventEmitter.on(event, listener);
    }

    /**
     * 获取所有活跃关联关系
     */
    public getAllActiveAssociations(): Association[] {
        return Array.from(this.associations.values()).filter(assoc => assoc.isActive);
    }

    // 私有方法
    private generateAssociationId(sourceFile: string, targetFile: string): string {
        const crypto = require('crypto');
        const hash = crypto
            .createHash('md5')
            .update(`${sourceFile}:${targetFile}`)
            .digest('hex');
        return `assoc_${hash.substring(0, 12)}`;
    }

    private async initializeDatabase(): Promise<void> {
        // 数据库初始化逻辑
        this.logger.info('初始化关联关系数据库');
    }

    private async loadExistingAssociations(): Promise<void> {
        // 从数据库加载现有关联关系
        this.logger.info('加载现有关联关系');
    }

    private async saveAssociationToDatabase(association: Association): Promise<void> {
        // 保存关联关系到数据库
        this.logger.debug('保存关联关系到数据库', { associationId: association.id });
    }

    private async deleteAssociationFromDatabase(id: string): Promise<void> {
        // 从数据库删除关联关系
        this.logger.debug('从数据库删除关联关系', { associationId: id });
    }
}
```

## 4. 核心代码实现

### 4.1 主入口扩展文件

```typescript
// src/extension.ts
/**
 * @fileoverview VSCode扩展主入口点 - 负责插件的激活、初始化和生命周期管理
 * @author 技术团队
 * @since v1.0.0
 */

import * as vscode from 'vscode';
import * as path from 'path';

// 核心业务层导入
import { AssociationManager } from './core/association/AssociationManager';
import { SyncCoordinator } from './core/synchronization/SyncCoordinator';
import { ConfigManager } from './core/configuration/ConfigManager';

// 提供器层导入
import { DualSpaceProvider } from './providers/webview/DualSpaceProvider';
import { AIAssistantProvider } from './providers/sidebar/AIAssistantProvider';
import { AssociationTreeProvider } from './providers/tree/AssociationTreeProvider';
import { SettingsProvider } from './providers/webview/SettingsProvider';

// 服务层导入
import { AIServiceManager } from './services/ai-services/AIServiceManager';
import { FileWatcher } from './services/file-services/FileWatcher';

// 命令处理导入
import { registerAssociationCommands } from './commands/association';
import { registerSyncCommands } from './commands/synchronization';
import { registerAICommands } from './commands/ai';

// 工具层导入
import { Logger } from './utils/common/Logger';
import { EventEmitter } from './utils/common/EventEmitter';

// 类型导入
import type { ExtensionContext } from './types/ui/CommandTypes';

// 常量导入
import { APP_NAME, EXTENSION_ID, DATABASE_NAME } from './constants/AppConstants';

/**
 * 扩展激活函数
 * 按照严格的初始化顺序启动各个组件
 */
export async function activate(context: vscode.ExtensionContext): Promise<void> {
    const logger = new Logger('Extension');
    logger.info(`${APP_NAME} 开始激活`, { extensionId: EXTENSION_ID });

    try {
        // 1. 初始化配置管理器
        const configManager = new ConfigManager(context);
        await configManager.initialize();

        // 2. 初始化数据库路径
        const databasePath = path.join(context.globalStoragePath, DATABASE_NAME);
        
        // 3. 初始化核心业务服务
        const associationManager = new AssociationManager(context, databasePath);
        const syncCoordinator = new SyncCoordinator(associationManager, configManager);
        const aiServiceManager = new AIServiceManager(configManager);

        // 4. 初始化文件监听服务
        const fileWatcher = new FileWatcher(syncCoordinator);
        await fileWatcher.initialize();

        // 5. 注册UI提供器
        await registerUIProviders(context, associationManager, aiServiceManager, configManager);

        // 6. 注册命令处理器
        registerCommands(context, associationManager, syncCoordinator, aiServiceManager);

        // 7. 设置文件变更监听
        setupFileWatchers(context, fileWatcher);

        // 8. 初始化状态栏
        setupStatusBar(context);

        // 9. 设置上下文菜单
        setupContextMenus(context);

        logger.info(`${APP_NAME} 激活完成`);

    } catch (error) {
        logger.error('扩展激活失败', { error });
        vscode.window.showErrorMessage(`${APP_NAME} 激活失败: ${error.message}`);
        throw error;
    }
}

/**
 * 注册UI提供器
 */
async function registerUIProviders(
    context: vscode.ExtensionContext,
    associationManager: AssociationManager,
    aiServiceManager: AIServiceManager,
    configManager: ConfigManager
): Promise<void> {
    const logger = new Logger('UIProviders');

    try {
        // 注册双空间主视图提供器
        const dualSpaceProvider = new DualSpaceProvider(
            context, 
            associationManager, 
            aiServiceManager
        );
        
        // 创建WebView面板
        const dualSpacePanel = vscode.window.createWebviewPanel(
            'dualSpace.main',
            '双空间管理',
            vscode.ViewColumn.One,
            {
                enableScripts: true,
                retainContextWhenHidden: true,
                localResourceRoots: [
                    vscode.Uri.joinPath(context.extensionUri, 'media')
                ]
            }
        );
        
        dualSpaceProvider.resolveWebviewView(dualSpacePanel);
        context.subscriptions.push(dualSpacePanel);

        // 注册AI助手侧边栏
        const aiAssistantProvider = new AIAssistantProvider(
            context, 
            aiServiceManager, 
            associationManager
        );
        vscode.window.registerWebviewViewProvider(
            'dualSpace.aiAssistant', 
            aiAssistantProvider,
            { webviewOptions: { retainContextWhenHidden: true } }
        );

        // 注册关联关系树视图
        const associationTreeProvider = new AssociationTreeProvider(associationManager);
        vscode.window.registerTreeDataProvider(
            'dualSpace.associationExplorer', 
            associationTreeProvider
        );

        // 注册设置页面提供器
        const settingsProvider = new SettingsProvider(context, configManager);
        vscode.window.registerWebviewViewProvider(
            'dualSpace.settings',
            settingsProvider
        );

        logger.info('UI提供器注册完成');

    } catch (error) {
        logger.error('UI提供器注册失败', { error });
        throw error;
    }
}

/**
 * 注册命令处理器
 */
function registerCommands(
    context: vscode.ExtensionContext,
    associationManager: AssociationManager,
    syncCoordinator: SyncCoordinator,
    aiServiceManager: AIServiceManager
): void {
    const logger = new Logger('Commands');

    try {
        // 注册关联相关命令
        registerAssociationCommands(context, associationManager);
        
        // 注册同步相关命令
        registerSyncCommands(context, syncCoordinator);
        
        // 注册AI相关命令
        registerAICommands(context, aiServiceManager, associationManager);

        // 注册通用命令
        const generalCommands = [
            vscode.commands.registerCommand('dualSpace.openSettings', () => {
                vscode.commands.executeCommand('workbench.view.extension.dualSpace');
            }),
            
            vscode.commands.registerCommand('dualSpace.showOutput', () => {
                vscode.commands.executeCommand('workbench.action.output.toggleOutput');
            }),
            
            vscode.commands.registerCommand('dualSpace.refreshAll', async () => {
                await vscode.commands.executeCommand('dualSpace.refreshAssociations');
                await vscode.commands.executeCommand('dualSpace.refreshAI');
            })
        ];

        generalCommands.forEach(cmd => context.subscriptions.push(cmd));
        
        logger.info('命令注册完成');

    } catch (error) {
        logger.error('命令注册失败', { error });
        throw error;
    }
}

/**
 * 设置文件监听器
 */
function setupFileWatchers(
    context: vscode.ExtensionContext,
    fileWatcher: FileWatcher
): void {
    const logger = new Logger('FileWatchers');

    try {
        // 监听工作区文件变化
        const workspaceWatcher = vscode.workspace.createFileSystemWatcher(
            '**/*.{ts,js,py,md,txt}',
            false, // 不忽略创建
            false, // 不忽略修改
            false  // 不忽略删除
        );

        workspaceWatcher.onDidCreate(uri => {
            fileWatcher.onFileCreated(uri.fsPath);
        });

        workspaceWatcher.onDidChange(uri => {
            fileWatcher.onFileChanged(uri.fsPath);
        });

        workspaceWatcher.onDidDelete(uri => {
            fileWatcher.onFileDeleted(uri.fsPath);
        });

        context.subscriptions.push(workspaceWatcher);

        // 监听活动编辑器变化
        const activeEditorWatcher = vscode.window.onDidChangeActiveTextEditor(editor => {
            if (editor) {
                fileWatcher.onActiveEditorChanged(editor.document.uri.fsPath);
            }
        });

        context.subscriptions.push(activeEditorWatcher);

        logger.info('文件监听器设置完成');

    } catch (error) {
        logger.error('文件监听器设置失败', { error });
        throw error;
    }
}

/**
 * 设置状态栏
 */
function setupStatusBar(context: vscode.ExtensionContext): void {
    const statusBarItem = vscode.window.createStatusBarItem(
        vscode.StatusBarAlignment.Right,
        100
    );
    
    statusBarItem.text = '$(link) 双空间';
    statusBarItem.tooltip = '双空间智能管理插件';
    statusBarItem.command = 'dualSpace.openMain';
    statusBarItem.show();

    context.subscriptions.push(statusBarItem);
}

/**
 * 设置上下文菜单
 */
function setupContextMenus(context: vscode.ExtensionContext): void {
    // 上下文菜单将通过 package.json 中的 contributes.menus 配置
    // 这里可以添加动态菜单逻辑
}

/**
 * 扩展停用函数
 */
export function deactivate(): void {
    const logger = new Logger('Extension');
    logger.info(`${APP_NAME} 开始停用`);
    
    // 清理资源
    // 大部分清理工作由 VSCode 自动处理
    // 这里可以添加特殊的清理逻辑
    
    logger.info(`${APP_NAME} 停用完成`);
}
```

### 4.2 同步协调器

```typescript
// src/core/synchronization/SyncCoordinator.ts
/**
 * @fileoverview 同步协调器 - 负责代码与文档之间的智能同步
 * @author 技术团队
 * @since v1.0.0
 */

import { AssociationManager } from '../association/AssociationManager';
import { ConfigManager } from '../configuration/ConfigManager';
import { SyncOperation, SyncOperationType, SyncStatus } from '../../types/core/SyncTypes';
import { DualSpaceError, ErrorCodes } from '../../types/common/ErrorTypes';
import { Logger } from '../../utils/common/Logger';
import { EventEmitter } from '../../utils/common/EventEmitter';
import { RealTimeSyncStrategy } from './RealTimeSyncStrategy';
import { BatchSyncStrategy } from './BatchSyncStrategy';

export class SyncCoordinator {
    private logger: Logger;
    private eventEmitter: EventEmitter;
    private activeSyncOperations: Map<string, SyncOperation> = new Map();
    private realTimeStrategy: RealTimeSyncStrategy;
    private batchStrategy: BatchSyncStrategy;

    constructor(
        private associationManager: AssociationManager,
        private configManager: ConfigManager
    ) {
        this.logger = new Logger('SyncCoordinator');
        this.eventEmitter = new EventEmitter();
        this.realTimeStrategy = new RealTimeSyncStrategy();
        this.batchStrategy = new BatchSyncStrategy();
        
        this.setupEventListeners();
    }

    /**
     * 同步所有关联关系
     */
    public async syncAll(): Promise<SyncOperation[]> {
        this.logger.info('开始同步所有关联关系');

        const associations = this.associationManager.getAllActiveAssociations();
        const syncOperations: SyncOperation[] = [];

        for (const association of associations) {
            try {
                const operation = await this.syncAssociation(
                    association.id,
                    SyncOperationType.BIDIRECTIONAL
                );
                syncOperations.push(operation);
            } catch (error) {
                this.logger.error('关联同步失败', { 
                    associationId: association.id, 
                    error 
                });
            }
        }

        this.logger.info('所有关联关系同步完成', { 
            total: associations.length,
            successful: syncOperations.filter(op => op.status === SyncStatus.COMPLETED).length
        });

        return syncOperations;
    }

    /**
     * 同步单个关联关系
     */
    public async syncAssociation(
        associationId: string,
        type: SyncOperationType = SyncOperationType.BIDIRECTIONAL
    ): Promise<SyncOperation> {
        this.logger.info('开始同步关联关系', { associationId, type });

        // 检查是否已有进行中的同步操作
        if (this.activeSyncOperations.has(associationId)) {
            throw new DualSpaceError(
                ErrorCodes.SYNC_CONFLICT,
                `关联 ${associationId} 正在同步中`,
                { associationId },
                true,
                '该关联正在同步中，请稍后再试'
            );
        }

        const syncOperation: SyncOperation = {
            id: this.generateSyncOperationId(),
            associationId,
            type,
            status: SyncStatus.PENDING,
            startTime: Date.now(),
            changes: []
        };

        try {
            // 记录开始同步
            this.activeSyncOperations.set(associationId, syncOperation);
            syncOperation.status = SyncStatus.IN_PROGRESS;
            
            this.eventEmitter.emit('syncStarted', syncOperation);

            // 获取关联信息
            const associations = await this.associationManager.getAssociationsByFile('');
            const association = associations.find(a => a.id === associationId);
            
            if (!association) {
                throw new DualSpaceError(
                    ErrorCodes.ASSOCIATION_NOT_FOUND,
                    `关联关系不存在: ${associationId}`,
                    { associationId }
                );
            }

            // 根据配置选择同步策略
            const syncConfig = await this.configManager.getSyncConfiguration();
            const strategy = syncConfig.realTimeSync ? 
                this.realTimeStrategy : 
                this.batchStrategy;

            // 执行同步
            const changes = await strategy.sync(association, type);
            syncOperation.changes = changes;
            syncOperation.status = SyncStatus.COMPLETED;
            syncOperation.endTime = Date.now();

            // 更新关联的最后同步时间
            await this.associationManager.updateAssociation(associationId, {
                lastSyncTime: Date.now()
            });

            this.eventEmitter.emit('syncCompleted', syncOperation);
            this.logger.info('关联同步完成', { 
                associationId, 
                changesCount: changes.length,
                duration: syncOperation.endTime - syncOperation.startTime
            });

            return syncOperation;

        } catch (error) {
            syncOperation.status = SyncStatus.FAILED;
            syncOperation.endTime = Date.now();
            syncOperation.error = {
                code: error.code || ErrorCodes.SYSTEM_ERROR,
                message: error.message,
                details: error.details
            };

            this.eventEmitter.emit('syncFailed', syncOperation);
            this.logger.error('关联同步失败', { associationId, error });
            
            throw error;

        } finally {
            // 清理活跃同步操作
            this.activeSyncOperations.delete(associationId);
        }
    }

    /**
     * 取消同步操作
     */
    public async cancelSync(associationId: string): Promise<void> {
        const operation = this.activeSyncOperations.get(associationId);
        if (!operation) {
            this.logger.warn('尝试取消不存在的同步操作', { associationId });
            return;
        }

        operation.status = SyncStatus.CANCELLED;
        operation.endTime = Date.now();
        
        this.activeSyncOperations.delete(associationId);
        this.eventEmitter.emit('syncCancelled', operation);
        
        this.logger.info('同步操作已取消', { associationId });
    }

    /**
     * 获取活跃的同步操作
     */
    public getActiveSyncOperations(): SyncOperation[] {
        return Array.from(this.activeSyncOperations.values());
    }

    /**
     * 事件监听器注册
     */
    public on(event: string, listener: (...args: any[]) => void): void {
        this.eventEmitter.on(event, listener);
    }

    // 私有方法
    private setupEventListeners(): void {
        // 监听关联关系变化
        this.associationManager.on('associationUpdated', (association) => {
            this.handleAssociationUpdated(association);
        });

        this.associationManager.on('associationDeleted', (association) => {
            this.handleAssociationDeleted(association);
        });
    }

    private async handleAssociationUpdated(association: any): Promise<void> {
        // 关联更新时的处理逻辑
        const config = await this.configManager.getSyncConfiguration();
        if (config.autoSyncOnChange) {
            await this.syncAssociation(association.id);
        }
    }

    private async handleAssociationDeleted(association: any): Promise<void> {
        // 关联删除时取消相关同步操作
        if (this.activeSyncOperations.has(association.id)) {
            await this.cancelSync(association.id);
        }
    }

    private generateSyncOperationId(): string {
        return `sync_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
    }
}
```

### 4.3 AI服务管理器

```typescript
// src/services/ai-services/AIServiceManager.ts
/**
 * @fileoverview AI服务管理器 - 统一管理多个AI服务提供商
 * @author 技术团队
 * @since v1.0.0
 */

import { ConfigManager } from '../../core/configuration/ConfigManager';
import { OpenAIProvider } from './providers/OpenAIProvider';
import { ClaudeProvider } from './providers/ClaudeProvider';
import { LocalModelProvider } from './providers/LocalModelProvider';
import { AIResponseCache } from './AIResponseCache';
import { 
    AIServiceTypes, 
    DocumentationRequest, 
    AIResponse,
    AIProvider 
} from '../../types/services/AIServiceTypes';
import { DualSpaceError, ErrorCodes } from '../../types/common/ErrorTypes';
import { Logger } from '../../utils/common/Logger';

export class AIServiceManager {
    private logger: Logger;
    private providers: Map<AIServiceTypes, AIProvider> = new Map();
    private responseCache: AIResponseCache;
    private currentProvider: AIServiceTypes;

    constructor(private configManager: ConfigManager) {
        this.logger = new Logger('AIServiceManager');
        this.responseCache = new AIResponseCache();
        
        this.initializeProviders();
        this.loadConfiguration();
    }

    /**
     * 生成文档
     */
    public async generateDocumentation(request: DocumentationRequest): Promise<AIResponse> {
        this.logger.info('生成文档请求', { 
            language: request.language, 
            type: request.type,
            provider: this.currentProvider
        });

        try {
            // 检查缓存
            const cacheKey = this.generateCacheKey(request);
            const cachedResponse = await this.responseCache.get(cacheKey);
            
            if (cachedResponse) {
                this.logger.debug('使用缓存响应', { cacheKey });
                return cachedResponse;
            }

            // 获取当前提供商
            const provider = this.providers.get(this.currentProvider);
            if (!provider) {
                throw new DualSpaceError(
                    ErrorCodes.AI_SERVICE_UNAVAILABLE,
                    `AI提供商不可用: ${this.currentProvider}`,
                    { provider: this.currentProvider }
                );
            }

            // 调用AI服务
            const response = await provider.generateDocumentation(request);
            
            // 缓存响应
            await this.responseCache.set(cacheKey, response);

            this.logger.info('文档生成完成', { 
                provider: this.currentProvider,
                confidence: response.confidence
            });

            return response;

        } catch (error) {
            this.logger.error('文档生成失败', { error, request });
            
            // 尝试降级到其他提供商
            if (await this.shouldFallback(error)) {
                return this.generateWithFallback(request);
            }
            
            throw error;
        }
    }

    /**
     * 分析代码结构
     */
    public async analyzeCodeStructure(
        code: string, 
        language: string
    ): Promise<string[]> {
        const provider = this.providers.get(this.currentProvider);
        if (!provider) {
            throw new DualSpaceError(
                ErrorCodes.AI_SERVICE_UNAVAILABLE,
                `AI提供商不可用: ${this.currentProvider}`
            );
        }

        return provider.analyzeCodeStructure(code, language);
    }

    /**
     * 切换AI提供商
     */
    public async switchProvider(providerType: AIServiceTypes): Promise<void> {
        if (!this.providers.has(providerType)) {
            throw new DualSpaceError(
                ErrorCodes.INVALID_CONFIGURATION,
                `不支持的AI提供商: ${providerType}`
            );
        }

        this.currentProvider = providerType;
        await this.configManager.updateAIConfiguration({ provider: providerType });
        
        this.logger.info('AI提供商已切换', { provider: providerType });
    }

    /**
     * 获取提供商状态
     */
    public async getProviderStatus(): Promise<Record<AIServiceTypes, boolean>> {
        const status: Record<AIServiceTypes, boolean> = {} as any;
        
        for (const [type, provider] of this.providers) {
            try {
                status[type] = await provider.isAvailable();
            } catch {
                status[type] = false;
            }
        }

        return status;
    }

    // 私有方法
    private initializeProviders(): void {
        this.providers.set(AIServiceTypes.OPENAI, new OpenAIProvider(this.configManager));
        this.providers.set(AIServiceTypes.CLAUDE, new ClaudeProvider(this.configManager));
        this.providers.set(AIServiceTypes.LOCAL, new LocalModelProvider(this.configManager));
    }

    private async loadConfiguration(): Promise<void> {
        const config = await this.configManager.getAIConfiguration();
        this.currentProvider = config.provider || AIServiceTypes.OPENAI;
    }

    private generateCacheKey(request: DocumentationRequest): string {
        const crypto = require('crypto');
        const content = JSON.stringify({
            code: request.code,
            language: request.language,
            type: request.type,
            context: request.context
        });
        
        return crypto.createHash('md5').update(content).digest('hex');
    }

    private async shouldFallback(error: any): Promise<boolean> {
        // 判断是否应该降级到其他提供商
        return error.code === ErrorCodes.AI_SERVICE_UNAVAILABLE ||
               error.code === ErrorCodes.AI_QUOTA_EXCEEDED ||
               error.code === ErrorCodes.AI_TIMEOUT;
    }

    private async generateWithFallback(request: DocumentationRequest): Promise<AIResponse> {
        const fallbackOrder = this.getFallbackOrder();
        
        for (const providerType of fallbackOrder) {
            if (providerType === this.currentProvider) continue;
            
            const provider = this.providers.get(providerType);
            if (!provider) continue;

            try {
                this.logger.info('尝试降级提供商', { provider: providerType });
                const response = await provider.generateDocumentation(request);
                
                this.logger.info('降级提供商成功', { provider: providerType });
                return response;
                
            } catch (error) {
                this.logger.warn('降级提供商失败', { provider: providerType, error });
                continue;
            }
        }

        throw new DualSpaceError(
            ErrorCodes.AI_SERVICE_UNAVAILABLE,
            '所有AI提供商都不可用',
            { providers: fallbackOrder }
        );
    }

    private getFallbackOrder(): AIServiceTypes[] {
        // 根据当前提供商返回降级顺序
        switch (this.currentProvider) {
            case AIServiceTypes.OPENAI:
                return [AIServiceTypes.CLAUDE, AIServiceTypes.LOCAL];
            case AIServiceTypes.CLAUDE:
                return [AIServiceTypes.OPENAI, AIServiceTypes.LOCAL];
            case AIServiceTypes.LOCAL:
                return [AIServiceTypes.OPENAI, AIServiceTypes.CLAUDE];
            default:
                return [AIServiceTypes.OPENAI, AIServiceTypes.CLAUDE, AIServiceTypes.LOCAL];
        }
    }
}
```

### 4.4 双空间主视图组件

```typescript
// src/providers/DualSpaceProvider.ts
import * as vscode from 'vscode';
import { AssociationManager } from '../core/AssociationManager';

export class DualSpaceProvider {
    private _view?: vscode.WebviewPanel;

    constructor(
        private context: vscode.ExtensionContext,
        private associationManager: AssociationManager
    ) {}

    public resolveWebviewView(webviewPanel: vscode.WebviewPanel) {
        this._view = webviewPanel;

        webviewPanel.webview.options = {
            enableScripts: true,
            localResourceRoots: [this.context.extensionUri]
        };

        webviewPanel.webview.html = this.getHtmlForWebview(webviewPanel.webview);
      
        // 处理来自WebView的消息
        webviewPanel.webview.onDidReceiveMessage(async (message) => {
            switch (message.command) {
                case 'loadFile':
                    return this.loadFile(message.filePath);
                case 'saveFile':
                    return this.saveFile(message.filePath, message.content);
                case 'createAssociation':
                    return this.createAssociation(message.sourceFile, message.targetFile);
                case 'syncContent':
                    return this.syncContent(message.associationId);
            }
        });
    }

    private getHtmlForWebview(webview: vscode.Webview): string {
        const scriptUri = webview.asWebviewUri(
            vscode.Uri.joinPath(this.context.extensionUri, 'media', 'main.js')
        );
        const styleUri = webview.asWebviewUri(
            vscode.Uri.joinPath(this.context.extensionUri, 'media', 'style.css')
        );

        return `<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>双空间管理</title>
    <link href="${styleUri}" rel="stylesheet">
</head>
<body>
    <div id="app">
        <!-- 双空间主界面 -->
        <div class="dual-space-container">
            <!-- 顶部工具栏 -->
            <div class="toolbar">
                <div class="toolbar-left">
                    <button id="newAssociation" class="btn btn-primary">
                        <i class="icon-link"></i> 新建关联
                    </button>
                    <button id="syncAll" class="btn btn-secondary">
                        <i class="icon-sync"></i> 同步全部
                    </button>
                </div>
                <div class="toolbar-right">
                    <div class="space-ratio-control">
                        <input type="range" id="spaceRatio" min="20" max="80" value="50">
                        <span>空间比例</span>
                    </div>
                </div>
            </div>

            <!-- 双空间工作区 -->
            <div class="workspace" id="workspace">
                <!-- 代码空间 -->
                <div class="code-space" id="codeSpace">
                    <div class="space-header">
                        <div class="space-title">
                            <i class="icon-code"></i>
                            <span>代码空间</span>
                            <div class="file-path" id="codeFilePath"></div>
                        </div>
                        <div class="space-actions">
                            <button class="btn-icon" id="openInEditor" title="在编辑器中打开">
                                <i class="icon-external-link"></i>
                            </button>
                        </div>
                    </div>
                    <div class="space-content">
                        <div class="editor-container" id="codeEditor"></div>
                        <div class="association-indicators" id="codeAssociations"></div>
                    </div>
                </div>

                <!-- 拖拽分割条 -->
                <div class="splitter" id="splitter"></div>

                <!-- 文档空间 -->
                <div class="doc-space" id="docSpace">
                    <div class="space-header">
                        <div class="space-title">
                            <i class="icon-file-text"></i>
                            <span>文档空间</span>
                            <div class="file-path" id="docFilePath"></div>
                        </div>
                        <div class="space-actions">
                            <button class="btn-icon" id="editMode" title="编辑模式">
                                <i class="icon-edit"></i>
                            </button>
                            <button class="btn-icon" id="previewMode" title="预览模式">
                                <i class="icon-eye"></i>
                            </button>
                        </div>
                    </div>
                    <div class="space-content">
                        <div class="editor-container" id="docEditor"></div>
                        <div class="preview-container" id="docPreview" style="display:none;"></div>
                    </div>
                </div>
            </div>

            <!-- 状态栏 -->
            <div class="status-bar">
                <div class="status-left">
                    <div class="sync-status" id="syncStatus">
                        <i class="icon-check-circle text-success"></i>
                        <span>已同步</span>
                    </div>
                    <div class="association-count" id="associationCount">
                        关联数: 0
                    </div>
                </div>
                <div class="status-right">
                    <button class="btn-link" id="openSettings">设置</button>
                </div>
            </div>
        </div>

        <!-- 加载遮罩 -->
        <div class="loading-overlay" id="loadingOverlay" style="display:none;">
            <div class="loading-spinner"></div>
            <div class="loading-text">正在处理...</div>
        </div>
    </div>

    <script src="${scriptUri}"></script>
</body>
</html>`;
    }

    private async loadFile(filePath: string) {
        try {
            const uri = vscode.Uri.file(filePath);
            const document = await vscode.workspace.openTextDocument(uri);
            const content = document.getText();
          
            this._view?.webview.postMessage({
                command: 'fileLoaded',
                filePath,
                content,
                language: document.languageId
            });
        } catch (error) {
            vscode.window.showErrorMessage(`加载文件失败: ${error}`);
        }
    }

    private async saveFile(filePath: string, content: string) {
        try {
            const uri = vscode.Uri.file(filePath);
            const encoder = new TextEncoder();
            await vscode.workspace.fs.writeFile(uri, encoder.encode(content));
          
            vscode.window.showInformationMessage('文件已保存');
        } catch (error) {
            vscode.window.showErrorMessage(`保存文件失败: ${error}`);
        }
    }
}
```

### 3. 关联关系管理核心

```typescript
// src/core/AssociationManager.ts
import * as vscode from 'vscode';
import * as path from 'path';
import { Database } from 'sqlite3';

export interface Association {
    id: string;
    sourceFile: string;
    targetFile: string;
    sourceRange?: vscode.Range;
    targetRange?: vscode.Range;
    type: 'function' | 'class' | 'module' | 'manual';
    description?: string;
    lastSyncTime: number;
    isActive: boolean;
}

export class AssociationManager {
    private db: Database;
    private associations: Map<string, Association> = new Map();

    constructor(private context: vscode.ExtensionContext) {
        this.initDatabase();
        this.loadAssociations();
    }

    private initDatabase() {
        const dbPath = path.join(this.context.globalStoragePath, 'associations.db');
        this.db = new Database(dbPath);

        this.db.serialize(() => {
            this.db.run(`
                CREATE TABLE IF NOT EXISTS associations (
                    id TEXT PRIMARY KEY,
                    source_file TEXT NOT NULL,
                    target_file TEXT NOT NULL,
                    source_range TEXT,
                    target_range TEXT,
                    type TEXT NOT NULL,
                    description TEXT,
                    last_sync_time INTEGER,
                    is_active BOOLEAN DEFAULT 1,
                    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
                    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
                )
            `);
        });
    }

    public async createAssociation(
        sourceFile: string,
        targetFile: string,
        options: Partial<Association> = {}
    ): Promise<Association> {
        const id = this.generateAssociationId(sourceFile, targetFile);
      
        const association: Association = {
            id,
            sourceFile,
            targetFile,
            type: options.type || 'manual',
            description: options.description,
            sourceRange: options.sourceRange,
            targetRange: options.targetRange,
            lastSyncTime: Date.now(),
            isActive: true,
            ...options
        };

        // 保存到数据库
        await this.saveAssociation(association);
      
        // 缓存到内存
        this.associations.set(id, association);

        // 通知界面更新
        this.notifyAssociationChanged('created', association);

        return association;
    }

    public async getAssociationsByFile(filePath: string): Promise<Association[]> {
        return Array.from(this.associations.values()).filter(
            assoc => assoc.sourceFile === filePath || assoc.targetFile === filePath
        );
    }

    public async updateAssociation(id: string, updates: Partial<Association>): Promise<void> {
        const association = this.associations.get(id);
        if (!association) {
            throw new Error(`关联 ${id} 不存在`);
        }

        const updatedAssociation = { ...association, ...updates };
        this.associations.set(id, updatedAssociation);
      
        await this.saveAssociation(updatedAssociation);
        this.notifyAssociationChanged('updated', updatedAssociation);
    }

    public async deleteAssociation(id: string): Promise<void> {
        const association = this.associations.get(id);
        if (!association) return;

        this.associations.delete(id);
      
        await new Promise((resolve, reject) => {
            this.db.run('DELETE FROM associations WHERE id = ?', [id], (err) => {
                if (err) reject(err);
                else resolve(void 0);
            });
        });

        this.notifyAssociationChanged('deleted', association);
    }

    public async analyzeFileForAssociations(filePath: string): Promise<Association[]> {
        // 使用AST分析代码结构，识别潜在的关联
        const suggestions: Association[] = [];
      
        try {
            const document = await vscode.workspace.openTextDocument(filePath);
            const content = document.getText();
          
            // 简单的注释分析（实际实现中可以使用更sophisticated的AST解析）
            const docCommentRegex = /\/\*\*([\s\S]*?)\*\//g;
            let match;
          
            while ((match = docCommentRegex.exec(content)) !== null) {
                const commentContent = match[1];
                const docLinkMatch = commentContent.match(/@doc\s+([^\s]+)/);
              
                if (docLinkMatch) {
                    const targetDoc = path.resolve(path.dirname(filePath), docLinkMatch[1]);
                    const startPos = document.positionAt(match.index);
                  
                    suggestions.push({
                        id: this.generateAssociationId(filePath, targetDoc),
                        sourceFile: filePath,
                        targetFile: targetDoc,
                        type: 'function',
                        sourceRange: new vscode.Range(startPos, startPos),
                        lastSyncTime: Date.now(),
                        isActive: true
                    });
                }
            }
        } catch (error) {
            console.error('文件分析失败:', error);
        }

        return suggestions;
    }

    private async saveAssociation(association: Association): Promise<void> {
        return new Promise((resolve, reject) => {
            this.db.run(
                `INSERT OR REPLACE INTO associations 
                 (id, source_file, target_file, source_range, target_range, 
                  type, description, last_sync_time, is_active) 
                 VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?)`,
                [
                    association.id,
                    association.sourceFile,
                    association.targetFile,
                    association.sourceRange ? JSON.stringify(association.sourceRange) : null,
                    association.targetRange ? JSON.stringify(association.targetRange) : null,
                    association.type,
                    association.description,
                    association.lastSyncTime,
                    association.isActive ? 1 : 0
                ],
                (err) => {
                    if (err) reject(err);
                    else resolve();
                }
            );
        });
    }

    private async loadAssociations(): Promise<void> {
        return new Promise((resolve, reject) => {
            this.db.all('SELECT * FROM associations WHERE is_active = 1', (err, rows: any[]) => {
                if (err) {
                    reject(err);
                    return;
                }

                rows.forEach(row => {
                    const association: Association = {
                        id: row.id,
                        sourceFile: row.source_file,
                        targetFile: row.target_file,
                        sourceRange: row.source_range ? JSON.parse(row.source_range) : undefined,
                        targetRange: row.target_range ? JSON.parse(row.target_range) : undefined,
                        type: row.type,
                        description: row.description,
                        lastSyncTime: row.last_sync_time,
                        isActive: row.is_active === 1
                    };
                  
                    this.associations.set(association.id, association);
                });

                resolve();
            });
        });
    }

    private generateAssociationId(sourceFile: string, targetFile: string): string {
        const hash = require('crypto')
            .createHash('md5')
            .update(`${sourceFile}:${targetFile}`)
            .digest('hex');
        return `assoc_${hash.substring(0, 8)}`;
    }

    private notifyAssociationChanged(action: 'created' | 'updated' | 'deleted', association: Association) {
        // 通知其他组件关联关系发生变化
        vscode.commands.executeCommand('setContext', 'dualSpace.associationsCount', this.associations.size);
      
        // 可以添加事件发射器来通知其他组件
    }
}
```

### 4. AI助手服务

```typescript
// src/services/AIService.ts
import * as vscode from 'vscode';

export interface AIResponse {
    content: string;
    confidence: number;
    suggestions?: string[];
}

export interface DocumentationRequest {
    code: string;
    language: string;
    context?: string;
    type: 'function' | 'class' | 'module';
}

export class AIService {
    private config: vscode.WorkspaceConfiguration;

    constructor() {
        this.config = vscode.workspace.getConfiguration('dualSpace.ai');
    }

    public async generateDocumentation(request: DocumentationRequest): Promise<AIResponse> {
        const provider = this.config.get<string>('provider', 'openai');
      
        switch (provider) {
            case 'openai':
                return this.generateWithOpenAI(request);
            case 'claude':
                return this.generateWithClaude(request);
            case 'local':
                return this.generateWithLocal(request);
            default:
                throw new Error(`不支持的AI提供商: ${provider}`);
        }
    }

    public async analyzeCodeForDocumentation(code: string, language: string): Promise<string[]> {
        // 分析代码结构，返回建议的文档章节
        const suggestions = [];
      
        // 基于语言类型的分析
        switch (language) {
            case 'typescript':
            case 'javascript':
                suggestions.push(...this.analyzeJSCode(code));
                break;
            case 'python':
                suggestions.push(...this.analyzePythonCode(code));
                break;
            default:
                suggestions.push('函数说明', '参数说明', '返回值', '使用示例');
        }

        return suggestions;
    }

    private async generateWithOpenAI(request: DocumentationRequest): Promise<AIResponse> {
        const apiKey = this.config.get<string>('openai.apiKey');
        if (!apiKey) {
            throw new Error('OpenAI API密钥未配置');
        }

        const prompt = this.buildDocumentationPrompt(request);
      
        try {
            const response = await fetch('https://api.openai.com/v1/chat/completions', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${apiKey}`
                },
                body: JSON.stringify({
                    model: 'gpt-3.5-turbo',
                    messages: [
                        {
                            role: 'system',
                            content: '你是一个专业的技术文档编写助手，请为提供的代码生成清晰、详细的文档。'
                        },
                        {
                            role: 'user',
                            content: prompt
                        }
                    ],
                    max_tokens: 1000,
                    temperature: 0.7
                })
            });

            const data = await response.json();
          
            if (!response.ok) {
                throw new Error(`OpenAI API错误: ${data.error?.message || '未知错误'}`);
            }

            return {
                content: data.choices[0].message.content,
                confidence: 0.9,
                suggestions: this.extractSuggestions(data.choices[0].message.content)
            };
        } catch (error) {
            throw new Error(`生成文档失败: ${error}`);
        }
    }

    private async generateWithClaude(request: DocumentationRequest): Promise<AIResponse> {
        // Claude API 实现
        const apiKey = this.config.get<string>('claude.apiKey');
        if (!apiKey) {
            throw new Error('Claude API密钥未配置');
        }

        // 实现 Claude API 调用逻辑
        return {
            content: '# Claude 生成的文档\n\n这是一个示例文档...',
            confidence: 0.85
        };
    }

    private async generateWithLocal(request: DocumentationRequest): Promise<AIResponse> {
        // 本地模型实现（可以集成Ollama等）
        return {
            content: '# 本地生成的文档\n\n这是一个示例文档...',
            confidence: 0.75
        };
    }

    private buildDocumentationPrompt(request: DocumentationRequest): string {
        return `
请为以下${request.language}代码生成专业的技术文档：

代码类型: ${request.type}
${request.context ? `上下文: ${request.context}` : ''}

代码内容:
\`\`\`${request.language}
${request.code}
\`\`\`

请生成包含以下内容的Markdown格式文档：
1. 功能概述
2. 参数说明（如果有）
3. 返回值说明（如果有）
4. 使用示例
5. 注意事项（如果有）

请确保文档清晰、准确、易于理解。
        `.trim();
    }

    private analyzeJSCode(code: string): string[] {
        const suggestions = ['功能描述', '参数'];
      
        // 检查是否有返回值
        if (code.includes('return')) {
            suggestions.push('返回值');
        }
      
        // 检查是否是async函数
        if (code.includes('async') || code.includes('await')) {
            suggestions.push('异步说明');
        }
      
        // 检查是否有异常处理
        if (code.includes('throw') || code.includes('catch')) {
            suggestions.push('异常处理');
        }
      
        suggestions.push('使用示例');
        return suggestions;
    }

    private analyzePythonCode(code: string): string[] {
        const suggestions = ['功能描述', '参数'];
      
        if (code.includes('return')) {
            suggestions.push('返回值');
        }
      
        if (code.includes('raise') || code.includes('except')) {
            suggestions.push('异常说明');
        }
      
        if (code.includes('yield')) {
            suggestions.push('生成器说明');
        }
      
        suggestions.push('使用示例');
        return suggestions;
    }

    private extractSuggestions(content: string): string[] {
        // 从生成的内容中提取建议的改进点
        const suggestions = [];
      
        if (content.includes('TODO') || content.includes('待完善')) {
            suggestions.push('文档需要进一步完善');
        }
      
        if (content.length < 100) {
            suggestions.push('建议添加更详细的说明');
        }
      
        return suggestions;
    }
}
```

### 5. 前端主界面JavaScript

```javascript
// media/main.js
(function() {
    const vscode = acquireVsCodeApi();
  
    // 应用状态
    let currentAssociations = [];
    let currentCodeFile = null;
    let currentDocFile = null;
  
    // DOM元素
    const workspace = document.getElementById('workspace');
    const codeSpace = document.getElementById('codeSpace');
    const docSpace = document.getElementById('docSpace');
    const splitter = document.getElementById('splitter');
    const spaceRatio = document.getElementById('spaceRatio');
  
    // 初始化
    initialize();
  
    function initialize() {
        setupSplitter();
        setupEventListeners();
        setupEditors();
        loadInitialData();
    }
  
    // 设置分割器拖拽功能
    function setupSplitter() {
        let isResizing = false;
      
        splitter.addEventListener('mousedown', (e) => {
            isResizing = true;
            document.body.style.cursor = 'col-resize';
            document.addEventListener('mousemove', handleMouseMove);
            document.addEventListener('mouseup', handleMouseUp);
        });
      
        function handleMouseMove(e) {
            if (!isResizing) return;
          
            const containerRect = workspace.getBoundingClientRect();
            const percentage = ((e.clientX - containerRect.left) / containerRect.width) * 100;
          
            if (percentage >= 20 && percentage <= 80) {
                codeSpace.style.width = `${percentage}%`;
                docSpace.style.width = `${100 - percentage}%`;
                spaceRatio.value = percentage;
            }
        }
      
        function handleMouseUp() {
            isResizing = false;
            document.body.style.cursor = '';
            document.removeEventListener('mousemove', handleMouseMove);
            document.removeEventListener('mouseup', handleMouseUp);
        }
      
        // 比例滑块控制
        spaceRatio.addEventListener('input', (e) => {
            const ratio = parseInt(e.target.value);
            codeSpace.style.width = `${ratio}%`;
            docSpace.style.width = `${100 - ratio}%`;
        });
    }
  
    // 设置事件监听器
    function setupEventListeners() {
        // 新建关联按钮
        document.getElementById('newAssociation').addEventListener('click', () => {
            showCreateAssociationDialog();
        });
      
        // 同步全部按钮
        document.getElementById('syncAll').addEventListener('click', () => {
            syncAllAssociations();
        });
      
        // 编辑模式切换
        document.getElementById('editMode').addEventListener('click', () => {
            switchToEditMode();
        });
      
        document.getElementById('previewMode').addEventListener('click', () => {
            switchToPreviewMode();
        });
      
        // 在编辑器中打开
        document.getElementById('openInEditor').addEventListener('click', () => {
            if (currentCodeFile) {
                openInExternalEditor(currentCodeFile);
            }
        });
      
        // 设置按钮
        document.getElementById('openSettings').addEventListener('click', () => {
            openSettings();
        });
      
        // 监听来自扩展的消息
        window.addEventListener('message', (event) => {
            handleMessage(event.data);
        });
    }
  
    // 设置代码编辑器
    function setupEditors() {
        // 这里可以集成Monaco Editor或CodeMirror
        // 为了简化，使用textarea作为示例
        const codeEditor = document.createElement('textarea');
        codeEditor.className = 'editor-textarea';
        codeEditor.placeholder = '在此处显示代码...';
        document.getElementById('codeEditor').appendChild(codeEditor);
      
        const docEditor = document.createElement('textarea');
        docEditor.className = 'editor-textarea';
        docEditor.placeholder = '在此处编辑文档...';
        document.getElementById('docEditor').appendChild(docEditor);
      
        // 代码编辑器变更监听
        codeEditor.addEventListener('input', debounce((e) => {
            onCodeChanged(e.target.value);
        }, 500));
      
        // 文档编辑器变更监听
        docEditor.addEventListener('input', debounce((e) => {
            onDocChanged(e.target.value);
            updatePreview(e.target.value);
        }, 300));
    }
  
    // 加载初始数据
    function loadInitialData() {
        vscode.postMessage({
            command: 'getActiveFile'
        });
      
        vscode.postMessage({
            command: 'getAssociations'
        });
    }
  
    // 处理来自扩展的消息
    function handleMessage(message) {
        switch (message.command) {
            case 'fileLoaded':
                handleFileLoaded(message);
                break;
            case 'associationsUpdated':
                handleAssociationsUpdated(message.associations);
                break;
            case 'syncStatus':
                updateSyncStatus(message.status);
                break;
            case 'aiResponse':
                handleAIResponse(message.response);
                break;
        }
    }
  
    // 文件加载处理
    function handleFileLoaded(data) {
        if (data.type === 'code') {
            currentCodeFile = data.filePath;
            document.getElementById('codeFilePath').textContent = getFileDisplayName(data.filePath);
            document.querySelector('#codeEditor .editor-textarea').value = data.content;
            highlightAssociations('code', data.filePath);
        } else if (data.type === 'doc') {
            currentDocFile = data.filePath;
            document.getElementById('docFilePath').textContent = getFileDisplayName(data.filePath);
            document.querySelector('#docEditor .editor-textarea').value = data.content;
            updatePreview(data.content);
        }
    }
  
    // 关联关系更新处理
    function handleAssociationsUpdated(associations) {
        currentAssociations = associations;
        updateAssociationIndicators();
        updateAssociationCount();
    }
  
    // 创建关联对话框
    function showCreateAssociationDialog() {
        const dialog = document.createElement('div');
        dialog.className = 'modal-overlay';
        dialog.innerHTML = `
            <div class="modal-content">
                <div class="modal-header">
                    <h3>创建新关联</h3>
                    <button class="close-btn">&times;</button>
                </div>
                <div class="modal-body">
                    <div class="form-group">
                        <label>源文件:</label>
                        <input type="text" id="sourceFile" placeholder="选择代码文件">
                        <button type="button" id="browseSource">浏览...</button>
                    </div>
                    <div class="form-group">
                        <label>目标文档:</label>
                        <input type="text" id="targetFile" placeholder="选择文档文件">
                        <button type="button" id="browseTarget">浏览...</button>
                    </div>
                    <div class="form-group">
                        <label>关联类型:</label>
                        <select id="associationType">
                            <option value="function">函数</option>
                            <option value="class">类</option>
                            <option value="module">模块</option>
                            <option value="manual">手动</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label>描述:</label>
                        <textarea id="associationDesc" placeholder="可选的描述信息"></textarea>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" id="createAssocBtn" class="btn btn-primary">创建关联</button>
                    <button type="button" id="cancelBtn" class="btn btn-secondary">取消</button>
                </div>
            </div>
        `;
      
        document.body.appendChild(dialog);
      
        // 事件监听
        dialog.querySelector('.close-btn').addEventListener('click', () => {
            document.body.removeChild(dialog);
        });
      
        dialog.querySelector('#cancelBtn').addEventListener('click', () => {
            document.body.removeChild(dialog);
        });
      
        dialog.querySelector('#createAssocBtn').addEventListener('click', () => {
            const sourceFile = dialog.querySelector('#sourceFile').value;
            const targetFile = dialog.querySelector('#targetFile').value;
            const type = dialog.querySelector('#associationType').value;
            const description = dialog.querySelector('#associationDesc').value;
          
            if (sourceFile && targetFile) {
                vscode.postMessage({
                    command: 'createAssociation',
                    sourceFile,
                    targetFile,
                    type,
                    description
                });
                document.body.removeChild(dialog);
            } else {
                alert('请选择源文件和目标文档');
            }
        });
    }
  
    // 代码变更处理
    function onCodeChanged(content) {
        if (currentCodeFile) {
            vscode.postMessage({
                command: 'saveFile',
                filePath: currentCodeFile,
                content: content
            });
          
            // 触发智能同步检测
            checkForSyncNeeds();
        }
    }
  
    // 文档变更处理
    function onDocChanged(content) {
        if (currentDocFile) {
            vscode.postMessage({
                command: 'saveFile',
                filePath: currentDocFile,
                content: content
            });
        }
    }
  
    // 更新预览
    function updatePreview(markdown) {
        const previewContainer = document.getElementById('docPreview');
        // 这里应该使用markdown解析器，简化起见直接显示
        previewContainer.innerHTML = `<div class="markdown-content">${markdown}</div>`;
    }
  
    // 切换到编辑模式
    function switchToEditMode() {
        document.getElementById('docEditor').style.display = 'block';
        document.getElementById('docPreview').style.display = 'none';
        document.getElementById('editMode').classList.add('active');
        document.getElementById('previewMode').classList.remove('active');
    }
  
    // 切换到预览模式
    function switchToPreviewMode() {
        document.getElementById('docEditor').style.display = 'none';
        document.getElementById('docPreview').style.display = 'block';
        document.getElementById('previewMode').classList.add('active');
        document.getElementById('editMode').classList.remove('active');
    }
  
    // 更新关联指示器
    function updateAssociationIndicators() {
        const codeIndicators = document.getElementById('codeAssociations');
        const fileAssociations = currentAssociations.filter(
            assoc => assoc.sourceFile === currentCodeFile
        );
      
        codeIndicators.innerHTML = '';
        fileAssociations.forEach(assoc => {
            const indicator = document.createElement('div');
            indicator.className = 'association-indicator';
            indicator.innerHTML = `
                <i class="icon-link"></i>
                <span>${getFileDisplayName(assoc.targetFile)}</span>
                <button class="goto-btn" onclick="gotoAssociation('${assoc.id}')">
                    <i class="icon-arrow-right"></i>
                </button>
            `;
            codeIndicators.appendChild(indicator);
        });
    }
  
    // 更新关联计数
    function updateAssociationCount() {
        document.getElementById('associationCount').textContent = 
            `关联数: ${currentAssociations.length}`;
    }
  
    // 同步状态更新
    function updateSyncStatus(status) {
        const syncStatus = document.getElementById('syncStatus');
        const icon = syncStatus.querySelector('i');
        const text = syncStatus.querySelector('span');
      
        switch (status) {
            case 'synced':
                icon.className = 'icon-check-circle text-success';
                text.textContent = '已同步';
                break;
            case 'syncing':
                icon.className = 'icon-sync text-info spinning';
                text.textContent = '同步中...';
                break;
            case 'needs-sync':
                icon.className = 'icon-alert-circle text-warning';
                text.textContent = '需要同步';
                break;
            case 'error':
                icon.className = 'icon-x-circle text-error';
                text.textContent = '同步失败';
                break;
        }
    }
  
    // 检查同步需求
    function checkForSyncNeeds() {
        vscode.postMessage({
            command: 'checkSyncNeeds',
            filePath: currentCodeFile
        });
    }
  
    // 同步所有关联
    function syncAllAssociations() {
        showLoading('正在同步所有关联...');
        vscode.postMessage({
            command: 'syncAll'
        });
    }
  
    // 显示加载状态
    function showLoading(message) {
        const overlay = document.getElementById('loadingOverlay');
        overlay.querySelector('.loading-text').textContent = message;
        overlay.style.display = 'flex';
    }
  
    // 隐藏加载状态
    function hideLoading() {
        document.getElementById('loadingOverlay').style.display = 'none';
    }
  
    // 工具函数
    function debounce(func, wait) {
        let timeout;
        return function executedFunction(...args) {
            const later = () => {
                clearTimeout(timeout);
                func(...args);
            };
            clearTimeout(timeout);
            timeout = setTimeout(later, wait);
        };
    }
  
    function getFileDisplayName(filePath) {
        return filePath.split('/').pop() || filePath;
    }
  
    // 全局函数供HTML使用
    window.gotoAssociation = function(associationId) {
        const association = currentAssociations.find(a => a.id === associationId);
        if (association) {
            vscode.postMessage({
                command: 'loadFile',
                filePath: association.targetFile,
                type: 'doc'
            });
        }
    };
  
    window.openInExternalEditor = function(filePath) {
        vscode.postMessage({
            command: 'openInEditor',
            filePath: filePath
        });
    };
  
    window.openSettings = function() {
        vscode.postMessage({
            command: 'openSettings'
        });
    };
})();
```

### 6. CSS样式文件

```css
/* media/style.css */
:root {
    --primary-color: #007ACC;
    --secondary-color: #00BCF2;
    --success-color: #00A86B;
    --warning-color: #FF9500;
    --error-color: #F44336;
  
    --bg-primary: var(--vscode-editor-background);
    --bg-secondary: var(--vscode-sideBar-background);
    --bg-tertiary: var(--vscode-panel-background);
  
    --text-primary: var(--vscode-editor-foreground);
    --text-secondary: var(--vscode-descriptionForeground);
    --text-muted: var(--vscode-disabledForeground);
  
    --border-color: var(--vscode-panel-border);
    --hover-bg: var(--vscode-list-hoverBackground);
  
    --font-family: var(--vscode-font-family);
    --font-size: var(--vscode-font-size);
    --font-weight: var(--vscode-font-weight);
}

* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    font-family: var(--font-family);
    font-size: var(--font-size);
    color: var(--text-primary);
    background-color: var(--bg-primary);
    overflow: hidden;
}

/* 主容器样式 */
.dual-space-container {
    height: 100vh;
    display: flex;
    flex-direction: column;
}

/* 工具栏样式 */
.toolbar {
    height: 40px;
    background: var(--bg-secondary);
    border-bottom: 1px solid var(--border-color);
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 12px;
}

.toolbar-left {
    display: flex;
    gap: 8px;
}

.toolbar-right {
    display: flex;
    align-items: center;
    gap: 12px;
}

/* 按钮样式 */
.btn {
    padding: 6px 12px;
    border: none;
    border-radius: 4px;
    font-size: 13px;
    cursor: pointer;
    display: inline-flex;
    align-items: center;
    gap: 6px;
    transition: background-color 0.15s ease;
}

.btn-primary {
    background: var(--primary-color);
    color: white;
}

.btn-primary:hover {
    background: #005a9e;
}

.btn-secondary {
    background: var(--bg-tertiary);
    color: var(--text-primary);
    border: 1px solid var(--border-color);
}

.btn-secondary:hover {
    background: var(--hover-bg);
}

.btn-icon {
    width: 28px;
    height: 28px;
    padding: 0;
    background: transparent;
    border: none;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 4px;
    color: var(--text-secondary);
}

.btn-icon:hover {
    background: var(--hover-bg);
    color: var(--text-primary);
}

.btn-link {
    background: transparent;
    border: none;
    color: var(--primary-color);
    cursor: pointer;
    text-decoration: none;
    font-size: 12px;
}

.btn-link:hover {
    text-decoration: underline;
}

/* 空间比例控制 */
.space-ratio-control {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 12px;
    color: var(--text-secondary);
}

.space-ratio-control input[type="range"] {
    width: 100px;
}

/* 工作区样式 */
.workspace {
    flex: 1;
    display: flex;
    overflow: hidden;
}

.code-space, .doc-space {
    display: flex;
    flex-direction: column;
    overflow: hidden;
}

.code-space {
    width: 50%;
    border-right: 1px solid var(--border-color);
}

.doc-space {
    width: 50%;
}

/* 分割器样式 */
.splitter {
    width: 2px;
    background: var(--border-color);
    cursor: col-resize;
    position: relative;
}

.splitter:hover {
    background: var(--primary-color);
}

/* 空间头部 */
.space-header {
    height: 35px;
    background: var(--bg-secondary);
    border-bottom: 1px solid var(--border-color);
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 12px;
}

.space-title {
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 13px;
    font-weight: 500;
}

.file-path {
    font-size: 11px;
    color: var(--text-secondary);
    margin-left: 8px;
}

.space-actions {
    display: flex;
    gap: 4px;
}

/* 空间内容 */
.space-content {
    flex: 1;
    position: relative;
    overflow: hidden;
}

.editor-container {
    height: 100%;
    position: relative;
}

.editor-textarea {
    width: 100%;
    height: 100%;
    border: none;
    resize: none;
    outline: none;
    font-family: 'Fira Code', Monaco, Menlo, monospace;
    font-size: 14px;
    line-height: 1.5;
    background: var(--bg-primary);
    color: var(--text-primary);
    padding: 12px;
}

.preview-container {
    height: 100%;
    overflow-y: auto;
    padding: 16px;
    background: var(--bg-primary);
}

/* 关联指示器 */
.association-indicators {
    position: absolute;
    top: 0;
    right: 0;
    width: 200px;
    padding: 8px;
    background: rgba(var(--bg-secondary), 0.95);
    border-left: 1px solid var(--border-color);
    overflow-y: auto;
    max-height: 100%;
}

.association-indicator {
    display: flex;
    align-items: center;
    gap: 6px;
    padding: 4px 8px;
    margin-bottom: 4px;
    background: var(--bg-tertiary);
    border-radius: 4px;
    font-size: 12px;
    border: 1px solid var(--border-color);
}

.association-indicator:hover {
    background: var(--hover-bg);
}

.goto-btn {
    margin-left: auto;
    width: 20px;
    height: 20px;
    border: none;
    background: transparent;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--text-secondary);
    border-radius: 3px;
}

.goto-btn:hover {
    background: var(--hover-bg);
    color: var(--text-primary);
}

/* 状态栏样式 */
.status-bar {
    height: 24px;
    background: var(--bg-secondary);
    border-top: 1px solid var(--border-color);
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 12px;
    font-size: 12px;
}

.status-left {
    display: flex;
    gap: 16px;
}

.sync-status, .association-count {
    display: flex;
    align-items: center;
    gap: 4px;
}

.status-right {
    display: flex;
    gap: 8px;
}

/* 模态框样式 */
.modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
}

.modal-content {
    background: var(--bg-primary);
    border: 1px solid var(--border-color);
    border-radius: 6px;
    width: 500px;
    max-width: 90vw;
    max-height: 90vh;
    overflow-y: auto;
}

.modal-header {
    padding: 16px 20px;
    border-bottom: 1px solid var(--border-color);
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.modal-header h3 {
    font-size: 16px;
    font-weight: 600;
}

.close-btn {
    background: none;
    border: none;
    font-size: 20px;
    cursor: pointer;
    color: var(--text-secondary);
    width: 24px;
    height: 24px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 4px;
}

.close-btn:hover {
    background: var(--hover-bg);
    color: var(--text-primary);
}

.modal-body {
    padding: 20px;
}

.modal-footer {
    padding: 16px 20px;
    border-top: 1px solid var(--border-color);
    display: flex;
    gap: 8px;
    justify-content: flex-end;
}

/* 表单样式 */
.form-group {
    margin-bottom: 16px;
}

.form-group label {
    display: block;
    margin-bottom: 6px;
    font-size: 13px;
    font-weight: 500;
    color: var(--text-primary);
}

.form-group input,
.form-group select,
.form-group textarea {
    width: 100%;
    padding: 8px 12px;
    border: 1px solid var(--border-color);
    border-radius: 3px;
    background: var(--bg-primary);
    color: var(--text-primary);
    font-size: 13px;
    outline: none;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
    border-color: var(--primary-color);
}

.form-group textarea {
    resize: vertical;
    min-height: 80px;
}

/* 加载遮罩 */
.loading-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.7);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    z-index: 2000;
    color: white;
}

.loading-spinner {
    width: 40px;
    height: 40px;
    border: 4px solid rgba(255, 255, 255, 0.3);
    border-top: 4px solid white;
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin-bottom: 16px;
}

.loading-text {
    font-size: 14px;
}

/* 图标样式 */
[class^="icon-"] {
    font-style: normal;
    width: 16px;
    height: 16px;
    display: inline-block;
}

/* 状态颜色 */
.text-success { color: var(--success-color); }
.text-warning { color: var(--warning-color); }
.text-error { color: var(--error-color); }
.text-info { color: var(--secondary-color); }

/* 动画 */
@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

.spinning {
    animation: spin 1s linear infinite;
}

/* 主题适配 */
.vscode-light {
    --bg-primary: #ffffff;
    --bg-secondary: #f8f8f8;
    --bg-tertiary: #f0f0f0;
    --text-primary: #333333;
    --text-secondary: #666666;
    --border-color: #e1e1e1;
    --hover-bg: #e8e8e8;
}

.vscode-dark {
    --bg-primary: #1e1e1e;
    --bg-secondary: #252526;
    --bg-tertiary: #2d2d30;
    --text-primary: #cccccc;
    --text-secondary: #969696;
    --border-color: #464647;
    --hover-bg: #2a2d2e;
}

.vscode-high-contrast {
    --border-color: #ffffff;
    --hover-bg: rgba(255, 255, 255, 0.1);
}

/* 响应式设计 */
@media (max-width: 768px) {
    .workspace {
        flex-direction: column;
    }
  
    .code-space, .doc-space {
        width: 100%;
        height: 50%;
    }
  
    .splitter {
        width: 100%;
        height: 2px;
        cursor: row-resize;
    }
  
    .association-indicators {
        position: static;
        width: 100%;
        max-height: 150px;
    }
}
```

### 7. 包配置文件

```json
// package.json
{
  "name": "dual-space-manager",
  "displayName": "双空间智能管理",
  "description": "AI驱动的IDE代码文档双空间智能管理插件",
  "version": "1.0.0",
  "publisher": "dual-space",
  "engines": {
    "vscode": "^1.60.0"
  },
  "categories": [
    "Other",
    "Documentation",
    "Machine Learning"
  ],
  "keywords": [
    "documentation",
    "ai",
    "code-docs",
    "productivity",
    "intelligent"
  ],
  "main": "./out/extension.js",
  "contributes": {
    "commands": [
      {
        "command": "dualSpace.createAssociation",
        "title": "创建代码文档关联",
        "category": "双空间"
      },
      {
        "command": "dualSpace.syncAll",
        "title": "同步所有关联",
        "category": "双空间"
      },
      {
        "command": "dualSpace.openSettings",
        "title": "打开设置",
        "category": "双空间"
      },
      {
        "command": "dualSpace.generateDoc",
        "title": "AI生成文档",
        "category": "双空间"
      }
    ],
    "views": {
      "explorer": [
        {
          "id": "associationExplorer",
          "name": "关联管理",
          "when": "workspaceFolderCount > 0"
        }
      ],
      "panel": [
        {
          "id": "aiAssistant",
          "name": "AI助手",
          "when": "workspaceFolderCount > 0"
        }
      ]
    },
    "viewsWelcome": [
      {
        "view": "associationExplorer",
        "contents": "尚未发现任何代码文档关联。\n[扫描项目关联](command:dualSpace.scanProject)\n[手动创建关联](command:dualSpace.createAssociation)"
      }
    ],
    "menus": {
      "editor/context": [
        {
          "command": "dualSpace.generateDoc",
          "when": "editorHasSelection",
          "group": "1_modification"
        },
        {
          "command": "dualSpace.createAssociation",
          "when": "resourceExtname == .js || resourceExtname == .ts || resourceExtname == .py",
          "group": "dualSpace"
        }
      ],
      "explorer/context": [
        {
          "command": "dualSpace.createAssociation",
          "when": "resourceExtname == .md || resourceExtname == .txt",
          "group": "dualSpace"
        }
      ]
    },
    "configuration": {
      "title": "双空间管理",
      "properties": {
        "dualSpace.ai.provider": {
          "type": "string",
          "enum": ["openai", "claude", "local"],
          "default": "openai",
          "description": "AI服务提供商"
        },
        "dualSpace.ai.openai.apiKey": {
          "type": "string",
          "default": "",
          "description": "OpenAI API密钥"
        },
        "dualSpace.ai.claude.apiKey": {
          "type": "string",
          "default": "",
          "description": "Claude API密钥"
        },
        "dualSpace.sync.mode": {
          "type": "string",
          "enum": ["auto", "prompt", "manual"],
          "default": "prompt",
          "description": "同步模式"
        },
        "dualSpace.sync.delay": {
          "type": "number",
          "default": 2000,
          "minimum": 500,
          "maximum": 10000,
          "description": "自动同步延迟（毫秒）"
        },
        "dualSpace.ui.spaceRatio": {
          "type": "number",
          "default": 50,
          "minimum": 20,
          "maximum": 80,
          "description": "默认空间比例"
        }
      }
    },
    "activationEvents": [
      "onStartupFinished"
    ]
  },
  "scripts": {
    "vscode:prepublish": "npm run compile",
    "compile": "tsc -p ./",
    "watch": "tsc -watch -p ./",
    "pretest": "npm run compile && npm run lint",
    "lint": "eslint src --ext ts",
    "test": "node ./out/test/runTest.js"
  },
  "devDependencies": {
    "@types/vscode": "^1.60.0",
    "@types/node": "16.x",
    "@typescript-eslint/eslint-plugin": "^5.31.0",
    "@typescript-eslint/parser": "^5.31.0",
    "eslint": "^8.20.0",
    "typescript": "^4.7.4"
  },
  "dependencies": {
    "sqlite3": "^5.1.6",
    "marked": "^4.3.0"
  }
}
```

这个完整的插件架构提供了：

## 核心功能特性

1. **双空间管理**：可视化的代码文档双空间界面
2. **智能关联**：自动识别和手动创建代码文档关联
3. **AI辅助**：集成多种AI服务生成文档
4. **实时同步**：代码变更时智能同步相关文档
5. **扩展兼容**：完全集成VS Code生态系统

## 技术亮点

- **TypeScript开发**：类型安全，维护性强
- **模块化架构**：核心功能分离，易于扩展
- **数据持久化**：SQLite本地存储关联关系
- **响应式界面**：适配不同屏幕尺寸
- **主题适配**：完美融入VS Code主题系统

### 8. 同步协调器实现

```typescript
// src/core/SyncCoordinator.ts
import * as vscode from 'vscode';
import { AssociationManager, Association } from './AssociationManager';
import { AIService } from '../services/AIService';

export interface SyncTask {
    id: string;
    associationId: string;
    type: 'code-to-doc' | 'doc-to-code';
    priority: 'high' | 'medium' | 'low';
    status: 'pending' | 'processing' | 'completed' | 'failed';
    createdAt: number;
    completedAt?: number;
    error?: string;
}

export class SyncCoordinator {
    private syncTasks: Map<string, SyncTask> = new Map();
    private aiService: AIService;
    private syncQueue: SyncTask[] = [];
    private isProcessing = false;
    private config: vscode.WorkspaceConfiguration;

    constructor(private associationManager: AssociationManager) {
        this.aiService = new AIService();
        this.config = vscode.workspace.getConfiguration('dualSpace.sync');
        this.setupFileWatcher();
    }

    public async syncAll(): Promise<void> {
        const associations = await this.associationManager.getAllAssociations();
        const syncPromises = associations.map(assoc => this.syncAssociation(assoc.id));
        
        await Promise.allSettled(syncPromises);
        vscode.window.showInformationMessage(`完成同步 ${associations.length} 个关联`);
    }

    public async syncAssociation(associationId: string): Promise<void> {
        const association = await this.associationManager.getAssociation(associationId);
        if (!association) {
            throw new Error(`关联 ${associationId} 不存在`);
        }

        const syncTask = this.createSyncTask(association);
        this.addToQueue(syncTask);
        
        if (!this.isProcessing) {
            this.processQueue();
        }
    }

    public async checkSyncNeeded(filePath: string): Promise<boolean> {
        const associations = await this.associationManager.getAssociationsByFile(filePath);
        const fileStats = await vscode.workspace.fs.stat(vscode.Uri.file(filePath));
        
        return associations.some(assoc => {
            return assoc.lastSyncTime < fileStats.mtime;
        });
    }

    private setupFileWatcher(): void {
        const watcher = vscode.workspace.createFileSystemWatcher('**/*');
        
        watcher.onDidChange(async (uri) => {
            await this.handleFileChange(uri.fsPath);
        });

        watcher.onDidCreate(async (uri) => {
            await this.handleFileCreate(uri.fsPath);
        });

        watcher.onDidDelete(async (uri) => {
            await this.handleFileDelete(uri.fsPath);
        });
    }

    private async handleFileChange(filePath: string): Promise<void> {
        const syncMode = this.config.get<string>('mode', 'prompt');
        const associations = await this.associationManager.getAssociationsByFile(filePath);
        
        if (associations.length === 0) return;

        switch (syncMode) {
            case 'auto':
                // 自动同步
                const delay = this.config.get<number>('delay', 2000);
                setTimeout(() => {
                    associations.forEach(assoc => this.syncAssociation(assoc.id));
                }, delay);
                break;
                
            case 'prompt':
                // 提示用户
                this.showSyncPrompt(associations, filePath);
                break;
                
            case 'manual':
                // 仅标记需要同步
                this.markAsNeedsSync(associations);
                break;
        }
    }

    private async showSyncPrompt(associations: Association[], filePath: string): Promise<void> {
        const fileName = filePath.split('/').pop();
        const associationCount = associations.length;
        
        const action = await vscode.window.showInformationMessage(
            `文件 "${fileName}" 已更改，发现 ${associationCount} 个相关关联。是否同步？`,
            { modal: false },
            '立即同步',
            '稍后提醒',
            '忽略'
        );

        switch (action) {
            case '立即同步':
                associations.forEach(assoc => this.syncAssociation(assoc.id));
                break;
            case '稍后提醒':
                // 5分钟后再次提醒
                setTimeout(() => this.showSyncPrompt(associations, filePath), 300000);
                break;
            default:
                // 忽略，不做任何操作
                break;
        }
    }

    private createSyncTask(association: Association): SyncTask {
        const taskId = `sync_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
        
        return {
            id: taskId,
            associationId: association.id,
            type: this.determineSyncDirection(association),
            priority: this.calculatePriority(association),
            status: 'pending',
            createdAt: Date.now()
        };
    }

    private determineSyncDirection(association: Association): 'code-to-doc' | 'doc-to-code' {
        // 简化逻辑：通常是代码同步到文档
        // 实际实现中可以根据文件修改时间等因素判断
        return 'code-to-doc';
    }

    private calculatePriority(association: Association): 'high' | 'medium' | 'low' {
        // 根据关联类型和重要性计算优先级
        switch (association.type) {
            case 'function':
                return 'high';
            case 'class':
                return 'medium';
            default:
                return 'low';
        }
    }

    private addToQueue(task: SyncTask): void {
        this.syncTasks.set(task.id, task);
        
        // 根据优先级插入队列
        const insertIndex = this.syncQueue.findIndex(t => {
            const priorities = { 'high': 3, 'medium': 2, 'low': 1 };
            return priorities[task.priority] > priorities[t.priority];
        });

        if (insertIndex === -1) {
            this.syncQueue.push(task);
        } else {
            this.syncQueue.splice(insertIndex, 0, task);
        }

        this.notifyQueueUpdate();
    }

    private async processQueue(): Promise<void> {
        if (this.isProcessing || this.syncQueue.length === 0) return;

        this.isProcessing = true;
        
        while (this.syncQueue.length > 0) {
            const task = this.syncQueue.shift()!;
            await this.processTask(task);
        }

        this.isProcessing = false;
        this.notifyQueueUpdate();
    }

    private async processTask(task: SyncTask): Promise<void> {
        try {
            task.status = 'processing';
            this.notifyTaskUpdate(task);

            const association = await this.associationManager.getAssociation(task.associationId);
            if (!association) {
                throw new Error(`关联 ${task.associationId} 不存在`);
            }

            if (task.type === 'code-to-doc') {
                await this.syncCodeToDoc(association);
            } else {
                await this.syncDocToCode(association);
            }

            task.status = 'completed';
            task.completedAt = Date.now();
            
            // 更新关联的同步时间
            await this.associationManager.updateAssociation(association.id, {
                lastSyncTime: Date.now()
            });

        } catch (error) {
            task.status = 'failed';
            task.error = error instanceof Error ? error.message : '未知错误';
            vscode.window.showErrorMessage(`同步失败: ${task.error}`);
        }

        this.syncTasks.set(task.id, task);
        this.notifyTaskUpdate(task);
    }

    private async syncCodeToDoc(association: Association): Promise<void> {
        // 读取源代码
        const codeUri = vscode.Uri.file(association.sourceFile);
        const codeDocument = await vscode.workspace.openTextDocument(codeUri);
        const codeContent = codeDocument.getText();

        // 提取相关代码段
        const relevantCode = association.sourceRange 
            ? codeDocument.getText(association.sourceRange)
            : codeContent;

        // 使用AI生成文档
        const aiResponse = await this.aiService.generateDocumentation({
            code: relevantCode,
            language: codeDocument.languageId,
            type: association.type,
            context: association.description
        });

        // 读取现有文档
        const docUri = vscode.Uri.file(association.targetFile);
        let existingContent = '';
        
        try {
            const docDocument = await vscode.workspace.openTextDocument(docUri);
            existingContent = docDocument.getText();
        } catch {
            // 文档不存在，创建新文档
        }

        // 合并或替换文档内容
        const updatedContent = await this.mergeDocumentContent(
            existingContent,
            aiResponse.content,
            association
        );

        // 写入更新后的文档
        const encoder = new TextEncoder();
        await vscode.workspace.fs.writeFile(docUri, encoder.encode(updatedContent));

        // 如果文档在编辑器中打开，通知更新
        this.notifyDocumentUpdated(association.targetFile, updatedContent);
    }

    private async syncDocToCode(association: Association): Promise<void> {
        // 读取文档内容
        const docUri = vscode.Uri.file(association.targetFile);
        const docDocument = await vscode.workspace.openTextDocument(docUri);
        const docContent = docDocument.getText();

        // 分析文档变更，更新代码注释
        const codeUri = vscode.Uri.file(association.sourceFile);
        const codeDocument = await vscode.workspace.openTextDocument(codeUri);
        const codeContent = codeDocument.getText();

        // 提取文档中的关键信息
        const docSummary = this.extractDocumentSummary(docContent);
        
        // 更新代码注释
        const updatedCode = await this.updateCodeComments(codeContent, docSummary, association);

        // 写入更新后的代码
        const encoder = new TextEncoder();
        await vscode.workspace.fs.writeFile(codeUri, encoder.encode(updatedCode));
    }

    private async mergeDocumentContent(
        existingContent: string,
        newContent: string,
        association: Association
    ): Promise<string> {
        // 如果有目标范围，只替换指定部分
        if (association.targetRange && existingContent) {
            // 实现智能合并逻辑
            return this.smartMergeContent(existingContent, newContent, association.targetRange);
        }
        
        // 否则替换全部内容或追加
        return existingContent ? `${existingContent}\n\n${newContent}` : newContent;
    }

    private smartMergeContent(
        existingContent: string,
        newContent: string,
        targetRange: vscode.Range
    ): string {
        const lines = existingContent.split('\n');
        const beforeLines = lines.slice(0, targetRange.start.line);
        const afterLines = lines.slice(targetRange.end.line + 1);
        const newLines = newContent.split('\n');

        return [...beforeLines, ...newLines, ...afterLines].join('\n');
    }

    private extractDocumentSummary(content: string): string {
        // 提取文档的核心信息，用于更新代码注释
        const lines = content.split('\n');
        const summaryLines = lines
            .filter(line => line.trim().length > 0)
            .slice(0, 3) // 取前3行有内容的行作为摘要
            .map(line => line.replace(/^#+\s*/, '').trim());

        return summaryLines.join(' ');
    }

    private async updateCodeComments(
        codeContent: string,
        summary: string,
        association: Association
    ): Promise<string> {
        // 这里可以实现更复杂的代码注释更新逻辑
        // 简化实现：在函数前添加或更新注释
        
        if (association.sourceRange) {
            const lines = codeContent.split('\n');
            const commentLine = `// ${summary}`;
            
            // 在目标行前插入或更新注释
            if (association.sourceRange.start.line > 0) {
                const prevLine = lines[association.sourceRange.start.line - 1];
                if (prevLine.trim().startsWith('//')) {
                    // 更新现有注释
                    lines[association.sourceRange.start.line - 1] = commentLine;
                } else {
                    // 插入新注释
                    lines.splice(association.sourceRange.start.line, 0, commentLine);
                }
            }
            
            return lines.join('\n');
        }

        return codeContent;
    }

    private async handleFileCreate(filePath: string): Promise<void> {
        // 检查是否需要为新文件建立关联
        const suggestions = await this.associationManager.analyzeFileForAssociations(filePath);
        
        if (suggestions.length > 0) {
            const action = await vscode.window.showInformationMessage(
                `检测到新文件 "${filePath.split('/').pop()}"，是否创建推荐的关联？`,
                '查看建议',
                '忽略'
            );

            if (action === '查看建议') {
                this.showAssociationSuggestions(suggestions);
            }
        }
    }

    private async handleFileDelete(filePath: string): Promise<void> {
        const associations = await this.associationManager.getAssociationsByFile(filePath);
        
        if (associations.length > 0) {
            const action = await vscode.window.showWarningMessage(
                `文件 "${filePath.split('/').pop()}" 已删除，发现 ${associations.length} 个相关关联。如何处理？`,
                '删除关联',
                '保留关联',
                '取消'
            );

            if (action === '删除关联') {
                await Promise.all(
                    associations.map(assoc => this.associationManager.deleteAssociation(assoc.id))
                );
                vscode.window.showInformationMessage(`已删除 ${associations.length} 个关联`);
            }
        }
    }

    private showAssociationSuggestions(suggestions: Association[]): void {
        // 显示关联建议的快速选择面板
        const items = suggestions.map(assoc => ({
            label: `$(link) ${assoc.sourceFile.split('/').pop()} → ${assoc.targetFile.split('/').pop()}`,
            description: assoc.description || assoc.type,
            detail: `类型: ${assoc.type}`,
            association: assoc
        }));

        vscode.window.showQuickPick(items, {
            placeHolder: '选择要创建的关联',
            canPickMany: true
        }).then(selected => {
            if (selected) {
                selected.forEach(item => {
                    this.associationManager.createAssociation(
                        item.association.sourceFile,
                        item.association.targetFile,
                        item.association
                    );
                });
            }
        });
    }

    private markAsNeedsSync(associations: Association[]): void {
        // 标记关联需要同步，在状态栏显示提示
        vscode.commands.executeCommand('setContext', 'dualSpace.needsSync', true);
        
        const count = associations.length;
        vscode.window.setStatusBarMessage(
            `$(warning) ${count} 个关联需要同步`,
            5000
        );
    }

    private notifyQueueUpdate(): void {
        vscode.commands.executeCommand('setContext', 'dualSpace.queueLength', this.syncQueue.length);
    }

    private notifyTaskUpdate(task: SyncTask): void {
        // 通知WebView更新任务状态
        vscode.commands.executeCommand('dualSpace.updateTaskStatus', {
            taskId: task.id,
            status: task.status,
            error: task.error
        });
    }

    private notifyDocumentUpdated(filePath: string, content: string): void {
        // 通知相关的编辑器视图更新
        vscode.commands.executeCommand('dualSpace.documentUpdated', {
            filePath,
            content
        });
    }

    public getQueueStatus(): { total: number; pending: number; processing: number; completed: number; failed: number } {
        const tasks = Array.from(this.syncTasks.values());
        return {
            total: tasks.length,
            pending: tasks.filter(t => t.status === 'pending').length,
            processing: tasks.filter(t => t.status === 'processing').length,
            completed: tasks.filter(t => t.status === 'completed').length,
            failed: tasks.filter(t => t.status === 'failed').length
        };
    }

    public clearCompletedTasks(): void {
        const completedTasks = Array.from(this.syncTasks.values())
            .filter(t => t.status === 'completed' || t.status === 'failed');
        
        completedTasks.forEach(task => {
            this.syncTasks.delete(task.id);
        });
        
        this.notifyQueueUpdate();
    }
}
```

### 9. AI助手面板组件

```typescript
// src/providers/AIAssistantProvider.ts
import * as vscode from 'vscode';
import { AssociationManager } from '../core/AssociationManager';
import { AIService } from '../services/AIService';

export class AIAssistantProvider implements vscode.WebviewViewProvider {
    public static readonly viewType = 'aiAssistant';

    private _view?: vscode.WebviewView;
    private aiService: AIService;

    constructor(
        private readonly _extensionUri: vscode.Uri,
        private associationManager: AssociationManager
    ) {
        this.aiService = new AIService();
    }

    public resolveWebviewView(
        webviewView: vscode.WebviewView,
        context: vscode.WebviewViewResolveContext,
        _token: vscode.CancellationToken
    ) {
        this._view = webviewView;

        webviewView.webview.options = {
            enableScripts: true,
            localResourceRoots: [this._extensionUri]
        };

        webviewView.webview.html = this._getHtmlForWebview(webviewView.webview);

        webviewView.webview.onDidReceiveMessage(async (data) => {
            switch (data.type) {
                case 'generateDoc':
                    await this.handleGenerateDoc(data.code, data.language);
                    break;
                case 'analyzeCode':
                    await this.handleAnalyzeCode(data.code, data.language);
                    break;
                case 'improveDocs':
                    await this.handleImproveDocs(data.content);
                    break;
                case 'askQuestion':
                    await this.handleAskQuestion(data.question, data.context);
                    break;
                case 'applyResponse':
                    await this.handleApplyResponse(data.responseId, data.content);
                    break;
            }
        });
    }

    private async handleGenerateDoc(code: string, language: string) {
        try {
            this._view?.webview.postMessage({
                type: 'showLoading',
                message: '正在生成文档...'
            });

            const response = await this.aiService.generateDocumentation({
                code,
                language,
                type: this.detectCodeType(code)
            });

            this._view?.webview.postMessage({
                type: 'showResponse',
                response: {
                    id: this.generateResponseId(),
                    type: 'documentation',
                    content: response.content,
                    confidence: response.confidence,
                    suggestions: response.suggestions
                }
            });

        } catch (error) {
            this._view?.webview.postMessage({
                type: 'showError',
                error: error instanceof Error ? error.message : '生成文档失败'
            });
        }
    }

    private async handleAnalyzeCode(code: string, language: string) {
        try {
            this._view?.webview.postMessage({
                type: 'showLoading',
                message: '正在分析代码...'
            });

            const suggestions = await this.aiService.analyzeCodeForDocumentation(code, language);
            const analysis = this.performCodeAnalysis(code, language);

            this._view?.webview.postMessage({
                type: 'showAnalysis',
                analysis: {
                    suggestions,
                    complexity: analysis.complexity,
                    issues: analysis.issues,
                    recommendations: analysis.recommendations
                }
            });

        } catch (error) {
            this._view?.webview.postMessage({
                type: 'showError',
                error: error instanceof Error ? error.message : '分析代码失败'
            });
        }
    }

    private async handleImproveDocs(content: string) {
        try {
            this._view?.webview.postMessage({
                type: 'showLoading',
                message: '正在优化文档...'
            });

            const improvements = await this.generateDocImprovements(content);

            this._view?.webview.postMessage({
                type: 'showImprovements',
                improvements: {
                    id: this.generateResponseId(),
                    original: content,
                    improved: improvements.content,
                    changes: improvements.changes,
                    reasoning: improvements.reasoning
                }
            });

        } catch (error) {
            this._view?.webview.postMessage({
                type: 'showError',
                error: error instanceof Error ? error.message : '优化文档失败'
            });
        }
    }

    private async handleAskQuestion(question: string, context?: string) {
        try {
            this._view?.webview.postMessage({
                type: 'showLoading',
                message: '正在思考...'
            });

            const answer = await this.getAIAnswer(question, context);

            this._view?.webview.postMessage({
                type: 'showAnswer',
                answer: {
                    id: this.generateResponseId(),
                    question,
                    answer: answer.content,
                    confidence: answer.confidence,
                    relatedTopics: answer.relatedTopics
                }
            });

        } catch (error) {
            this._view?.webview.postMessage({
                type: 'showError',
                error: error instanceof Error ? error.message : '获取答案失败'
            });
        }
    }

    private async handleApplyResponse(responseId: string, content: string) {
        try {
            // 获取当前活动编辑器
            const editor = vscode.window.activeTextEditor;
            if (!editor) {
                vscode.window.showErrorMessage('没有打开的编辑器');
                return;
            }

            // 获取选中的文本范围
            const selection = editor.selection;
            
            if (selection.isEmpty) {
                // 如果没有选中文本，在光标位置插入
                await editor.edit(editBuilder => {
                    editBuilder.insert(selection.active, content);
                });
            } else {
                // 替换选中的文本
                await editor.edit(editBuilder => {
                    editBuilder.replace(selection, content);
                });
            }

            this._view?.webview.postMessage({
                type: 'responseApplied',
                responseId
            });

            vscode.window.showInformationMessage('内容已应用到编辑器');

        } catch (error) {
            this._view?.webview.postMessage({
                type: 'showError',
                error: error instanceof Error ? error.message : '应用内容失败'
            });
        }
    }

    private detectCodeType(code: string): 'function' | 'class' | 'module' {
        if (code.includes('function ') || code.includes('=>')) {
            return 'function';
        } else if (code.includes('class ')) {
            return 'class';
        } else {
            return 'module';
        }
    }

    private performCodeAnalysis(code: string, language: string) {
        // 简化的代码分析逻辑
        const lines = code.split('\n');
        const complexity = this.calculateComplexity(code);
        const issues = this.detectCommonIssues(code, language);
        const recommendations = this.generateRecommendations(code, language);

        return {
            complexity: {
                cyclomatic: complexity,
                lines: lines.length,
                functions: (code.match(/function\s+\w+/g) || []).length
            },
            issues,
            recommendations
        };
    }

    private calculateComplexity(code: string): number {
        // 简化的圈复杂度计算
        const keywords = ['if', 'else', 'while', 'for', 'switch', 'case', 'catch', '&&', '||'];
        let complexity = 1;

        keywords.forEach(keyword => {
            const regex = new RegExp(`\\b${keyword}\\b`, 'g');
            const matches = code.match(regex);
            if (matches) {
                complexity += matches.length;
            }
        });

        return complexity;
    }

    private detectCommonIssues(code: string, language: string): string[] {
        const issues: string[] = [];

        // 检查常见问题
        if (!code.includes('/**') && !code.includes('//')) {
            issues.push('缺少注释');
        }

        if (language === 'javascript' || language === 'typescript') {
            if (code.includes('var ')) {
                issues.push('建议使用 let/const 替代 var');
            }
            if (code.includes('== ') || code.includes('!= ')) {
                issues.push('建议使用严格相等运算符 (=== 和 !==)');
            }
        }

        const lines = code.split('\n');
        const longLines = lines.filter(line => line.length > 120);
        if (longLines.length > 0) {
            issues.push(`有 ${longLines.length} 行代码过长 (>120字符)`);
        }

        return issues;
    }

    private generateRecommendations(code: string, language: string): string[] {
        const recommendations: string[] = [];

        if (this.calculateComplexity(code) > 10) {
            recommendations.push('考虑将复杂的函数拆分成更小的函数');
        }

        if (!code.includes('/**')) {
            recommendations.push('添加JSDoc风格的文档注释');
        }

        if (language === 'typescript' && !code.includes(': ')) {
            recommendations.push('添加类型注解提高代码可读性');
        }

        return recommendations;
    }

    private async generateDocImprovements(content: string) {
        // 这里可以调用AI服务来改进文档
        // 简化实现，返回一些基本的改进建议
        
        const improvements = [];
        const lines = content.split('\n');
        
        if (lines.length < 3) {
            improvements.push('文档内容过于简短，建议添加更多详细信息');
        }

        if (!content.includes('##')) {
            improvements.push('建议使用标题结构化文档内容');
        }

        if (!content.includes('```')) {
            improvements.push('建议添加代码示例');
        }

        return {
            content: this.improveDocumentContent(content),
            changes: improvements,
            reasoning: '基于最佳实践的文档改进建议'
        };
    }

    private improveDocumentContent(content: string): string {
        // 简化的文档改进逻辑
        let improved = content;

        // 添加标题结构
        if (!improved.startsWith('#')) {
            improved = `# 文档标题\n\n${improved}`;
        }

        // 确保有基本章节
        if (!improved.includes('## 概述') && !improved.includes('## 说明')) {
            const lines = improved.split('\n');
            lines.splice(2, 0, '## 概述\n');
            improved = lines.join('\n');
        }

        return improved;
    }

    private async getAIAnswer(question: string, context?: string) {
        // 这里调用AI服务获取答案
        // 简化实现
        return {
            content: `关于"${question}"的回答：\n\n这是一个AI生成的回答示例。实际实现中会调用真实的AI服务。`,
            confidence: 0.8,
            relatedTopics: ['相关话题1', '相关话题2']
        };
    }

    private generateResponseId(): string {
        return `response_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
    }

    private _getHtmlForWebview(webview: vscode.Webview) {
        const scriptUri = webview.asWebviewUri(vscode.Uri.joinPath(this._extensionUri, 'media', 'ai-assistant.js'));
        const styleUri = webview.asWebviewUri(vscode.Uri.joinPath(this._extensionUri, 'media', 'ai-assistant.css'));

        return `<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI助手</title>
    <link href="${styleUri}" rel="stylesheet">
</head>
<body>
    <div class="ai-assistant">
        <!-- 对话历史 -->
        <div class="chat-history" id="chatHistory">
            <div class="welcome-message">
                <div class="ai-avatar">🤖</div>
                <div class="message-content">
                    <h3>AI助手</h3>
                    <p>我可以帮助您：</p>
                    <ul>
                        <li>生成代码文档</li>
                        <li>分析代码质量</li>
                        <li>优化现有文档</li>
                        <li>回答技术问题</li>
                    </ul>
                </div>
            </div>
        </div>

        <!-- 快速操作按钮 -->
        <div class="quick-actions" id="quickActions">
            <button class="action-btn" data-action="generateDoc">
                <i class="icon-file-text"></i>
                <span>生成文档</span>
            </button>
            <button class="action-btn" data-action="analyzeCode">
                <i class="icon-search"></i>
                <span>分析代码</span>
            </button>
            <button class="action-btn" data-action="improveDocs">
                <i class="icon-edit"></i>
                <span>优化文档</span>
            </button>
            <button class="action-btn" data-action="askQuestion">
                <i class="icon-help-circle"></i>
                <span>提问</span>
            </button>
        </div>

        <!-- 输入区域 -->
        <div class="input-area">
            <div class="input-container">
                <textarea id="messageInput" placeholder="输入您的问题或选择上方的快速操作..."></textarea>
                <button id="sendButton" class="send-btn">
                    <i class="icon-send"></i>
                </button>
            </div>
            <div class="input-tips" id="inputTips">
                💡 提示：您可以选中代码后使用快速操作，或直接描述您的需求
            </div>
        </div>

        <!-- 加载状态 -->
        <div class="loading-state" id="loadingState" style="display: none;">
            <div class="loading-animation">
                <div class="loading-spinner"></div>
                <span class="loading-text">AI正在思考...</span>
            </div>
        </div>
    </div>

    <script src="${scriptUri}"></script>
</body>
</html>`;
    }
}
```

### 10. AI助手前端JavaScript

```javascript
// media/ai-assistant.js
(function() {
    const vscode = acquireVsCodeApi();
    
    // DOM元素
    const chatHistory = document.getElementById('chatHistory');
    const messageInput = document.getElementById('messageInput');
    const sendButton = document.getElementById('sendButton');
    const quickActions = document.getElementById('quickActions');
    const loadingState = document.getElementById('loadingState');
    const inputTips = document.getElementById('inputTips');
    
    // 状态管理
    let currentAction = null;
    let conversationHistory = [];

    // 初始化
    initialize();

    function initialize() {
        setupEventListeners();
        loadConversationHistory();
    }

    function setupEventListeners() {
        // 发送按钮
        sendButton.addEventListener('click', handleSendMessage);
        
        // 回车发送
        messageInput.addEventListener('keydown', (e) => {
            if (e.key === 'Enter' && (e.ctrlKey || e.metaKey)) {
                e.preventDefault();
                handleSendMessage();
            }
        });

        // 快速操作按钮
        quickActions.addEventListener('click', (e) => {
            const actionBtn = e.target.closest('.action-btn');
            if (actionBtn) {
                const action = actionBtn.dataset.action;
                handleQuickAction(action);
            }
        });

        // 监听来自扩展的消息
        window.addEventListener('message', (event) => {
            handleMessage(event.data);
        });

        // 输入框焦点和失焦事件
        messageInput.addEventListener('focus', () => {
            inputTips.style.display = 'none';
        });

        messageInput.addEventListener('blur', () => {
            if (!messageInput.value.trim()) {
                inputTips.style.display = 'block';
            }
        });
    }

    function handleSendMessage() {
        const message = messageInput.value.trim();
        if (!message) return;

        // 添加用户消息到对话历史
        addUserMessage(message);
        
        // 清空输入框
        messageInput.value = '';
        
        // 根据当前动作处理消息
        if (currentAction) {
            handleActionWithMessage(currentAction, message);
        } else {
            // 智能判断用户意图
            const intent = detectUserIntent(message);
            handleUserIntent(intent, message);
        }
        
        currentAction = null;
    }

    function handleQuickAction(action) {
        currentAction = action;
        
        switch (action) {
            case 'generateDoc':
                handleGenerateDoc();
                break;
            case 'analyzeCode':
                handleAnalyzeCode();
                break;
            case 'improveDocs':
                handleImproveDocs();
                break;
            case 'askQuestion':
                handleAskQuestion();
                break;
        }
    }

    function handleGenerateDoc() {
        // 获取当前选中的代码
        vscode.postMessage({
            type: 'getSelectedCode'
        });
        
        addSystemMessage('请提供需要生成文档的代码，或者在编辑器中选中代码后再次点击生成文档按钮。');
        messageInput.placeholder = '粘贴代码或描述功能...';
        messageInput.focus();
    }

    function handleAnalyzeCode() {
        vscode.postMessage({
            type: 'getSelectedCode'
        });
        
        addSystemMessage('请提供需要分析的代码，或者在编辑器中选中代码后再次点击分析按钮。');
        messageInput.placeholder = '粘贴代码...';
        messageInput.focus();
    }

    function handleImproveDocs() {
        vscode.postMessage({
            type: 'getSelectedText'
        });
        
        addSystemMessage('请提供需要优化的文档内容，或者在编辑器中选中文本后再次点击优化按钮。');
        messageInput.placeholder = '粘贴文档内容...';
        messageInput.focus();
    }

    function handleAskQuestion() {
        addSystemMessage('请输入您的问题，我会尽力为您解答：');
        messageInput.placeholder = '输入您的问题...';
        messageInput.focus();
    }

    function handleActionWithMessage(action, message) {
        showLoading(`正在处理您的${getActionName(action)}请求...`);
        
        switch (action) {
            case 'generateDoc':
                vscode.postMessage({
                    type: 'generateDoc',
                    code: message,
                    language: detectLanguage(message)
                });
                break;
                
            case 'analyzeCode':
                vscode.postMessage({
                    type: 'analyzeCode',
                    code: message,
                    language: detectLanguage(message)
                });
                break;
                
            case 'improveDocs':
                vscode.postMessage({
                    type: 'improveDocs',
                    content: message
                });
                break;
                
            case 'askQuestion':
                vscode.postMessage({
                    type: 'askQuestion',
                    question: message,
                    context: getCurrentContext()
                });
                break;
        }
    }

    function detectUserIntent(message) {
        const lowerMessage = message.toLowerCase();
        
        if (lowerMessage.includes('生成') && (lowerMessage.includes('文档') || lowerMessage.includes('doc'))) {
            return 'generateDoc';
        } else if (lowerMessage.includes('分析') || lowerMessage.includes('检查')) {
            return 'analyzeCode';
        } else if (lowerMessage.includes('优化') || lowerMessage.includes('改进') || lowerMessage.includes('完善')) {
            return 'improveDocs';
        } else {
            return 'askQuestion';
        }
    }

    function handleUserIntent(intent, message) {
        handleActionWithMessage(intent, message);
    }

    function handleMessage(data) {
        hideLoading();
        
        switch (data.type) {
            case 'showResponse':
                showAIResponse(data.response);
                break;
            case 'showAnalysis':
                showCodeAnalysis(data.analysis);
                break;
            case 'showImprovements':
                showDocImprovements(data.improvements);
                break;
            case 'showAnswer':
                showAnswer(data.answer);
                break;
            case 'showError':
                showError(data.error);
                break;
            case 'showLoading':
                showLoading(data.message);
                break;
            case 'selectedCode':
                handleSelectedCode(data.code, data.language);
                break;
            case 'selectedText':
                handleSelectedText(data.text);
                break;
        }
    }

    function showAIResponse(response) {
        const messageElement = createAIMessage();
        
        messageElement.innerHTML = `
            <div class="response-header">
                <h4>📝 生成的文档</h4>
                <div class="confidence-badge">置信度: ${Math.round(response.confidence * 100)}%</div>
            </div>
            <div class="response-content">
                <div class="markdown-content">${formatMarkdown(response.content)}</div>
            </div>
            <div class="response-actions">
                <button class="action-btn apply-btn" onclick="applyResponse('${response.id}', ${JSON.stringify(response.content).replace(/"/g, '&quot;')})">
                    <i class="icon-check"></i> 应用到编辑器
                </button>
                <button class="action-btn copy-btn" onclick="copyToClipboard(${JSON.stringify(response.content).replace(/"/g, '&quot;')})">
                    <i class="icon-copy"></i> 复制
                </button>
                <button class="action-btn edit-btn" onclick="editResponse('${response.id}')">
                    <i class="icon-edit"></i> 编辑
                </button>
            </div>
            ${response.suggestions ? `<div class="suggestions">
                <h5>💡 改进建议：</h5>
                <ul>${response.suggestions.map(s => `<li>${s}</li>`).join('')}</ul>
            </div>` : ''}
        `;
        
        chatHistory.appendChild(messageElement);
        scrollToBottom();
        saveConversationHistory();
    }

    function showCodeAnalysis(analysis) {
        const messageElement = createAIMessage();
        
        messageElement.innerHTML = `
            <div class="response-header">
                <h4>🔍 代码分析结果</h4>
            </div>
            <div class="analysis-content">
                <div class="analysis-section">
                    <h5>📊 复杂度指标</h5>
                    <div class="metrics">
                        <div class="metric">
                            <span class="metric-label">圈复杂度:</span>
                            <span class="metric-value ${analysis.complexity.cyclomatic > 10 ? 'warning' : 'good'}">${analysis.complexity.cyclomatic}</span>
                        </div>
                        <div class="metric">
                            <span class="metric-label">代码行数:</span>
                            <span class="metric-value">${analysis.complexity.lines}</span>
                        </div>
                        <div class="metric">
                            <span class="metric-label">函数数量:</span>
                            <span class="metric-value">${analysis.complexity.functions}</span>
                        </div>
                    </div>
                </div>
                
                ${analysis.issues.length > 0 ? `
                <div class="analysis-section">
                    <h5>⚠️ 发现的问题</h5>
                    <ul class="issue-list">
                        ${analysis.issues.map(issue => `<li class="issue-item">${issue}</li>`).join('')}
                    </ul>
                </div>
                ` : ''}
                
                ${analysis.recommendations.length > 0 ? `
                <div class="analysis-section">
                    <h5>💡 优化建议</h5>
                    <ul class="recommendation-list">
                        ${analysis.recommendations.map(rec => `<li class="recommendation-item">${rec}</li>`).join('')}
                    </ul>
                </div>
                ` : ''}
                
                <div class="analysis-section">
                    <h5>📋 文档建议</h5>
                    <ul class="suggestion-list">
                        ${analysis.suggestions.map(suggestion => `<li class="suggestion-item">${suggestion}</li>`).join('')}
                    </ul>
                </div>
            </div>
        `;
        
        chatHistory.appendChild(messageElement);
        scrollToBottom();
        saveConversationHistory();
    }

    function showDocImprovements(improvements) {
        const messageElement = createAIMessage();
        
        messageElement.innerHTML = `
            <div class="response-header">
                <h4>✨ 文档优化建议</h4>
            </div>
            <div class="improvements-content">
                <div class="improvement-comparison">
                    <div class="before-after">
                        <div class="before">
                            <h5>原始内容:</h5>
                            <div class="content-preview">${formatMarkdown(improvements.original)}</div>
                        </div>
                        <div class="after">
                            <h5>优化后:</h5>
                            <div class="content-preview">${formatMarkdown(improvements.improved)}</div>
                        </div>
                    </div>
                </div>
                
                <div class="improvement-details">
                    <h5>🔧 主要改进:</h5>
                    <ul>
                        ${improvements.changes.map(change => `<li>${change}</li>`).join('')}
                    </ul>
                </div>
                
                <div class="improvement-reasoning">
                    <h5>💭 改进原因:</h5>
                    <p>${improvements.reasoning}</p>
                </div>
            </div>
            <div class="response-actions">
                <button class="action-btn apply-btn" onclick="applyResponse('${improvements.id}', ${JSON.stringify(improvements.improved).replace(/"/g, '&quot;')})">
                    <i class="icon-check"></i> 应用优化
                </button>
                <button class="action-btn copy-btn" onclick="copyToClipboard(${JSON.stringify(improvements.improved).replace(/"/g, '&quot;')})">
                    <i class="icon-copy"></i> 复制优化内容
                </button>
            </div>
        `;
        
        chatHistory.appendChild(messageElement);
        scrollToBottom();
        saveConversationHistory();
    }

    function showAnswer(answer) {
        const messageElement = createAIMessage();
        
        messageElement.innerHTML = `
            <div class="response-header">
                <h4>🤖 AI回答</h4>
                <div class="confidence-badge">置信度: ${Math.round(answer.confidence * 100)}%</div>
            </div>
            <div class="question-context">
                <strong>问题:</strong> ${answer.question}
            </div>
            <div class="answer-content">
                ${formatMarkdown(answer.answer)}
            </div>
            ${answer.relatedTopics && answer.relatedTopics.length > 0 ? `
            <div class="related-topics">
                <h5>🔗 相关话题:</h5>
                <div class="topic-tags">
                    ${answer.relatedTopics.map(topic => `<span class="topic-tag" onclick="askAboutTopic('${topic}')">${topic}</span>`).join('')}
                </div>
            </div>
            ` : ''}
            <div class="response-actions">
                <button class="action-btn copy-btn" onclick="copyToClipboard(${JSON.stringify(answer.answer).replace(/"/g, '&quot;')})">
                    <i class="icon-copy"></i> 复制回答
                </button>
            </div>
        `;
        
        chatHistory.appendChild(messageElement);
        scrollToBottom();
        saveConversationHistory();
    }

    function showError(error) {
        const messageElement = createAIMessage();
        messageElement.classList.add('error-message');
        
        messageElement.innerHTML = `
            <div class="error-content">
                <div class="error-icon">❌</div>
                <div class="error-text">
                    <h4>出现错误</h4>
                    <p>${error}</p>
                </div>
            </div>
            <div class="error-actions">
                <button class="action-btn retry-btn" onclick="retryLastAction()">
                    <i class="icon-refresh"></i> 重试
                </button>
            </div>
        `;
        
        chatHistory.appendChild(messageElement);
        scrollToBottom();
    }

    function addUserMessage(message) {
        const messageElement = document.createElement('div');
        messageElement.className = 'message user-message';
        
        messageElement.innerHTML = `
            <div class="message-content">
                <div class="message-text">${escapeHtml(message)}</div>
                <div class="message-time">${formatTime(new Date())}</div>
            </div>
            <div class="user-avatar">👤</div>
        `;
        
        chatHistory.appendChild(messageElement);
        scrollToBottom();
        
        // 添加到对话历史
        conversationHistory.push({
            type: 'user',
            content: message,
            timestamp: Date.now()
        });
    }

    function addSystemMessage(message) {
        const messageElement = createAIMessage();
        messageElement.classList.add('system-message');
        
        messageElement.innerHTML = `
            <div class="system-content">
                <div class="system-icon">ℹ️</div>
                <div class="system-text">${message}</div>
            </div>
        `;
        
        chatHistory.appendChild(messageElement);
        scrollToBottom();
    }

    function createAIMessage() {
        const messageElement = document.createElement('div');
        messageElement.className = 'message ai-message';
        
        const wrapper = document.createElement('div');
        wrapper.className = 'ai-message-wrapper';
        
        const avatar = document.createElement('div');
        avatar.className = 'ai-avatar';
        avatar.textContent = '🤖';
        
        const content = document.createElement('div');
        content.className = 'message-content';
        
        wrapper.appendChild(avatar);
        wrapper.appendChild(content);
        messageElement.appendChild(wrapper);
        
        return content;
    }

    function showLoading(message) {
        loadingState.querySelector('.loading-text').textContent = message;
        loadingState.style.display = 'block';
        scrollToBottom();
    }

    function hideLoading() {
        loadingState.style.display = 'none';
    }

    function scrollToBottom() {
        setTimeout(() => {
            chatHistory.scrollTop = chatHistory.scrollHeight;
        }, 100);
    }

    function handleSelectedCode(code, language) {
        if (currentAction === 'generateDoc') {
            vscode.postMessage({
                type: 'generateDoc',
                code,
                language
            });
        } else if (currentAction === 'analyzeCode') {
            vscode.postMessage({
                type: 'analyzeCode',
                code,
                language
            });
        }
        currentAction = null;
    }

    function handleSelectedText(text) {
        if (currentAction === 'improveDocs') {
            vscode.postMessage({
                type: 'improveDocs',
                content: text
            });
        }
        currentAction = null;
    }

    function detectLanguage(code) {
        // 简单的语言检测
        if (code.includes('function ') || code.includes('=>') || code.includes('const ')) {
            return 'javascript';
        } else if (code.includes('def ') || code.includes('import ')) {
            return 'python';
        } else if (code.includes('public class') || code.includes('private ')) {
            return 'java';
        } else {
            return 'text';
        }
    }

    function getCurrentContext() {
        // 获取当前上下文信息
        return {
            workspace: 'current-workspace',
            file: 'current-file',
            selection: 'current-selection'
        };
    }

    function getActionName(action) {
        const actionNames = {
            'generateDoc': '文档生成',
            'analyzeCode': '代码分析',
            'improveDocs': '文档优化',
            'askQuestion': '问题咨询'
        };
        return actionNames[action] || action;
    }

    function formatMarkdown(text) {
        // 简单的Markdown格式化
        return text
            .replace(/```(\w+)?\n([\s\S]*?)```/g, '<pre><code class="language-$1">$2</code></pre>')
            .replace(/`([^`]+)`/g, '<code>$1</code>')
            .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
            .replace(/\*(.*?)\*/g, '<em>$1</em>')
            .replace(/^## (.*$)/gim, '<h2>$1</h2>')
            .replace(/^### (.*$)/gim, '<h3>$1</h3>')
            .replace(/^- (.*$)/gim, '<li>$1</li>')
            .replace(/(<li>.*<\/li>)/s, '<ul>$1</ul>')
            .replace(/\n/g, '<br>');
    }

    function formatTime(date) {
        return date.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
    }

    function escapeHtml(text) {
        const div = document.createElement('div');
        div.textContent = text;
        return div.innerHTML;
    }

    function saveConversationHistory() {
        // 保存对话历史到本地存储
        vscode.setState({ conversationHistory });
    }

    function loadConversationHistory() {
        // 从本地存储加载对话历史
        const state = vscode.getState();
        if (state && state.conversationHistory) {
            conversationHistory = state.conversationHistory;
            // 重建对话界面
            rebuildConversationUI();
        }
    }

    function rebuildConversationUI() {
        // 根据历史记录重建对话界面
        conversationHistory.slice(-10).forEach(item => { // 只显示最近10条
            if (item.type === 'user') {
                addUserMessage(item.content);
            }
        });
    }

    // 全局函数供HTML调用
    window.applyResponse = function(responseId, content) {
        vscode.postMessage({
            type: 'applyResponse',
            responseId,
            content: content
        });
    };

    window.copyToClipboard = function(content) {
        navigator.clipboard.writeText(content).then(() => {
            // 显示复制成功提示
            const toast = document.createElement('div');
            toast.className = 'copy-toast';
            toast.textContent = '已复制到剪贴板';
            document.body.appendChild(toast);
            
            setTimeout(() => {
                document.body.removeChild(toast);
            }, 2000);
        });
    };

    window.editResponse = function(responseId) {
        // 打开编辑模式
        const responseElement = document.querySelector(`[data-response-id="${responseId}"]`);
        if (responseElement) {
            // 实现编辑功能
            console.log('编辑回应:', responseId);
        }
    };

    window.askAboutTopic = function(topic) {
        messageInput.value = `请详细介绍下"${topic}"`;
        handleSendMessage();
    };

    window.retryLastAction = function() {
        // 重试最后一个失败的操作
        if (conversationHistory.length > 0) {
            const lastMessage = conversationHistory[conversationHistory.length - 1];
            if (lastMessage.type === 'user') {
                const intent = detectUserIntent(lastMessage.content);
                handleUserIntent(intent, lastMessage.content);
            }
        }
    };
})();
```

## 总结

这个完整的AI驱动IDE双空间智能管理插件包含了：

### 核心特性
1. **双空间管理**: 代码和文档的统一视图
2. **智能关联**: 自动识别和手动创建关联关系
3. **AI辅助**: 多AI服务集成，智能文档生成
4. **实时同步**: 基于文件变更的智能同步机制
5. **插件化架构**: 模块化设计，易于扩展

### 技术亮点
- TypeScript + VS Code Extension API
- WebView + JavaScript 前端交互
- SQLite 数据持久化
- 多AI服务适配器模式
- 响应式UI设计
- 主题适配和国际化支持

这个插件可以显著提升开发者的代码文档管理效率，通过AI辅助让文档编写变得更加智能和高效。

## 5. 数据存储设计

### 5.1 数据库设计

#### 5.1.1 SQLite 数据库结构

```sql
-- 关联关系表
CREATE TABLE associations (
    id TEXT PRIMARY KEY,
    source_file TEXT NOT NULL,
    target_file TEXT NOT NULL,
    source_range TEXT,
    target_range TEXT,
    type TEXT NOT NULL CHECK (type IN ('function', 'class', 'module', 'manual', 'auto_detected')),
    description TEXT,
    last_sync_time INTEGER NOT NULL,
    is_active BOOLEAN DEFAULT 1,
    metadata TEXT, -- JSON格式的元数据
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- 同步历史表
CREATE TABLE sync_histories (
    id TEXT PRIMARY KEY,
    association_id TEXT NOT NULL,
    operation_type TEXT NOT NULL CHECK (operation_type IN ('code_to_doc', 'doc_to_code', 'bidirectional', 'validation')),
    status TEXT NOT NULL CHECK (status IN ('pending', 'in_progress', 'completed', 'failed', 'cancelled')),
    start_time INTEGER NOT NULL,
    end_time INTEGER,
    changes TEXT, -- JSON格式的变更记录
    error_info TEXT, -- JSON格式的错误信息
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (association_id) REFERENCES associations(id) ON DELETE CASCADE
);

-- AI响应缓存表
CREATE TABLE ai_cache_entries (
    cache_key TEXT PRIMARY KEY,
    provider TEXT NOT NULL,
    request_hash TEXT NOT NULL,
    response_content TEXT NOT NULL,
    confidence REAL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    expires_at DATETIME,
    access_count INTEGER DEFAULT 0,
    last_accessed DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- 用户行为分析表
CREATE TABLE user_analytics (
    id TEXT PRIMARY KEY,
    event_type TEXT NOT NULL,
    event_data TEXT, -- JSON格式的事件数据
    user_id TEXT,
    session_id TEXT,
    timestamp INTEGER NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- 索引定义
CREATE INDEX idx_associations_source_file ON associations(source_file);
CREATE INDEX idx_associations_target_file ON associations(target_file);
CREATE INDEX idx_associations_type ON associations(type);
CREATE INDEX idx_associations_active ON associations(is_active);
CREATE INDEX idx_sync_histories_association ON sync_histories(association_id);
CREATE INDEX idx_sync_histories_status ON sync_histories(status);
CREATE INDEX idx_ai_cache_provider ON ai_cache_entries(provider);
CREATE INDEX idx_ai_cache_expires ON ai_cache_entries(expires_at);
CREATE INDEX idx_analytics_event_type ON user_analytics(event_type);
CREATE INDEX idx_analytics_timestamp ON user_analytics(timestamp);
```

#### 5.1.2 配置文件结构

```typescript
// .dual-space/config/project-config.json
interface ProjectConfig {
    version: string;
    projectName: string;
    workspaceRoot: string;
    enabledFeatures: {
        realTimeSync: boolean;
        aiAssistant: boolean;
        autoAssociation: boolean;
        performanceMonitoring: boolean;
    };
    syncStrategy: {
        type: 'realtime' | 'batch' | 'manual';
        batchInterval: number; // 毫秒
        conflictResolution: 'manual' | 'auto_code' | 'auto_doc';
    };
    filePatterns: {
        codeFiles: string[];
        docFiles: string[];
        excludePatterns: string[];
    };
    performance: {
        maxCacheSize: number; // MB
        maxCacheAge: number; // 小时
        enableMetrics: boolean;
    };
}

// .dual-space/config/ai-provider-config.json
interface AIProviderConfig {
    currentProvider: 'openai' | 'claude' | 'local';
    providers: {
        openai: {
            apiKey?: string;
            model: string;
            maxTokens: number;
            temperature: number;
            timeout: number;
        };
        claude: {
            apiKey?: string;
            model: string;
            maxTokens: number;
            timeout: number;
        };
        local: {
            endpoint: string;
            model: string;
            timeout: number;
        };
    };
    fallbackOrder: string[];
    caching: {
        enabled: boolean;
        ttl: number; // 小时
        maxSize: number; // MB
    };
}

// .dual-space/config/user-preferences.json
interface UserPreferences {
    ui: {
        theme: 'light' | 'dark' | 'auto';
        spaceRatio: number; // 0.2 - 0.8
        showLineNumbers: boolean;
        fontSize: number;
        fontFamily: string;
    };
    editor: {
        autoSave: boolean;
        autoFormat: boolean;
        showMinimap: boolean;
        wordWrap: boolean;
    };
    notifications: {
        syncComplete: boolean;
        syncError: boolean;
        aiResponse: boolean;
        associationCreated: boolean;
    };
    shortcuts: Record<string, string>;
    language: 'zh-CN' | 'en-US' | 'ja-JP';
}
```

### 5.2 缓存策略

#### 5.2.1 多层缓存架构

```typescript
// src/utils/common/Cache.ts
/**
 * @fileoverview 多层缓存系统 - 提供内存、文件和数据库三级缓存
 * @author 技术团队
 * @since v1.0.0
 */

export class MultiLevelCache<T> {
    private memoryCache: Map<string, CacheEntry<T>> = new Map();
    private maxMemorySize: number;
    private maxFileSize: number;
    private ttl: number;

    constructor(
        private cacheDir: string,
        options: CacheOptions = {}
    ) {
        this.maxMemorySize = options.maxMemorySize || 50 * 1024 * 1024; // 50MB
        this.maxFileSize = options.maxFileSize || 200 * 1024 * 1024; // 200MB
        this.ttl = options.ttl || 24 * 60 * 60 * 1000; // 24小时
        
        this.setupCleanupTimer();
    }

    /**
     * 获取缓存项
     */
    public async get(key: string): Promise<T | null> {
        // 1. 检查内存缓存
        const memoryEntry = this.memoryCache.get(key);
        if (memoryEntry && !this.isExpired(memoryEntry)) {
            memoryEntry.lastAccessed = Date.now();
            memoryEntry.accessCount++;
            return memoryEntry.value;
        }

        // 2. 检查文件缓存
        const fileEntry = await this.getFromFileCache(key);
        if (fileEntry && !this.isExpired(fileEntry)) {
            // 提升到内存缓存
            this.setInMemoryCache(key, fileEntry);
            return fileEntry.value;
        }

        // 3. 检查数据库缓存
        const dbEntry = await this.getFromDatabaseCache(key);
        if (dbEntry && !this.isExpired(dbEntry)) {
            // 提升到文件和内存缓存
            await this.setInFileCache(key, dbEntry);
            this.setInMemoryCache(key, dbEntry);
            return dbEntry.value;
        }

        return null;
    }

    /**
     * 设置缓存项
     */
    public async set(key: string, value: T, customTtl?: number): Promise<void> {
        const entry: CacheEntry<T> = {
            value,
            createdAt: Date.now(),
            expiresAt: Date.now() + (customTtl || this.ttl),
            lastAccessed: Date.now(),
            accessCount: 1,
            size: this.calculateSize(value)
        };

        // 同时写入三级缓存
        this.setInMemoryCache(key, entry);
        await this.setInFileCache(key, entry);
        await this.setInDatabaseCache(key, entry);
    }

    /**
     * 删除缓存项
     */
    public async delete(key: string): Promise<void> {
        this.memoryCache.delete(key);
        await this.deleteFromFileCache(key);
        await this.deleteFromDatabaseCache(key);
    }

    /**
     * 清空所有缓存
     */
    public async clear(): Promise<void> {
        this.memoryCache.clear();
        await this.clearFileCache();
        await this.clearDatabaseCache();
    }

    // 私有方法实现...
    private isExpired(entry: CacheEntry<T>): boolean {
        return Date.now() > entry.expiresAt;
    }

    private calculateSize(value: T): number {
        return JSON.stringify(value).length * 2; // 粗略估算
    }

    private setInMemoryCache(key: string, entry: CacheEntry<T>): void {
        // 检查内存限制
        if (this.getMemoryCacheSize() + entry.size > this.maxMemorySize) {
            this.evictLeastRecentlyUsed();
        }
        
        this.memoryCache.set(key, entry);
    }

    private evictLeastRecentlyUsed(): void {
        let oldestKey = '';
        let oldestTime = Date.now();

        for (const [key, entry] of this.memoryCache) {
            if (entry.lastAccessed < oldestTime) {
                oldestTime = entry.lastAccessed;
                oldestKey = key;
            }
        }

        if (oldestKey) {
            this.memoryCache.delete(oldestKey);
        }
    }

    private getMemoryCacheSize(): number {
        let totalSize = 0;
        for (const entry of this.memoryCache.values()) {
            totalSize += entry.size;
        }
        return totalSize;
    }

    private setupCleanupTimer(): void {
        setInterval(() => {
            this.cleanupExpiredEntries();
        }, 60 * 60 * 1000); // 每小时清理一次
    }

    private cleanupExpiredEntries(): void {
        const now = Date.now();
        
        // 清理内存缓存
        for (const [key, entry] of this.memoryCache) {
            if (now > entry.expiresAt) {
                this.memoryCache.delete(key);
            }
        }
    }

    // 文件缓存和数据库缓存的具体实现...
}

interface CacheEntry<T> {
    value: T;
    createdAt: number;
    expiresAt: number;
    lastAccessed: number;
    accessCount: number;
    size: number;
}

interface CacheOptions {
    maxMemorySize?: number;
    maxFileSize?: number;
    ttl?: number;
}
```

## 6. 安全设计

### 6.1 数据安全

#### 6.1.1 API密钥管理

```typescript
// src/utils/common/SecurityManager.ts
/**
 * @fileoverview 安全管理器 - 负责API密钥加密存储和访问控制
 * @author 技术团队
 * @since v1.0.0
 */

import * as crypto from 'crypto';
import * as keytar from 'keytar';
import { DualSpaceError, ErrorCodes } from '../../types/common/ErrorTypes';

export class SecurityManager {
    private static readonly SERVICE_NAME = 'dual-space-plugin';
    private static readonly ENCRYPTION_ALGORITHM = 'aes-256-gcm';
    private static readonly KEY_LENGTH = 32;

    /**
     * 安全存储API密钥
     */
    public static async storeAPIKey(
        provider: string, 
        apiKey: string
    ): Promise<void> {
        try {
            // 生成加密密钥
            const encryptionKey = await this.getOrCreateEncryptionKey();
            
            // 加密API密钥
            const encryptedKey = this.encrypt(apiKey, encryptionKey);
            
            // 存储到系统密钥链
            await keytar.setPassword(
                this.SERVICE_NAME,
                `${provider}_api_key`,
                JSON.stringify(encryptedKey)
            );

        } catch (error) {
            throw new DualSpaceError(
                ErrorCodes.SYSTEM_ERROR,
                `API密钥存储失败: ${error.message}`,
                { provider, originalError: error }
            );
        }
    }

    /**
     * 获取API密钥
     */
    public static async getAPIKey(provider: string): Promise<string | null> {
        try {
            // 从系统密钥链获取
            const encryptedData = await keytar.getPassword(
                this.SERVICE_NAME,
                `${provider}_api_key`
            );

            if (!encryptedData) {
                return null;
            }

            // 获取解密密钥
            const encryptionKey = await this.getOrCreateEncryptionKey();
            
            // 解密API密钥
            const encryptedKey = JSON.parse(encryptedData);
            return this.decrypt(encryptedKey, encryptionKey);

        } catch (error) {
            throw new DualSpaceError(
                ErrorCodes.SYSTEM_ERROR,
                `API密钥获取失败: ${error.message}`,
                { provider, originalError: error }
            );
        }
    }

    /**
     * 删除API密钥
     */
    public static async deleteAPIKey(provider: string): Promise<void> {
        try {
            await keytar.deletePassword(
                this.SERVICE_NAME,
                `${provider}_api_key`
            );
        } catch (error) {
            throw new DualSpaceError(
                ErrorCodes.SYSTEM_ERROR,
                `API密钥删除失败: ${error.message}`,
                { provider, originalError: error }
            );
        }
    }

    // 私有方法
    private static async getOrCreateEncryptionKey(): Promise<Buffer> {
        let keyData = await keytar.getPassword(
            this.SERVICE_NAME,
            'encryption_key'
        );

        if (!keyData) {
            // 生成新的加密密钥
            const key = crypto.randomBytes(this.KEY_LENGTH);
            await keytar.setPassword(
                this.SERVICE_NAME,
                'encryption_key',
                key.toString('base64')
            );
            return key;
        }

        return Buffer.from(keyData, 'base64');
    }

    private static encrypt(text: string, key: Buffer): EncryptedData {
        const iv = crypto.randomBytes(16);
        const cipher = crypto.createCipher(this.ENCRYPTION_ALGORITHM, key);
        cipher.setAAD(Buffer.from('dual-space-auth'));

        let encrypted = cipher.update(text, 'utf8', 'hex');
        encrypted += cipher.final('hex');

        const authTag = cipher.getAuthTag();

        return {
            encrypted,
            iv: iv.toString('hex'),
            authTag: authTag.toString('hex')
        };
    }

    private static decrypt(encryptedData: EncryptedData, key: Buffer): string {
        const decipher = crypto.createDecipher(this.ENCRYPTION_ALGORITHM, key);
        decipher.setAAD(Buffer.from('dual-space-auth'));
        decipher.setAuthTag(Buffer.from(encryptedData.authTag, 'hex'));

        let decrypted = decipher.update(encryptedData.encrypted, 'hex', 'utf8');
        decrypted += decipher.final('utf8');

        return decrypted;
    }
}

interface EncryptedData {
    encrypted: string;
    iv: string;
    authTag: string;
}
```

### 6.2 输入验证

```typescript
// src/utils/validation-utils/InputValidator.ts
/**
 * @fileoverview 输入验证器 - 提供全面的输入数据验证
 * @author 技术团队
 * @since v1.0.0
 */

import * as path from 'path';
import { DualSpaceError, ErrorCodes } from '../../types/common/ErrorTypes';

export class InputValidator {
    private static readonly MAX_FILE_PATH_LENGTH = 260;
    private static readonly MAX_DESCRIPTION_LENGTH = 1000;
    private static readonly ALLOWED_FILE_EXTENSIONS = [
        '.ts', '.js', '.py', '.java', '.cpp', '.c', '.cs', '.go', '.rs',
        '.md', '.txt', '.rst', '.adoc'
    ];

    /**
     * 验证文件路径
     */
    public static validateFilePath(filePath: string): void {
        if (!filePath || typeof filePath !== 'string') {
            throw new DualSpaceError(
                ErrorCodes.INVALID_INPUT,
                '文件路径不能为空',
                { filePath },
                false,
                '请提供有效的文件路径'
            );
        }

        if (filePath.length > this.MAX_FILE_PATH_LENGTH) {
            throw new DualSpaceError(
                ErrorCodes.INVALID_INPUT,
                `文件路径过长，最大长度为 ${this.MAX_FILE_PATH_LENGTH} 字符`,
                { filePath, length: filePath.length },
                false,
                '文件路径过长，请使用较短的路径'
            );
        }

        // 检查路径遍历攻击
        const normalizedPath = path.normalize(filePath);
        if (normalizedPath.includes('..')) {
            throw new DualSpaceError(
                ErrorCodes.INVALID_INPUT,
                '文件路径包含非法字符',
                { filePath },
                false,
                '文件路径不能包含相对路径符号'
            );
        }

        // 检查文件扩展名
        const ext = path.extname(filePath).toLowerCase();
        if (!this.ALLOWED_FILE_EXTENSIONS.includes(ext)) {
            throw new DualSpaceError(
                ErrorCodes.INVALID_INPUT,
                `不支持的文件类型: ${ext}`,
                { filePath, extension: ext },
                false,
                `支持的文件类型: ${this.ALLOWED_FILE_EXTENSIONS.join(', ')}`
            );
        }
    }

    /**
     * 验证关联描述
     */
    public static validateDescription(description?: string): void {
        if (description && description.length > this.MAX_DESCRIPTION_LENGTH) {
            throw new DualSpaceError(
                ErrorCodes.INVALID_INPUT,
                `描述过长，最大长度为 ${this.MAX_DESCRIPTION_LENGTH} 字符`,
                { description, length: description.length },
                false,
                '请缩短描述内容'
            );
        }

        // 检查XSS攻击
        if (description && this.containsXSSPatterns(description)) {
            throw new DualSpaceError(
                ErrorCodes.INVALID_INPUT,
                '描述包含非法内容',
                { description },
                false,
                '描述不能包含脚本代码'
            );
        }
    }

    /**
     * 验证代码内容
     */
    public static validateCodeContent(code: string): void {
        if (!code || typeof code !== 'string') {
            throw new DualSpaceError(
                ErrorCodes.INVALID_INPUT,
                '代码内容不能为空',
                { code },
                false,
                '请提供有效的代码内容'
            );
        }

        // 检查代码长度限制
        const MAX_CODE_LENGTH = 100000; // 100KB
        if (code.length > MAX_CODE_LENGTH) {
            throw new DualSpaceError(
                ErrorCodes.INVALID_INPUT,
                `代码内容过长，最大长度为 ${MAX_CODE_LENGTH} 字符`,
                { length: code.length },
                false,
                '请减少代码内容或分段处理'
            );
        }
    }

    /**
     * 验证AI请求参数
     */
    public static validateAIRequest(request: any): void {
        if (!request || typeof request !== 'object') {
            throw new DualSpaceError(
                ErrorCodes.INVALID_INPUT,
                'AI请求参数无效',
                { request },
                false,
                '请提供有效的请求参数'
            );
        }

        // 验证必需字段
        const requiredFields = ['code', 'language', 'type'];
        for (const field of requiredFields) {
            if (!request[field]) {
                throw new DualSpaceError(
                    ErrorCodes.INVALID_INPUT,
                    `缺少必需参数: ${field}`,
                    { request, missingField: field },
                    false,
                    `请提供 ${field} 参数`
                );
            }
        }

        // 验证语言类型
        const supportedLanguages = [
            'typescript', 'javascript', 'python', 'java', 
            'cpp', 'c', 'csharp', 'go', 'rust'
        ];
        
        if (!supportedLanguages.includes(request.language)) {
            throw new DualSpaceError(
                ErrorCodes.INVALID_INPUT,
                `不支持的编程语言: ${request.language}`,
                { language: request.language },
                false,
                `支持的语言: ${supportedLanguages.join(', ')}`
            );
        }
    }

    // 私有方法
    private static containsXSSPatterns(text: string): boolean {
        const xssPatterns = [
            /<script\b[^<]*(?:(?!<\/script>)<[^<]*)*<\/script>/gi,
            /javascript:/gi,
            /on\w+\s*=/gi,
            /<iframe\b[^<]*(?:(?!<\/iframe>)<[^<]*)*<\/iframe>/gi
        ];

        return xssPatterns.some(pattern => pattern.test(text));
    }
}
```

## 7. 性能优化

### 7.1 性能监控

```typescript
// src/utils/common/PerformanceMonitor.ts
/**
 * @fileoverview 性能监控器 - 监控和分析插件性能指标
 * @author 技术团队
 * @since v1.0.0
 */

export class PerformanceMonitor {
    private static instance: PerformanceMonitor;
    private metrics: Map<string, PerformanceMetric[]> = new Map();
    private activeOperations: Map<string, OperationTimer> = new Map();
    private logger: Logger;

    private constructor() {
        this.logger = new Logger('PerformanceMonitor');
        this.setupPeriodicReporting();
    }

    public static getInstance(): PerformanceMonitor {
        if (!PerformanceMonitor.instance) {
            PerformanceMonitor.instance = new PerformanceMonitor();
        }
        return PerformanceMonitor.instance;
    }

    /**
     * 开始性能计时
     */
    public startTimer(operationName: string, metadata?: Record<string, any>): string {
        const timerId = `${operationName}_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
        
        this.activeOperations.set(timerId, {
            operationName,
            startTime: performance.now(),
            startMemory: process.memoryUsage(),
            metadata: metadata || {}
        });

        return timerId;
    }

    /**
     * 结束性能计时
     */
    public endTimer(timerId: string): PerformanceResult | null {
        const timer = this.activeOperations.get(timerId);
        if (!timer) {
            this.logger.warn('尝试结束不存在的计时器', { timerId });
            return null;
        }

        const endTime = performance.now();
        const endMemory = process.memoryUsage();
        const duration = endTime - timer.startTime;

        const result: PerformanceResult = {
            operationName: timer.operationName,
            duration,
            memoryDelta: {
                rss: endMemory.rss - timer.startMemory.rss,
                heapUsed: endMemory.heapUsed - timer.startMemory.heapUsed,
                heapTotal: endMemory.heapTotal - timer.startMemory.heapTotal
            },
            timestamp: Date.now(),
            metadata: timer.metadata
        };

        // 记录性能指标
        this.recordMetric(timer.operationName, {
            value: duration,
            type: 'duration',
            timestamp: Date.now(),
            metadata: result.metadata
        });

        // 清理计时器
        this.activeOperations.delete(timerId);

        // 检查性能阈值
        this.checkPerformanceThresholds(result);

        return result;
    }

    /**
     * 记录自定义性能指标
     */
    public recordMetric(name: string, metric: PerformanceMetric): void {
        if (!this.metrics.has(name)) {
            this.metrics.set(name, []);
        }

        const metrics = this.metrics.get(name)!;
        metrics.push(metric);

        // 保持最近1000个数据点
        if (metrics.length > 1000) {
            metrics.shift();
        }

        // 实时性能警告
        if (this.shouldAlert(name, metric)) {
            this.alertPerformanceIssue(name, metric);
        }
    }

    /**
     * 获取性能统计
     */
    public getPerformanceStats(operationName?: string): PerformanceStats {
        if (operationName) {
            return this.calculateStats(operationName);
        }

        const allStats: Record<string, PerformanceStats> = {};
        for (const [name] of this.metrics) {
            allStats[name] = this.calculateStats(name);
        }

        return {
            operations: allStats,
            summary: this.calculateOverallStats()
        } as any;
    }

    /**
     * 性能装饰器
     */
    public static performance(operationName: string) {
        return function (target: any, propertyName: string, descriptor: PropertyDescriptor) {
            const method = descriptor.value;
            const monitor = PerformanceMonitor.getInstance();

            descriptor.value = async function (...args: any[]) {
                const timerId = monitor.startTimer(operationName, {
                    className: target.constructor.name,
                    methodName: propertyName,
                    args: args.length
                });

                try {
                    const result = await method.apply(this, args);
                    monitor.endTimer(timerId);
                    return result;
                } catch (error) {
                    const timer = monitor.activeOperations.get(timerId);
                    if (timer) {
                        monitor.recordMetric(`${operationName}_error`, {
                            value: 1,
                            type: 'counter',
                            timestamp: Date.now(),
                            metadata: { error: error.message }
                        });
                    }
                    monitor.endTimer(timerId);
                    throw error;
                }
            };
        };
    }

    // 私有方法
    private calculateStats(operationName: string): PerformanceStats {
        const metrics = this.metrics.get(operationName) || [];
        if (metrics.length === 0) {
            return {
                count: 0,
                average: 0,
                min: 0,
                max: 0,
                p50: 0,
                p95: 0,
                p99: 0
            };
        }

        const values = metrics.map(m => m.value).sort((a, b) => a - b);
        const count = values.length;

        return {
            count,
            average: values.reduce((sum, val) => sum + val, 0) / count,
            min: values[0],
            max: values[count - 1],
            p50: values[Math.floor(count * 0.5)],
            p95: values[Math.floor(count * 0.95)],
            p99: values[Math.floor(count * 0.99)]
        };
    }

    private shouldAlert(name: string, metric: PerformanceMetric): boolean {
        const thresholds = {
            'ai_generation': 10000, // 10秒
            'file_sync': 5000,      // 5秒
            'association_creation': 1000, // 1秒
            'memory_usage': 100 * 1024 * 1024 // 100MB
        };

        const threshold = thresholds[name];
        return threshold && metric.value > threshold;
    }

    private alertPerformanceIssue(name: string, metric: PerformanceMetric): void {
        this.logger.warn('性能警告', {
            operation: name,
            value: metric.value,
            threshold: this.getThreshold(name),
            metadata: metric.metadata
        });

        // 可以在这里添加更多的警告机制，如发送通知等
    }

    private getThreshold(name: string): number {
        const thresholds = {
            'ai_generation': 10000,
            'file_sync': 5000,
            'association_creation': 1000,
            'memory_usage': 100 * 1024 * 1024
        };
        return thresholds[name] || 1000;
    }

    private setupPeriodicReporting(): void {
        setInterval(() => {
            this.generatePerformanceReport();
        }, 5 * 60 * 1000); // 每5分钟生成一次报告
    }

    private generatePerformanceReport(): void {
        const stats = this.getPerformanceStats();
        this.logger.info('性能报告', { stats });
        
        // 清理过期数据
        this.cleanupOldMetrics();
    }

    private cleanupOldMetrics(): void {
        const cutoffTime = Date.now() - (24 * 60 * 60 * 1000); // 24小时前
        
        for (const [name, metrics] of this.metrics) {
            const filteredMetrics = metrics.filter(m => m.timestamp > cutoffTime);
            this.metrics.set(name, filteredMetrics);
        }
    }

    private calculateOverallStats(): any {
        // 计算整体性能统计
        return {
            totalOperations: Array.from(this.metrics.values())
                .reduce((sum, metrics) => sum + metrics.length, 0),
            averageResponseTime: this.calculateAverageResponseTime(),
            memoryUsage: process.memoryUsage(),
            uptime: process.uptime()
        };
    }

    private calculateAverageResponseTime(): number {
        let totalTime = 0;
        let totalCount = 0;

        for (const metrics of this.metrics.values()) {
            for (const metric of metrics) {
                if (metric.type === 'duration') {
                    totalTime += metric.value;
                    totalCount++;
                }
            }
        }

        return totalCount > 0 ? totalTime / totalCount : 0;
    }
}

// 类型定义
interface OperationTimer {
    operationName: string;
    startTime: number;
    startMemory: NodeJS.MemoryUsage;
    metadata: Record<string, any>;
}

interface PerformanceResult {
    operationName: string;
    duration: number;
    memoryDelta: {
        rss: number;
        heapUsed: number;
        heapTotal: number;
    };
    timestamp: number;
    metadata: Record<string, any>;
}

interface PerformanceMetric {
    value: number;
    type: 'duration' | 'counter' | 'gauge';
    timestamp: number;
    metadata?: Record<string, any>;
}

interface PerformanceStats {
    count: number;
    average: number;
    min: number;
    max: number;
    p50: number;
    p95: number;
    p99: number;
}
```

## 8. 测试策略

### 8.1 测试框架配置

```typescript
// config/jest.config.js
module.exports = {
    preset: 'ts-jest',
    testEnvironment: 'node',
    roots: ['<rootDir>/src', '<rootDir>/test'],
    testMatch: [
        '**/__tests__/**/*.test.ts',
        '**/?(*.)+(spec|test).ts'
    ],
    transform: {
        '^.+\\.ts$': 'ts-jest'
    },
    collectCoverageFrom: [
        'src/**/*.ts',
        '!src/**/*.d.ts',
        '!src/types/**/*',
        '!src/constants/**/*'
    ],
    coverageDirectory: 'coverage',
    coverageReporters: ['text', 'lcov', 'html'],
    coverageThreshold: {
        global: {
            branches: 80,
            functions: 80,
            lines: 80,
            statements: 80
        }
    },
    setupFilesAfterEnv: ['<rootDir>/test/setup.ts'],
    testTimeout: 10000,
    verbose: true
};
```

### 8.2 单元测试示例

```typescript
// test/unit/core/association/AssociationManager.test.ts
/**
 * @fileoverview AssociationManager 单元测试
 * @author 技术团队
 * @since v1.0.0
 */

import { AssociationManager } from '../../../../src/core/association/AssociationManager';
import { AssociationType } from '../../../../src/types/core/AssociationTypes';
import { DualSpaceError, ErrorCodes } from '../../../../src/types/common/ErrorTypes';
import { createMockContext, createMockDatabase } from '../../../helpers/mock-factories';

describe('AssociationManager', () => {
    let associationManager: AssociationManager;
    let mockContext: any;
    let mockDatabasePath: string;

    beforeEach(() => {
        mockContext = createMockContext();
        mockDatabasePath = '/mock/path/associations.db';
        associationManager = new AssociationManager(mockContext, mockDatabasePath);
    });

    afterEach(() => {
        jest.clearAllMocks();
    });

    describe('createAssociation', () => {
        it('应该成功创建新的关联关系', async () => {
            // Arrange
            const sourceFile = '/path/to/source.ts';
            const targetFile = '/path/to/target.md';
            const options = {
                type: AssociationType.FUNCTION,
                description: '测试关联'
            };

            // Act
            const result = await associationManager.createAssociation(
                sourceFile,
                targetFile,
                options
            );

            // Assert
            expect(result).toBeDefined();
            expect(result.sourceFile).toBe(sourceFile);
            expect(result.targetFile).toBe(targetFile);
            expect(result.type).toBe(AssociationType.FUNCTION);
            expect(result.description).toBe('测试关联');
            expect(result.isActive).toBe(true);
            expect(result.id).toMatch(/^assoc_[a-f0-9]{12}$/);
        });

        it('应该在重复创建时抛出错误', async () => {
            // Arrange
            const sourceFile = '/path/to/source.ts';
            const targetFile = '/path/to/target.md';

            await associationManager.createAssociation(sourceFile, targetFile);

            // Act & Assert
            await expect(
                associationManager.createAssociation(sourceFile, targetFile)
            ).rejects.toThrow(DualSpaceError);

            await expect(
                associationManager.createAssociation(sourceFile, targetFile)
            ).rejects.toMatchObject({
                code: ErrorCodes.DUPLICATE_ASSOCIATION
            });
        });

        it('应该在无效输入时抛出验证错误', async () => {
            // Act & Assert
            await expect(
                associationManager.createAssociation('', '/path/to/target.md')
            ).rejects.toThrow(DualSpaceError);

            await expect(
                associationManager.createAssociation('/path/to/source.ts', '')
            ).rejects.toThrow(DualSpaceError);
        });
    });

    describe('getAssociationsByFile', () => {
        it('应该返回文件的所有关联关系', async () => {
            // Arrange
            const sourceFile = '/path/to/source.ts';
            const targetFile1 = '/path/to/target1.md';
            const targetFile2 = '/path/to/target2.md';

            await associationManager.createAssociation(sourceFile, targetFile1);
            await associationManager.createAssociation(sourceFile, targetFile2);

            // Act
            const associations = await associationManager.getAssociationsByFile(sourceFile);

            // Assert
            expect(associations).toHaveLength(2);
            expect(associations.every(a => a.sourceFile === sourceFile)).toBe(true);
        });

        it('应该只返回活跃的关联关系', async () => {
            // Arrange
            const sourceFile = '/path/to/source.ts';
            const targetFile = '/path/to/target.md';

            const association = await associationManager.createAssociation(sourceFile, targetFile);
            await associationManager.updateAssociation(association.id, { isActive: false });

            // Act
            const associations = await associationManager.getAssociationsByFile(sourceFile);

            // Assert
            expect(associations).toHaveLength(0);
        });
    });

    describe('updateAssociation', () => {
        it('应该成功更新关联关系', async () => {
            // Arrange
            const sourceFile = '/path/to/source.ts';
            const targetFile = '/path/to/target.md';
            const association = await associationManager.createAssociation(sourceFile, targetFile);

            const updates = {
                description: '更新后的描述',
                type: AssociationType.CLASS
            };

            // Act
            const updatedAssociation = await associationManager.updateAssociation(
                association.id,
                updates
            );

            // Assert
            expect(updatedAssociation.description).toBe('更新后的描述');
            expect(updatedAssociation.type).toBe(AssociationType.CLASS);
            expect(updatedAssociation.lastSyncTime).toBeGreaterThan(association.lastSyncTime);
        });

        it('应该在关联不存在时抛出错误', async () => {
            // Act & Assert
            await expect(
                associationManager.updateAssociation('non-existent-id', {})
            ).rejects.toThrow(DualSpaceError);

            await expect(
                associationManager.updateAssociation('non-existent-id', {})
            ).rejects.toMatchObject({
                code: ErrorCodes.ASSOCIATION_NOT_FOUND
            });
        });
    });

    describe('deleteAssociation', () => {
        it('应该成功删除关联关系', async () => {
            // Arrange
            const sourceFile = '/path/to/source.ts';
            const targetFile = '/path/to/target.md';
            const association = await associationManager.createAssociation(sourceFile, targetFile);

            // Act
            await associationManager.deleteAssociation(association.id);

            // Assert
            const associations = await associationManager.getAssociationsByFile(sourceFile);
            expect(associations).toHaveLength(0);
        });

        it('应该在删除不存在的关联时静默处理', async () => {
            // Act & Assert
            await expect(
                associationManager.deleteAssociation('non-existent-id')
            ).resolves.not.toThrow();
        });
    });

    describe('analyzeFileForAssociations', () => {
        it('应该分析文件并返回建议的关联', async () => {
            // Arrange
            const filePath = '/path/to/source.ts';
            
            // Mock 文件内容
            jest.spyOn(require('vscode').workspace, 'openTextDocument')
                .mockResolvedValue({
                    getText: () => `
                        /**
                         * @doc ./docs/function.md
                         */
                        function testFunction() {
                            return 'test';
                        }
                    `
                });

            // Act
            const suggestions = await associationManager.analyzeFileForAssociations(filePath);

            // Assert
            expect(suggestions).toHaveLength(1);
            expect(suggestions[0].sourceFile).toBe(filePath);
            expect(suggestions[0].type).toBe(AssociationType.FUNCTION);
        });

        it('应该在文件分析失败时抛出错误', async () => {
            // Arrange
            const filePath = '/path/to/nonexistent.ts';
            
            jest.spyOn(require('vscode').workspace, 'openTextDocument')
                .mockRejectedValue(new Error('File not found'));

            // Act & Assert
            await expect(
                associationManager.analyzeFileForAssociations(filePath)
            ).rejects.toThrow(DualSpaceError);
        });
    });
});
```

## 9. 部署和发布

### 9.1 构建配置

```javascript
// config/webpack.config.js
const path = require('path');
const webpack = require('webpack');

module.exports = (env, argv) => {
    const isProduction = argv.mode === 'production';

    return {
        target: 'node',
        entry: './src/extension.ts',
        output: {
            path: path.resolve(__dirname, '../dist'),
            filename: 'extension.js',
            libraryTarget: 'commonjs2',
            devtoolModuleFilenameTemplate: '../[resource-path]'
        },
        devtool: isProduction ? 'source-map' : 'eval-source-map',
        externals: {
            vscode: 'commonjs vscode',
            sqlite3: 'commonjs sqlite3',
            keytar: 'commonjs keytar'
        },
        resolve: {
            extensions: ['.ts', '.js'],
            alias: {
                '@': path.resolve(__dirname, '../src')
            }
        },
        module: {
            rules: [
                {
                    test: /\.ts$/,
                    exclude: /node_modules/,
                    use: [
                        {
                            loader: 'ts-loader',
                            options: {
                                compilerOptions: {
                                    sourceMap: true
                                }
                            }
                        }
                    ]
                }
            ]
        },
        plugins: [
            new webpack.DefinePlugin({
                'process.env.NODE_ENV': JSON.stringify(argv.mode || 'development')
            })
        ],
        optimization: {
            minimize: isProduction,
            splitChunks: false
        },
        performance: {
            hints: isProduction ? 'warning' : false,
            maxAssetSize: 1024 * 1024, // 1MB
            maxEntrypointSize: 1024 * 1024 // 1MB
        }
    };
};
```

### 9.2 发布脚本

```json
{
  "scripts": {
    "vscode:prepublish": "npm run compile",
    "compile": "webpack --mode production",
    "compile:dev": "webpack --mode development",
    "watch": "webpack --mode development --watch",
    "test": "npm run compile && node ./out/test/runTest.js",
    "test:unit": "jest --testPathPattern=unit",
    "test:integration": "jest --testPathPattern=integration",
    "test:e2e": "jest --testPathPattern=e2e",
    "test:coverage": "jest --coverage",
    "lint": "eslint src --ext ts",
    "lint:fix": "eslint src --ext ts --fix",
    "format": "prettier --write src/**/*.ts",
    "format:check": "prettier --check src/**/*.ts",
    "package": "vsce package",
    "publish": "vsce publish",
    "publish:pre": "vsce publish --pre-release",
    "clean": "rimraf dist out coverage",
    "build:analyze": "webpack-bundle-analyzer dist/extension.js",
    "security:audit": "npm audit --audit-level high",
    "docs:generate": "typedoc src --out docs/api",
    "version:patch": "npm version patch && git push --tags",
    "version:minor": "npm version minor && git push --tags",
    "version:major": "npm version major && git push --tags"
  }
}
```

## 10. 总结

本技术设计文档详细描述了IDE双空间智能管理插件的完整技术架构，包括：

### 10.1 核心特性
- **严格分层架构**: 扩展层、核心业务层、服务层、工具层的清晰分离
- **类型安全**: 完整的TypeScript类型系统和严格的类型检查
- **性能优化**: 多级缓存、性能监控、资源管理
- **安全设计**: API密钥加密存储、输入验证、XSS防护
- **可扩展性**: 插件化AI提供商、模块化设计

### 10.2 技术亮点
- **AI驱动**: 支持多个AI提供商的智能文档生成
- **实时同步**: 代码与文档的智能双向同步
- **关联管理**: 自动和手动的代码-文档关联
- **性能监控**: 全面的性能指标收集和分析
- **测试覆盖**: 80%以上的测试覆盖率要求

### 10.3 质量保证
- **代码规范**: ESLint + Prettier 强制代码格式
- **类型检查**: 严格的TypeScript配置
- **测试策略**: 单元测试、集成测试、E2E测试
- **安全审计**: 依赖漏洞扫描和安全检查
- **性能基准**: 响应时间和资源使用监控

### 10.4 开发流程
- **Git工作流**: 严格的分支管理和提交规范
- **代码审查**: 多层次的代码审查流程
- **持续集成**: 自动化测试和质量检查
- **发布管理**: 版本控制和发布流程

这个技术设计为插件的开发提供了坚实的基础，确保代码质量、性能和可维护性。所有开发人员都应严格遵循本文档中的架构设计和编码规范。