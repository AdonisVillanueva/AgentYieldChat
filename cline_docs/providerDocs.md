# Providers Documentation

## 🛠️ Providers Overview
Providers are the service integration layer of the Eliza framework, enabling agents to interact with external systems and data sources. They follow a plugin architecture that allows for easy extension and customization.

### Key Functions
- **Data Access**: Connect to external APIs and data sources
- **Service Integration**: Provide standardized interfaces for common services
- **Abstraction Layer**: Simplify complex operations into reusable components
- **Security Management**: Handle authentication and secure data transmission

## Core Provider Types

### Built-in Providers
- **TimeProvider**: Date/time operations and scheduling
- **WalletProvider**: Blockchain wallet interactions
- **DataProvider**: Generic data access and manipulation
- **AuthProvider**: Authentication and authorization services

### Custom Providers
- **Third-party API Integrations**
- **Blockchain Interactions**
- **Specialized Data Processors**
- **AI Service Wrappers**

## Provider Interface
The `IProvider` interface defines the core structure:

```typescript
interface IProvider<T = any> {
    // Core identification
    providerId: string;
    providerType: ProviderType;

    // Configuration
    config: ProviderConfig;

    // Lifecycle methods
    initialize(): Promise<void>;
    shutdown(): Promise<void>;

    // Core functionality
    execute(request: ProviderRequest): Promise<ProviderResponse<T>>;
    validate(request: ProviderRequest): ProviderValidationResult;

    // Error handling
    errorHandler: IErrorHandler;
}
```

### Interface Elements
- **Identification**: Unique provider ID and type
- **Configuration**: Provider-specific settings and parameters
- **Lifecycle Management**: Initialization and shutdown hooks
- **Execution**: Core service implementation
- **Validation**: Request validation and sanitization
- **Error Handling**: Standardized error management

## Implementation Patterns

### Basic Provider Implementation
```typescript
class TimeProvider implements IProvider<Date> {
    providerId = 'time-provider';
    providerType = ProviderType.TIME;

    async initialize() {
        // Initialization logic
    }

    async execute(request: ProviderRequest) {
        return {
            success: true,
            data: new Date(),
            metadata: {}
        };
    }

    validate(request: ProviderRequest) {
        return { valid: true, errors: [] };
    }
}
```

### Advanced Provider with Caching
```typescript
class CachedDataProvider implements IProvider<any> {
    private cache: Map<string, any> = new Map();

    async execute(request: ProviderRequest) {
        const cacheKey = this.generateCacheKey(request);

        if (this.cache.has(cacheKey)) {
            return this.cache.get(cacheKey);
        }

        const result = await this.fetchData(request);
        this.cache.set(cacheKey, result);
        return result;
    }
}
```

## Configuration Management

### Provider Configuration Interface
```typescript
interface ProviderConfig {
    endpoint?: string;
    apiKey?: string;
    cacheTTL?: number;
    retryPolicy?: RetryPolicy;
    timeout?: number;
    security?: SecurityConfig;
}
```

### Configuration Best Practices
- Use environment variables for sensitive data
- Implement validation for all configurations
- Provide default values where appropriate
- Support runtime configuration updates

## Security Considerations

### Security Configuration
```typescript
interface SecurityConfig {
    encryption: {
        enabled: boolean;
        algorithm: string;
    };
    authentication: {
        type: 'apiKey' | 'oauth2' | 'jwt';
        credentials: any;
    };
    rateLimiting: {
        enabled: boolean;
        requestsPerMinute: number;
    };
}
```

### Security Best Practices
- Always use HTTPS for external connections
- Implement proper authentication mechanisms
- Validate and sanitize all inputs
- Use encryption for sensitive data
- Implement rate limiting and throttling

## Error Handling

### Error Handling Interface
```typescript
interface IErrorHandler {
    handle(error: Error, context: ErrorContext): Promise<ProviderResponse>;
    isRetryable(error: Error): boolean;
    getRetryDelay(error: Error): number;
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
const timeProvider = new TimeProvider();
await timeProvider.initialize();

const response = await timeProvider.execute({
    action: 'getCurrentTime',
    parameters: {}
});
```

### Advanced Usage with Error Handling
```typescript
try {
    const dataProvider = new DataProvider();
    await dataProvider.initialize();

    const response = await dataProvider.execute({
        action: 'fetchData',
        parameters: { query: 'someQuery' }
    });

    if (!response.success) {
        throw new Error('Provider execution failed');
    }

    return response.data;
} catch (error) {
    await dataProvider.errorHandler.handle(error, {
        providerId: dataProvider.providerId,
        request: { /* request details */ }
    });
    throw error;
}
```

## Best Practices
- Implement proper error handling and logging
- Use caching where appropriate
- Follow security best practices
- Document provider interfaces and usage
- Implement proper lifecycle management
- Use configuration management effectively
- Consider performance implications
- Implement proper testing and monitoring
