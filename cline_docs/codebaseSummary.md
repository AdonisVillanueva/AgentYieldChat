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
