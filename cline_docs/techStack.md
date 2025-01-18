## Database Configuration

- **Type**: SQLite
- **Location**: agent/data/db.sqlite
- **Purpose**: Stores character-specific data for agentyield system
- **Integration**:
  - Accessed via agent/src/index.ts
  - Linked to agentyield.character.json configuration
  - Used for persistent storage of character state and interactions

## Build System

- **TurboRepo** for monorepo management
- **PNPM** for package management
- **Build Error Resolution**:
  - When encountering build failures after pull requests/merges:
    1. Delete pnpm-lock.yaml
    2. Run `pnpm install` to regenerate lockfile
    3. Rebuild affected packages
  - Common causes:
    - Dependency version conflicts
    - Inconsistent lockfile state
    - Missing peer dependencies
