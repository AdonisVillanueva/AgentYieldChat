# Actions Documentation

## 🚀 Actions Overview
Actions are the fundamental building blocks of agent behavior in the Eliza framework. They represent discrete units of work that agents can perform, following a standardized interface for execution and management.

### Key Characteristics
- **Atomic Operations**: Each action performs a single, well-defined task
- **Composable**: Actions can be combined to create complex behaviors
- **Trackable**: Execution state and results are maintained
- **Extensible**: New actions can be easily added to the system

## Core Action Types

### Built-in Actions
- **DataProcessingAction**: Transform and manipulate data
- **APICallAction**: Make HTTP requests to external APIs
- **DecisionAction**: Conditional logic and branching
- **NotificationAction**: Send alerts and notifications

### Custom Actions
- **Domain-specific Operations**
- **Integration with External Systems**
- **Specialized Processing Tasks**
- **AI/ML Model Interactions**

## Action Interface
The `IAction` interface defines the core structure:

```typescript
interface IAction<T = any> {
    // Core identification
    actionId: string;
    actionType: ActionType;

    // Configuration
    config: ActionConfig;

    // Lifecycle methods
    initialize(): Promise<void>;
    shutdown(): Promise<void>;

    // Execution methods
    execute(context: ActionContext): Promise<ActionResult<T>>;
    validate(context: ActionContext): ActionValidationResult;

    // Error handling
    errorHandler: IActionErrorHandler;
}
```

### Interface Elements
- **Identification**: Unique action ID and type
- **Configuration**: Action-specific settings and parameters
- **Lifecycle Management**: Initialization and shutdown hooks
- **Execution**: Core action implementation
- **Validation**: Context validation and sanitization
- **Error Handling**: Standardized error management

## Implementation Patterns

### Basic Action Implementation
```typescript
class LogAction implements IAction<void> {
    actionId = 'log-action';
    actionType = ActionType.LOGGING;

    async initialize() {
        // Initialization logic
    }

    async execute(context: ActionContext) {
        console.log(context.message);
        return {
            success: true,
            data: undefined,
            metadata: {}
        };
    }

    validate(context: ActionContext) {
        return { valid: true, errors: [] };
    }
}
```

### Advanced Action with Retry Logic
```typescript
class RetryableAPIAction implements IAction<any> {
    private maxRetries = 3;
    private retryDelay = 1000;

    async execute(context: ActionContext) {
        let attempts = 0;
        while (attempts <= this.maxRetries) {
            try {
                const result = await this.makeAPICall(context);
                return {
                    success: true,
                    data: result,
                    metadata: { attempts: attempts + 1 }
                };
            } catch (error) {
                attempts++;
                if (attempts > this.maxRetries) {
                    throw error;
                }
                await new Promise(resolve => setTimeout(resolve, this.retryDelay));
            }
        }
    }
}
```

## Configuration Management

### Action Configuration Interface
```typescript
interface ActionConfig {
    timeout?: number;
    retryPolicy?: RetryPolicy;
    logging?: {
        enabled: boolean;
        level: 'info' | 'warn' | 'error';
    };
    validation?: {
        strict: boolean;
        schema?: any;
    };
}
```

### Configuration Best Practices
- Use environment variables for sensitive configurations
- Implement schema validation for configurations
- Provide sensible defaults
- Support runtime configuration updates

## Security Considerations

### Security Configuration
```typescript
interface ActionSecurityConfig {
    inputValidation: {
        enabled: boolean;
        sanitizationRules: any[];
    };
    outputValidation: {
        enabled: boolean;
        sanitizationRules: any[];
    };
    dataProtection: {
        encryption: boolean;
        algorithm?: string;
    };
}
```

### Security Best Practices
- Validate all inputs and outputs
- Use parameterized queries for database interactions
- Implement proper authentication for API calls
- Encrypt sensitive data in transit and at rest
- Implement proper error handling to avoid information leakage

## Error Handling

### Error Handling Interface
```typescript
interface IActionErrorHandler {
    handle(error: Error, context: ErrorContext): Promise<ActionResult>;
    isRetryable(error: Error): boolean;
    getRetryDelay(error: Error): number;
    shouldLog(error: Error): boolean;
}
```

### Error Handling Patterns
- **Transient Errors**: Automatic retries with exponential backoff
- **Permanent Errors**: Logging and graceful degradation
- **Validation Errors**: Detailed error messages
- **Rate Limiting**: Automatic throttling

## Usage Examples

### Basic Usage
```typescript
const logAction = new LogAction();
await logAction.initialize();

const result = await logAction.execute({
    message: 'Action executed successfully'
});
```

### Advanced Usage with Error Handling
```typescript
try {
    const apiAction = new APICallAction();
    await apiAction.initialize();

    const response = await apiAction.execute({
        endpoint: 'https://api.example.com/data',
        method: 'GET',
        headers: {
            'Content-Type': 'application/json'
        }
    });

    if (!response.success) {
        throw new Error('Action execution failed');
    }

    return response.data;
} catch (error) {
    await apiAction.errorHandler.handle(error, {
        actionId: apiAction.actionId,
        context: { /* execution context */ }
    });
    throw error;
}
```

## Best Practices
- Keep actions focused and single-purpose
- Implement proper error handling and logging
- Use configuration management effectively
- Follow security best practices
- Document action interfaces and usage
- Implement proper lifecycle management
- Consider performance implications
- Implement proper testing and monitoring
