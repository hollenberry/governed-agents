# Contributing to Enterprise Custom Agents

Thank you for your interest in contributing to the Enterprise Custom Agents repository! This guide will help you create high-quality agents that benefit the entire organization.

## Table of Contents

- [Getting Started](#getting-started)
- [Contribution Process](#contribution-process)
- [Agent Requirements](#agent-requirements)
- [Development Guidelines](#development-guidelines)
- [Testing Your Agent](#testing-your-agent)
- [Submission Checklist](#submission-checklist)
- [Review Process](#review-process)

## Getting Started

Before creating a new agent:

1. **Search existing agents**: Check if a similar agent already exists
2. **Review templates**: Familiarize yourself with the agent templates
3. **Read governance policies**: Understand the approval requirements
4. **Join the discussion**: Engage with the community via GitHub Discussions

## Contribution Process

### 1. Plan Your Agent

- Define the agent's purpose and scope
- Identify the target user persona
- List required tools and capabilities
- Document expected inputs and outputs

### 2. Create a Branch

```bash
git checkout -b agent/your-agent-name
```

### 3. Use a Template

Start with the appropriate template from the `templates/` directory:

```bash
cp -r templates/agent-template agents/category/your-agent-name
```

### 4. Develop Your Agent

- Write the agent definition file
- Create comprehensive documentation
- Add usage examples
- Include test cases

### 5. Test Thoroughly

- Test in a sandbox environment
- Validate all use cases
- Ensure error handling works
- Document any limitations

### 6. Submit Pull Request

- Fill out the PR template completely
- Tag relevant reviewers
- Link to any related issues
- Await review feedback

## Agent Requirements

All agents must meet these requirements:

### Mandatory

- ✅ **Clear purpose**: Single, well-defined responsibility
- ✅ **Documentation**: README with usage examples
- ✅ **Security**: No hardcoded secrets or credentials
- ✅ **Error handling**: Graceful failure modes
- ✅ **Metadata**: Version, author, and category information

### Recommended

- 📝 **Examples**: Multiple usage examples
- 🧪 **Tests**: Validation test cases
- 📊 **Performance**: Efficiency considerations
- 🔄 **Versioning**: Semantic versioning
- 🏷️ **Tags**: Relevant keywords for discoverability

## Development Guidelines

### Agent Definition Structure

```yaml
name: agent-name
version: 1.0.0
category: development
description: Brief description of what the agent does

author:
  name: Your Name
  email: your.email@company.com
  team: Your Team

instructions: |
  Detailed instructions for the agent's behavior
  and capabilities. Be specific and clear.

tools:
  - tool-name-1
  - tool-name-2

examples:
  - description: Example use case 1
    usage: How to use the agent for this case
  - description: Example use case 2
    usage: How to use the agent for this case
```

### Documentation Standards

Each agent must include a `README.md` with:

1. **Overview**: What the agent does
2. **Use Cases**: When to use this agent
3. **Requirements**: Prerequisites and dependencies
4. **Configuration**: How to configure the agent
5. **Examples**: Practical usage examples
6. **Limitations**: Known limitations or constraints
7. **Support**: How to get help

### Code Quality

- Follow organization coding standards
- Use clear, descriptive names
- Comment complex logic
- Keep instructions concise but complete
- Avoid overly complex agent definitions

### Security Requirements

- ❌ Never hardcode secrets or API keys
- ❌ Never access unauthorized systems
- ✅ Use environment variables for sensitive data
- ✅ Validate all inputs
- ✅ Follow principle of least privilege
- ✅ Document security considerations

## Testing Your Agent

### Pre-Submission Testing

1. **Functionality Test**: Verify the agent works as documented
2. **Edge Cases**: Test with unusual or invalid inputs
3. **Error Handling**: Ensure graceful failure
4. **Documentation**: Verify examples work correctly
5. **Security Scan**: Check for vulnerabilities

### Test Checklist

- [ ] Agent executes successfully in test environment
- [ ] All examples in README work as documented
- [ ] Error messages are clear and helpful
- [ ] No sensitive data is exposed
- [ ] Performance is acceptable
- [ ] Documentation is complete and accurate

## Submission Checklist

Before submitting your PR, ensure:

- [ ] Agent definition is complete and valid
- [ ] README.md is comprehensive
- [ ] Examples are tested and working
- [ ] No hardcoded secrets or credentials
- [ ] Version number follows semantic versioning
- [ ] Appropriate category is assigned
- [ ] Metadata is complete (author, description, etc.)
- [ ] Security guidelines are followed
- [ ] Agent has been tested in isolation
- [ ] All files are in correct directory structure

## Review Process

### Timeline

- **Initial Review**: Within 2 business days
- **Technical Review**: 3-5 business days
- **Security Review**: 3-5 business days
- **Final Approval**: 1-2 business days

### Review Criteria

Reviewers will evaluate:

1. **Functionality**: Does it work as intended?
2. **Documentation**: Is it clear and complete?
3. **Security**: Does it meet security standards?
4. **Quality**: Is the code well-written?
5. **Value**: Does it benefit the organization?
6. **Compliance**: Does it follow governance policies?

### Addressing Feedback

- Respond to all reviewer comments
- Make requested changes promptly
- Re-request review after updates
- Ask questions if feedback is unclear

### Approval

Once approved, your agent will be:

1. Merged to the main branch
2. Added to the agent catalog
3. Announced to the organization
4. Available for enterprise-wide use

## Questions?

- **Slack**: #custom-agents channel
- **Email**: agents-governance@company.com
- **Discussions**: GitHub Discussions in this repo

Thank you for contributing to our shared agent library! 🎉
