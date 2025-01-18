# Core Agents Documentation

## 🤖 Agents Overview
Agents are the core components of the Eliza framework that handle autonomous interactions. Each agent runs in a runtime environment and can interact through various clients (Discord, Telegram, etc.) while maintaining consistent behavior and memory.

### Key Functions
- **Message and Memory Processing**: Storing, retrieving, and managing conversation data and contextual memory
- **State Management**: Composing and updating the agent's state for coherent, ongoing interactions
- **Action Execution**: Handling behaviors like transcribing media, generating images, and following rooms
- **Evaluation and Response**: Assessing responses, managing goals, and extracting relevant information

## Core Components
Each agent runtime consists of key components that enable flexible and extensible functionality:

### Clients
Enable communication across platforms:
- Discord
- Telegram
- Direct (REST API)

### Providers
Extend agent capabilities through service integration:
- Time
- Wallet
- Custom Data services

### Actions
Define agent behaviors:
- Following rooms
- Generating images
- Processing attachments
- Custom actions for specific needs

### Evaluators
Manage agent responses by:
- Assessing message relevance
- Managing goals
- Extracting facts
- Building long-term memory

## AgentRuntime Interface
The `IAgentRuntime` interface defines the main structure of the runtime environment:

```typescript
interface IAgentRuntime {
    // Core identification
    agentId: UUID;
    serverUrl: string;
    token: string;

    // Configuration
    character: Character;
    modelProvider: ModelProviderName;

    // Components
    actions: Action[];
    evaluators: Evaluator[];
    providers: Provider[];

    // Database & Memory
    databaseAdapter: IDatabaseAdapter;
    messageManager: IMemoryManager;
    descriptionManager: IMemoryManager;
    loreManager: IMemoryManager;
}
```

### Interface Elements
- **Identification**: Agent ID, server URL, and token for authentication
- **Configuration**: Character profile and model provider define personality and language model
- **Components**: Actions, evaluators, and providers support extensible behaviors
- **Memory Management**: Specialized managers track conversations, descriptions, and static knowledge

## State Management
The runtime maintains state through the `State` interface:

```typescript
interface State {
    userId?: UUID;
    agentId?: UUID;
    roomId: UUID;
    bio: string;
    lore: string;
    agentName?: string;
    senderName?: string;
    actors: string;
    actorsData?: Actor[];
    recentMessages: string;
    recentMessagesData: Memory[];
    goals?: string;
    goalsData?: Goal[];
    actions?: string;
    actionNames?: string;
    providers?: string;
}
```

### State Management Methods
```typescript
// Compose initial state
const state = await runtime.composeState(message, {
    additionalContext: "custom-context"
});

// Update message state
const updatedState = await runtime.updateRecentMessageState(state);
```

### Best Practices
- Keep state immutable where possible
- Use `composeState` for initial state creation
- Use `updateRecentMessageState` for updates
- Cache frequently accessed state data

## Memory Systems
The Eliza framework uses multiple memory types:

### Memory Types
- **Message History**: Stores recent conversations for continuity
- **Factual Memory**: Holds user-specific facts and preferences
- **Knowledge Base**: Contains general knowledge and static character lore
- **Relationship Tracking**: Manages user-agent interaction history
- **RAG Integration**: Vector-based contextual recall using similarity matching

### Memory Managers
- `messageManager`: Conversation messages and responses
- `descriptionManager`: User descriptions and profiles
- `loreManager`: Static character knowledge

## Usage Examples

### Message Processing
```typescript
await runtime.processActions(message, responses, state, async (newMessages) => {
    // Handle new messages
    return [message];
});
```

### State Management
```typescript
const state = await runtime.composeState(message, {
    additionalContext: "custom-context"
});
```

### Memory Management
```typescript
const memoryManager = runtime.getMemoryManager("messages");
await memoryManager.createMemory({
    id: messageId,
    content: { text: "Message content" },
    userId,
    roomId
});
```

### Service Management
```typescript
// Register service
runtime.registerService(new TranscriptionService());

// Get service
const service = runtime.getService<ITranscriptionService>(
    ServiceType.TRANSCRIPTION
);
```

### Evaluation System
```typescript
const evaluationResults = await runtime.evaluate(message, state, didRespond);
```

## Best Practices
- Use appropriate memory managers for different data types
- Consider memory limits and clean up old memories periodically
- Use immutability in state management
- Log errors and maintain stability during service failures
- Use unique flag for deduplicated storage
