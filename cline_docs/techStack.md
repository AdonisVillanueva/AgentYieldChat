## Database Configuration

- **Type**: SQLite
- **Location**: agent/data/db.sqlite
- **Purpose**: Stores character-specific data for agentyield system
- **Integration**:
  - Accessed via agent/src/index.ts
  - Linked to agentyield.character.json configuration
  - Used for persistent storage of character state and interactions
