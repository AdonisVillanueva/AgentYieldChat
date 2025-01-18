# Evaluators Documentation

## 🧠 Evaluators Overview
Evaluators are the decision-making components of the Eliza framework, responsible for assessing conditions, making judgments, and determining the course of action. They provide a standardized way to evaluate various types of data and conditions.

### Key Characteristics
- **Condition Assessment**: Evaluate specific conditions or states
- **Decision Making**: Determine appropriate actions based on evaluations
- **Modular Design**: Can be combined and composed
- **Extensible**: New evaluators can be easily added

## Core Evaluator Types

### Built-in Evaluators
- **BooleanEvaluator**: Simple true/false evaluations
- **ThresholdEvaluator**: Compare values against thresholds
- **PatternEvaluator**: Match patterns in data
- **CompositeEvaluator**: Combine multiple evaluators

### Custom Evaluators
- **Domain-specific Evaluations**
- **Complex Decision Trees**
- **AI/ML-based Evaluations**
- **Real-time Data Analysis**

## Evaluator Interface
The `IEvaluator` interface defines the core structure:

```typescript
interface IEvaluator<T = any> {
    // Core identification
    evaluatorId: string;
    evaluatorType: EvaluatorType;

    // Configuration
    config: EvaluatorConfig;

    // Lifecycle methods
    initialize(): Promise<void>;
    shutdown(): Promise<void>;

    // Evaluation methods
    evaluate(context: EvaluationContext): Promise<EvaluationResult<T>>;
    validate(context: EvaluationContext): EvaluationValidationResult;

    // Error handling
    errorHandler: IEvaluatorErrorHandler;
}
```

### Interface Elements
- **Identification**: Unique evaluator ID and type
- **Configuration**: Evaluator-specific settings
- **Lifecycle Management**: Initialization and shutdown
- **Evaluation**: Core evaluation logic
- **Validation**: Context validation
- **Error Handling**: Standardized error management

## Implementation Patterns

### Basic Evaluator Implementation
```typescript
class ThresholdEvaluator implements IEvaluator<boolean> {
    evaluatorId = 'threshold-evaluator';
    evaluatorType = EvaluatorType.THRESHOLD;

    async initialize() {
        // Initialization logic
    }

    async evaluate(context: EvaluationContext) {
        const value = context.value;
        const threshold = context.threshold;
        return {
            success: true,
            result: value >= threshold,
            metadata: {}
        };
    }

    validate(context: EvaluationContext) {
        return { valid: true, errors: [] };
    }
}
```

### Advanced Evaluator with Multiple Conditions
```typescript
class CompositeEvaluator implements IEvaluator<boolean> {
    private evaluators: IEvaluator<boolean>[];

    async evaluate(context: EvaluationContext) {
        const results = await Promise.all(
            this.evaluators.map(e => e.evaluate(context))
        );

        return {
            success: true,
            result: results.every(r => r.result),
            metadata: {
                individualResults: results
            }
        };
    }
}
```

## Configuration Management

### Evaluator Configuration Interface
```typescript
interface EvaluatorConfig {
    evaluationStrategy?: 'strict' | 'lenient';
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
interface EvaluatorSecurityConfig {
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
- Implement proper authentication for external data sources
- Encrypt sensitive data in transit and at rest
- Implement proper error handling to avoid information leakage

## Error Handling

### Error Handling Interface
```typescript
interface IEvaluatorErrorHandler {
    handle(error: Error, context: ErrorContext): Promise<EvaluationResult>;
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
const thresholdEvaluator = new ThresholdEvaluator();
await thresholdEvaluator.initialize();

const result = await thresholdEvaluator.evaluate({
    value: 75,
    threshold: 50
});
```

### Advanced Usage with Error Handling
```typescript
try {
    const compositeEvaluator = new CompositeEvaluator();
    await compositeEvaluator.initialize();

    const response = await compositeEvaluator.evaluate({
        conditions: [
            { type: 'threshold', value: 75, threshold: 50 },
            { type: 'pattern', value: 'test', pattern: /test/ }
        ]
    });

    if (!response.success) {
        throw new Error('Evaluation failed');
    }

    return response.result;
} catch (error) {
    await compositeEvaluator.errorHandler.handle(error, {
        evaluatorId: compositeEvaluator.evaluatorId,
        context: { /* evaluation context */ }
    });
    throw error;
}
```

## Best Practices
- Keep evaluators focused and single-purpose
- Implement proper error handling and logging
- Use configuration management effectively
- Follow security best practices
- Document evaluator interfaces and usage
- Implement proper lifecycle management
- Consider performance implications
- Implement proper testing and monitoring
