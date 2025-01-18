## Database Integration

- **Component**: SQLite Database
- **Location**: agent/data/db.sqlite
- **Functionality**:
  - Persistent storage for agentyield character state
  - Transaction history and interaction logs
  - Configuration synchronization with agentyield.character.json
- **Dependencies**:
  - agent/src/index.ts (primary interface)
  - characters/agentyield.character.json (configuration source)

## Core Documentation

### Agent Runtime System
- **Overview**: Core component handling autonomous interactions through various clients
- **Key Features**:
  - Message and Memory Processing
  - State Management
  - Action Execution
  - Evaluation and Response
- **Core Components**:
  - Clients: Discord, Telegram, Direct (REST API)
  - Providers: Time, Wallet, Custom Data services
  - Actions: Follow rooms, Generate images, Process attachments
  - Evaluators: Message relevance, Goal management, Fact extraction
- **Memory Systems**:
  - Message History: Recent conversations
  - Factual Memory: User-specific facts
  - Knowledge Base: General knowledge
  - Relationship Tracking: User-agent interactions
  - RAG Integration: Vector-based contextual recall
- **State Management**:
  - Immutable state where possible
  - composeState for initial creation
  - updateRecentMessageState for updates
  - Caching for frequently accessed data

### GitHub Repository
- **elizaOS/eliza**: [elizaOS/eliza](https://github.com/elizaOS/eliza)
  - Core documentation and README
  - Architecture and development setup
  - Contribution guidelines
  - Example implementations
