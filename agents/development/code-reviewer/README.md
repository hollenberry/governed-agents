# Code Reviewer Agent

An intelligent code review agent that provides comprehensive feedback on code changes, identifying issues, suggesting improvements, and ensuring best practices.

## Description

The Code Reviewer agent is designed to perform thorough code reviews across multiple programming languages. It analyzes code for quality, correctness, security, performance, and adherence to best practices. The agent provides structured, actionable feedback that helps developers improve their code before merging.

## Use Cases

This agent is useful for:

- **Pull Request Reviews**: Automated first-pass review of pull requests
- **Pre-Commit Checks**: Review code before committing to catch issues early
- **Learning Tool**: Help junior developers learn best practices through feedback
- **Consistency Enforcement**: Ensure code follows team standards and conventions
- **Security Scanning**: Identify common security vulnerabilities

## Prerequisites

Before using this agent, ensure:

- [ ] Code changes are committed or in a pull request
- [ ] You have read access to the repository
- [ ] The code is in a supported programming language

## Installation

To use this agent in your repository:

1. Copy the agent directory to your repository:
   ```bash
   cp -r agents/development/code-reviewer .github/agents/
   ```

2. The agent is ready to use - no additional configuration needed!

## Supported Languages

Primary support for:
- Python
- JavaScript / TypeScript
- Java
- C#
- Go
- Ruby

Partial support for most other common languages.

## Usage

### Basic Usage

Ask the agent to review code changes:

```
Review the code changes in this pull request
```

### Review Specific Files

```
Review the changes in src/api/handlers.py and src/utils/validator.js
```

### Focus on Specific Aspects

```
Review this code focusing on security and performance
```

### Example 1: Pull Request Review

**Input:**
```
Review the changes in PR #456, focusing on code quality and potential bugs
```

**Output:**
```
## Code Review Summary

I've reviewed the 3 files changed in this PR. Overall, the implementation is solid 
with a few areas for improvement.

## Strengths
✅ Clear variable naming and function structure
✅ Good error handling in most cases
✅ Comprehensive input validation

## Issues Found

### 🔴 Critical Issues

**File: src/api/user_handler.py, Line 45**
```python
# Current code
password = request.args.get('password')
user = authenticate(username, password)
```

Issue: Password passed in URL query parameters
Why: Passwords in URLs are logged and cached
Fix: Use POST request body instead:
```python
password = request.json.get('password')
user = authenticate(username, password)
```

### 🟡 Important Issues

**File: src/utils/cache.js, Line 23**
```javascript
// Current code
const data = await fetchData();
cache[key] = data;
```

Issue: No cache size limit or eviction policy
Why: Could lead to memory exhaustion
Fix: Implement LRU cache or size limits

### 🔵 Suggestions

**File: src/models/user.py, Line 15**
Consider using dataclasses for cleaner code:
```python
from dataclasses import dataclass

@dataclass
class User:
    id: int
    name: str
    email: str
```

## Overall Recommendation
🔄 Request Changes - Please address the critical security issue before merging.
```

### Example 2: Focused Security Review

**Input:**
```
Review src/auth/login.py for security vulnerabilities
```

**Output:**
```
## Security Review: src/auth/login.py

### 🔴 Critical Security Issues

1. **SQL Injection Vulnerability (Line 34)**
   Using string concatenation for SQL queries
   Fix: Use parameterized queries

2. **Weak Password Hashing (Line 56)**
   Using MD5 for password hashing
   Fix: Use bcrypt or Argon2

3. **Missing Rate Limiting (Line 12)**
   No protection against brute force attacks
   Fix: Implement rate limiting middleware

### Recommended Fixes
[Detailed code examples provided...]

### Security Checklist
- [ ] Input validation
- [ ] Output encoding  
- [x] HTTPS enforcement
- [ ] CSRF protection
- [ ] Rate limiting
```

## Review Criteria

The agent evaluates code across multiple dimensions:

### Code Quality
- Readability and clarity
- Naming conventions
- Code organization
- Comment quality

### Functionality
- Logic correctness
- Error handling
- Edge cases
- Performance

### Security
- Input validation
- Authentication/authorization
- Data sanitization
- Secure coding practices

### Best Practices
- DRY principle
- SOLID principles
- Design patterns
- Language idioms

## Limitations

- **Cannot Execute Code**: Reviews are static analysis only
- **Context Required**: May need business logic context for some reviews
- **Language Coverage**: Best results with primary supported languages
- **Large Changes**: Very large PRs may need to be reviewed in chunks

## Troubleshooting

### Issue: Generic or Vague Feedback

**Solution:** Provide more context about what you want reviewed:
```
Review this API handler, focusing on error handling and input validation
```

### Issue: Missing Language-Specific Issues

**Solution:** Specify the language or framework:
```
Review this React component for React-specific best practices
```

## Performance Considerations

- **Small PRs** (< 10 files): 1-2 minutes
- **Medium PRs** (10-50 files): 3-5 minutes  
- **Large PRs** (> 50 files): Consider reviewing in batches

## Security Notes

This agent:
- Does not modify code
- Only requires read access to repositories
- Does not store or transmit code externally
- Operates within GitHub's security boundaries

## Version History

See [CHANGELOG.md](./CHANGELOG.md)

## Contributing

To suggest improvements to this agent, please submit a pull request following the [contribution guidelines](../../../CONTRIBUTING.md).

## Support

- **Issues**: Report in the main repository
- **Questions**: #custom-agents Slack channel
- **Email**: agents-support@company.com

## License

Proprietary - Internal use only

## Author

**Team**: Enterprise Architecture Team  
**Email**: architecture@company.com  
**Last Updated**: 2025-10-26
