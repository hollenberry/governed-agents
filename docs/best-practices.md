# Best Practices for Custom Agents

## Introduction

This guide provides best practices for creating, maintaining, and using custom agents in the enterprise. Following these practices ensures high-quality, secure, and maintainable agents.

## Design Principles

### 1. Single Responsibility

Each agent should have one clear purpose.

✅ **Good**:
```yaml
name: python-code-reviewer
description: Reviews Python code for style, bugs, and best practices
```

❌ **Bad**:
```yaml
name: multi-purpose-agent
description: Reviews code, writes tests, generates docs, and deploys
```

**Why**: Focused agents are easier to understand, maintain, and reuse.

### 2. Clear Intent

Make the agent's purpose immediately obvious.

✅ **Good**:
```yaml
name: api-documentation-generator
description: Generates OpenAPI/Swagger documentation from code comments
```

❌ **Bad**:
```yaml
name: helper-agent
description: Helps with stuff
```

**Why**: Users should know exactly when to use each agent.

### 3. Composability

Design agents to work well with other agents.

✅ **Good**:
- Standardized inputs and outputs
- No hidden side effects
- Clear dependencies
- Modular design

**Why**: Enables building complex workflows from simple agents.

### 4. Defensive Programming

Anticipate and handle errors gracefully.

✅ **Good**:
```
If the file doesn't exist, create it.
If the format is invalid, provide clear error message.
If dependencies are missing, list them.
```

**Why**: Makes agents reliable and user-friendly.

## Documentation Standards

### README Template

Every agent should include:

```markdown
# Agent Name

## Description
Brief, clear description of what the agent does.

## Use Cases
- Use case 1
- Use case 2
- Use case 3

## Prerequisites
- Required tools
- Required permissions
- Required configurations

## Configuration
Explain configuration options

## Usage Examples

### Example 1: Basic Usage
Description and code

### Example 2: Advanced Usage
Description and code

## Limitations
- Limitation 1
- Limitation 2

## Troubleshooting
Common issues and solutions

## Support
How to get help
```

### Inline Documentation

For complex agent instructions:

```yaml
instructions: |
  # Overview
  This agent performs X, Y, and Z.
  
  # Step 1: Analyze
  First, analyze the input for...
  
  # Step 2: Process
  Then, process the data by...
  
  # Step 3: Output
  Finally, generate output that...
```

## Security Best Practices

### 1. No Hardcoded Secrets

❌ **Never**:
```yaml
instructions: |
  Connect to database using:
  username: admin
  password: secretpassword123
```

✅ **Always**:
```yaml
instructions: |
  Connect to database using environment variables:
  - DB_USERNAME
  - DB_PASSWORD
```

### 2. Input Validation

✅ **Always validate**:
```yaml
instructions: |
  Before processing the file:
  1. Verify file exists
  2. Check file size is reasonable
  3. Validate file type/extension
  4. Sanitize file path
```

### 3. Least Privilege

Request only necessary permissions:

✅ **Good**:
```yaml
permissions:
  - read: repository
  - write: pull_request_comments
```

❌ **Bad**:
```yaml
permissions:
  - admin: everything
```

### 4. Secure Dependencies

```yaml
dependencies:
  - name: trusted-library
    version: ">=2.0.0,<3.0.0"  # Pin version range
    source: official-registry   # Use official sources
```

## Performance Best Practices

### 1. Efficient Processing

✅ **Good**:
```yaml
instructions: |
  Process files in batches of 10
  Use streaming for large files
  Cache results when appropriate
```

❌ **Bad**:
```yaml
instructions: |
  Load entire file into memory
  Process one character at a time
  Recalculate everything on each run
```

### 2. Timeout Handling

```yaml
instructions: |
  Set appropriate timeouts:
  - Short operations: 30 seconds
  - Medium operations: 2 minutes
  - Long operations: 5 minutes
  
  If timeout occurs, provide partial results and clear status.
```

### 3. Resource Management

```yaml
instructions: |
  Clean up temporary files after processing
  Release locks when done
  Close connections properly
```

## Testing Best Practices

### 1. Test Coverage

Test these scenarios:
- ✅ Happy path (normal operation)
- ✅ Edge cases (boundary conditions)
- ✅ Error cases (invalid inputs)
- ✅ Performance (large inputs)
- ✅ Integration (with other systems)

