# Troubleshooting Guide

## Module Resolution Issues

### Cannot find module '@elizaos/plugin-di' or its corresponding type declarations

**Symptoms:**
- TypeScript error in agent/src/index.ts
- Module not found error during compilation

**Solution:**
1. Ensure the package is built locally:
   ```bash
   cd packages/plugin-di
   pnpm build
   ```
2. Verify the built package exists in:
   ```bash
   packages/plugin-di/dist
   ```
3. Check that the package is properly linked in the workspace:
   ```bash
   pnpm -r list @elizaos/plugin-di
   ```
4. Restart TypeScript server in VSCode (Ctrl+Shift+P -> Restart TS server)

**Prevention:**
- Always build dependent packages before running the main application
- Use `pnpm -r build` to build all packages in the workspace
- Ensure proper TypeScript path mappings in tsconfig.json
