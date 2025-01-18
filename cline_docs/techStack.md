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
  - For monorepo-specific operations:
    - Use `pnpm install --lockfile-only` to generate lockfile without installing dependencies
- **Build Error Resolution**:
  - When encountering build failures after pull requests/merges:
    1. Delete pnpm-lock.yaml
    2. Run `pnpm install` to regenerate lockfile
    3. Rebuild affected packages
  - Common causes:
    - Dependency version conflicts
    - Inconsistent lockfile state
    - Missing peer dependencies

## Dependency Injection

- **Framework**: InversifyJS
- **Features**:
  - Decorator-based configuration
  - Type-safe dependency resolution
  - Modular architecture with:
    - Symbols for dependency identification
    - Factories for object creation
    - Templates for common patterns
  - Integration with core Eliza system
  - Support for:
    - Actions
    - Evaluators
    - Providers
- **Dependencies**:
  - @elizaos/core
  - reflect-metadata
  - zod for validation
  - uuid for instance identification