### 2. Test Documentation

```markdown
## Testing

### Manual Testing
1. Create test repository
2. Run agent with input X
3. Verify output Y
4. Clean up test data

### Automated Testing
Run test suite: `npm test`
Expected results: All tests pass
```

### 3. Regression Testing

Before updates:
- Test existing functionality still works
- Verify backward compatibility
- Check for breaking changes

## Version Management

### Semantic Versioning

```
MAJOR.MINOR.PATCH

1.0.0 -> 1.0.1  (Bug fix)
1.0.1 -> 1.1.0  (New feature, backward compatible)
1.1.0 -> 2.0.0  (Breaking change)
```

### Changelog

Maintain CHANGELOG.md:

```markdown
# Changelog

## [1.2.0] - 2025-10-26
### Added
- New feature X

### Changed
- Improved performance of Y

### Fixed
- Bug in Z

## [1.1.0] - 2025-10-15
...
```

### Deprecation

When deprecating features:

```yaml
deprecated:
  version: "2.0.0"
  reason: "Replaced by new-agent-name"
  migration: "See migration-guide.md"
  end_of_life: "2026-01-01"
```

## Error Handling

### 1. Clear Error Messages

✅ **Good**:
```
Error: File 'config.yaml' not found.
Solution: Create config.yaml in the root directory.
Example: See templates/config.yaml.example
```

❌ **Bad**:
```
Error: File error
```

### 2. Graceful Degradation

```yaml
instructions: |
  If optional feature fails:
  1. Log the error
  2. Continue with core functionality
  3. Notify user of limitation
  4. Provide fallback option
```

### 3. Recovery Guidance

```yaml
instructions: |
  When encountering error:
  1. Explain what went wrong
  2. Suggest how to fix it
  3. Provide debugging steps
  4. Link to documentation
```

## Maintenance Best Practices

### 1. Regular Updates

- Review quarterly
- Update dependencies
- Fix reported bugs
- Improve documentation
- Enhance performance

### 2. Monitoring

Track these metrics:
- Usage frequency
- Success/failure rate
- Performance metrics
- User feedback
- Error patterns

### 3. Deprecation Process

1. Announce deprecation (3 months notice)
2. Provide migration path
3. Support during transition
4. Archive when usage drops
5. Document lessons learned

## Collaboration Best Practices

### 1. Code Review

When reviewing agents:
- Test functionality
- Check documentation
- Verify security
- Assess performance
- Validate examples

### 2. Knowledge Sharing

- Document design decisions
- Share lessons learned
- Present at team meetings
- Contribute to wiki
- Mentor new contributors

### 3. Community Engagement

- Respond to issues promptly
- Accept feedback graciously
- Collaborate on improvements
- Recognize contributors
- Build consensus

## Anti-Patterns to Avoid

### ❌ Over-Engineering

Don't create overly complex agents for simple tasks.

### ❌ Magic Behavior

Avoid implicit or hidden behavior. Make everything explicit.

### ❌ Tight Coupling

Don't hardcode dependencies on specific systems or configurations.

### ❌ Poor Naming

Use clear, descriptive names, not abbreviations or jargon.

### ❌ Incomplete Documentation

Always document all features, limitations, and requirements.

### ❌ Ignoring Feedback

Address user feedback and reported issues promptly.

## Examples of Excellent Agents

See these agents for reference:

1. `agents/development/python-linter` - Clean, focused, well-documented
2. `agents/security/secrets-scanner` - Comprehensive security checks
3. `agents/documentation/api-doc-generator` - Great examples and error handling

## Checklist for New Agents

Before submitting:

- [ ] Single, clear purpose
- [ ] Comprehensive README
- [ ] Multiple usage examples
- [ ] Error handling implemented
- [ ] Security review passed
- [ ] Performance tested
- [ ] No hardcoded secrets
- [ ] Dependencies documented
- [ ] Version number set
- [ ] Changelog created
- [ ] All tests passing
- [ ] Peer reviewed

## Resources

- [Governance Policy](governance-policy.md)
- [Approval Process](approval-process.md)
- [Security Guidelines](security-guidelines.md)
- [Agent Templates](../templates/)

---

**Questions?** Contact: governance-team@company.com

**Last Updated**: 2025-10-26
