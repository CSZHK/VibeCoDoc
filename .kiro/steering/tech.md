# Technology Stack & Build System

## Core Technology Stack

### Plugin Development
- **Primary Language**: TypeScript
- **IDE Integration**: VSCode Extension API (primary), with plans for Cursor and other IDEs
- **Runtime**: Node.js
- **UI Framework**: Vue.js + Electron for complex interfaces
- **Styling**: CSS with VSCode theme integration

### Backend Services
- **Core Engine**: Node.js + TypeScript
- **Database**: SQLite for local storage, JSON for configuration
- **File System**: Native Node.js fs with VSCode workspace APIs
- **AI Integration**: Multiple providers (OpenAI, Claude, local models)

### AI & ML Integration
- **Primary AI**: OpenAI GPT models via REST API
- **Secondary**: Anthropic Claude, local models (Ollama)
- **Fallback**: Template-based generation for offline scenarios
- **Caching**: Local response caching to reduce API costs

### Data Storage
- **Configuration**: JSON files in `.dual-space/` directory
- **Associations**: SQLite database for relationship mapping
- **Cache**: File-based caching for AI responses and analysis
- **User Settings**: VSCode workspace/user settings integration

## Build System & Development

### Package Management
```bash
# Install dependencies
npm install

# Development build with watch
npm run dev

# Production build
npm run build

# Package extension
npm run package

# Run tests
npm run test
```

### Key Scripts
- `npm run dev` - Development mode with hot reload
- `npm run build` - Production build for distribution
- `npm run test` - Run unit and integration tests
- `npm run lint` - Code quality checks with ESLint
- `npm run package` - Create .vsix extension package
- `npm run publish` - Publish to VSCode Marketplace

### Development Workflow
1. **Local Development**: Use `npm run dev` for live development
2. **Testing**: Run `npm run test` before commits
3. **Building**: Use `npm run build` for production builds
4. **Packaging**: Create distributable with `npm run package`

### Project Structure
```
├── src/                    # TypeScript source code
│   ├── extension.ts       # Main extension entry point
│   ├── core/              # Core business logic
│   ├── providers/         # VSCode providers and views
│   ├── services/          # AI and external services
│   └── utils/             # Utility functions
├── media/                 # Web assets (CSS, JS, images)
├── package.json           # Extension manifest and dependencies
├── tsconfig.json          # TypeScript configuration
├── webpack.config.js      # Build configuration
└── .dual-space/           # Plugin configuration directory
```

### Dependencies
- **VSCode API**: Core extension functionality
- **sqlite3**: Local database for associations
- **axios**: HTTP client for AI API calls
- **markdown-it**: Markdown parsing and rendering
- **monaco-editor**: Code editor integration
- **webpack**: Module bundling and build process

### Testing Framework
- **Unit Tests**: Jest for core logic testing
- **Integration Tests**: VSCode extension testing framework
- **E2E Tests**: Automated user workflow testing
- **Performance Tests**: Memory and response time benchmarks

### Code Quality
- **ESLint**: Code linting with TypeScript rules
- **Prettier**: Code formatting consistency
- **Husky**: Git hooks for pre-commit checks
- **Coverage**: Minimum 80% test coverage requirement

## AI Service Configuration

### API Keys Management
Store API keys in VSCode settings:
```json
{
  "dualSpace.ai.openai.apiKey": "your-openai-key",
  "dualSpace.ai.claude.apiKey": "your-claude-key",
  "dualSpace.ai.provider": "openai"
}
```

### Local AI Setup
For local AI models using Ollama:
```bash
# Install Ollama
curl -fsSL https://ollama.ai/install.sh | sh

# Pull a model
ollama pull codellama

# Configure in settings
{
  "dualSpace.ai.provider": "local",
  "dualSpace.ai.local.endpoint": "http://localhost:11434"
}
```

## Performance Considerations
- **Lazy Loading**: Load components and features on demand
- **Caching**: Aggressive caching of AI responses and file analysis
- **Debouncing**: Limit API calls with intelligent debouncing
- **Memory Management**: Efficient cleanup of resources and listeners
- **Background Processing**: Use web workers for heavy computations