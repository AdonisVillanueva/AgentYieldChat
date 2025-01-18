# Character File Documentation

## 🎭 Character File Overview
Character files are JSON configuration files that define the personality, behavior, and capabilities of Eliza agents. They serve as the blueprint for creating and customizing AI characters.

### Key Characteristics
- **JSON Format**: Standardized structure for easy creation and modification
- **Modular Design**: Components can be mixed and matched
- **Extensible**: Supports custom fields and configurations
- **Version Controlled**: Changes can be tracked and managed

## Core Components

### Basic Structure
```json
{
  "id": "unique-identifier",
  "name": "character_name",
  "modelProvider": "ModelProviderName",
  "clients": ["Client1", "Client2"],
  "settings": {
    "secrets": { "key": "value" },
    "voice": { "model": "VoiceModelName", "url": "VoiceModelURL" },
    "model": "CharacterModel",
    "embeddingModel": "EmbeddingModelName"
  },
  "bio": "Character biography or description",
  "lore": [
    "Storyline or backstory element 1",
    "Storyline or backstory element 2"
  ],
  "messageExamples": [["Message example 1", "Message example 2"]],
  "postExamples": ["Post example 1", "Post example 2"],
  "topics": ["Topic1", "Topic2"],
  "adjectives": ["Adjective1", "Adjective2"],
  "style": {
    "all": ["All style guidelines"],
    "chat": ["Chat-specific style guidelines"],
    "post": ["Post-specific style guidelines"]
  }
}
```

### Required Fields
- **id**: Unique identifier for the character
- **name**: Display name of the character
- **modelProvider**: AI model provider (e.g., anthropic, llama_local, openai)
- **clients**: Supported client types (e.g., discord, direct, twitter)

### Optional Fields
- **bio**: Character background and description
- **lore**: Detailed backstory and personality traits
- **knowledge**: Facts for Retrieval Augmented Generation (RAG)
- **messageExamples**: Sample conversations and interactions
- **postExamples**: Example social media posts
- **topics**: Areas of expertise and interest
- **adjectives**: Descriptive words that define the character
- **style**: Guidelines for tone and communication style

## Advanced Configuration

### Model Settings
```json
"settings": {
  "model": {
    "temperature": 0.7,
    "maxTokens": 1000,
    "topP": 0.9,
    "frequencyPenalty": 0.5,
    "presencePenalty": 0.5
  },
  "voice": {
    "model": "VoiceModelName",
    "url": "VoiceModelURL",
    "speed": 1.0,
    "pitch": 0.5
  }
}
```

### Knowledge Management
```json
"knowledge": {
  "sources": [
    {
      "type": "file",
      "path": "knowledge/file1.txt"
    },
    {
      "type": "url",
      "url": "https://example.com/knowledge"
    }
  ],
  "refreshInterval": 86400
}
```

## Best Practices

### Character Development
- Use randomization for variety in bio and lore
- Be specific with style instructions
- Include diverse message examples
- Cover common scenarios in examples
- Keep knowledge relevant and organized

### Performance Optimization
- Use efficient data structures
- Implement caching for frequently accessed data
- Optimize knowledge base size
- Use compression for large datasets

## Security Considerations

### Security Configuration
```json
"security": {
  "dataProtection": {
    "encryption": true,
    "algorithm": "AES-256"
  },
  "accessControl": {
    "read": ["user1", "user2"],
    "write": ["admin"]
  }
}
```

### Security Best Practices
- Validate all inputs and outputs
- Implement proper authentication
- Encrypt sensitive data
- Implement proper error handling
- Regularly update security configurations

## Usage Examples

### Basic Usage
```typescript
const character = new Character(characterFile);
await character.initialize();

const response = await character.interact({
  message: "Hello",
  context: { /* interaction context */ }
});
```

### Advanced Usage with Error Handling
```typescript
try {
  const character = new Character(characterFile);
  await character.initialize();

  const response = await character.generatePost({
    topic: "Technology",
    style: "professional"
  });

  if (!response.success) {
    throw new Error('Post generation failed');
  }

  return response.content;
} catch (error) {
  await character.errorHandler.handle(error, {
    characterId: character.id,
    context: { /* error context */ }
  });
  throw error;
}
```

## Maintenance and Versioning

### Version Control
```json
"version": {
  "major": 1,
  "minor": 0,
  "patch": 0,
  "compatibility": {
    "min": "1.0.0",
    "max": "2.0.0"
  }
}
```

### Maintenance Best Practices
- Implement proper version control
- Document all changes
- Maintain backward compatibility
- Regularly update dependencies
- Implement proper testing

## Troubleshooting

### Common Issues
- Invalid JSON format
- Missing required fields
- Model provider configuration errors
- Knowledge base loading failures
- Style guideline conflicts

### Debugging Tips
- Validate JSON structure
- Check field requirements
- Verify model provider settings
- Test knowledge base loading
- Review style guidelines
