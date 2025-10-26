# Custom Agent Template

Use this template as a starting point for creating new custom agents.

## Agent Definition

Create a file named `agent.yml` or `agent.yaml`:

```yaml
# Basic Information
name: your-agent-name
version: 1.0.0
category: development  # development, security, documentation, testing, operations
description: A concise description of what this agent does

# Author Information
author:
  name: Your Name
  email: your.email@company.com
  team: Your Team Name
  date: 2025-10-26

# Agent Instructions
instructions: |
  Provide detailed instructions for the agent's behavior.
  
  Be specific about:
  - What the agent should do
  - How it should approach the task
  - What tools it can use
  - How it should handle errors
  - What output it should produce
  
  You can use markdown formatting in instructions:
  
  ## Step 1: Analysis
  First, analyze the input to understand...
  
  ## Step 2: Processing
  Then, process the data by...
  
  ## Step 3: Output
  Finally, generate output that...

# Tools the agent can use (optional)
tools:
  - bash
  - view
  - edit
  - create
  # Add other tools as needed

# Configuration options (optional)
config:
  max_iterations: 5
  timeout_seconds: 300
  verbose: false

# Examples of how to use this agent
examples:
  - description: Basic usage example
    input: "Sample input for the agent"
    expected_output: "What the agent should produce"
    
  - description: Advanced usage example
    input: "More complex input scenario"
    expected_output: "Expected advanced output"

# Known limitations
limitations:
  - "Cannot process files larger than 10MB"
  - "Requires specific file format"
  - "Does not support language X"

# Dependencies (optional)
dependencies:
  - name: tool-or-library-name
    version: ">=1.0.0"
    optional: false
    purpose: "Why this dependency is needed"

# Tags for discoverability
tags:
  - keyword1
  - keyword2
  - keyword3

# Changelog
changelog:
  - version: 1.0.0
    date: 2025-10-26
    changes:
      - Initial release
```

## README Template

Create a `README.md` file:

```markdown
# Agent Name

Brief description of what this agent does.

## Description

Comprehensive description of the agent's purpose, capabilities, and use cases.

## Use Cases

This agent is useful for:

- **Use Case 1**: Description of when and why to use this
- **Use Case 2**: Another scenario where this agent helps
- **Use Case 3**: Additional use case

## Prerequisites

Before using this agent, ensure:

- [ ] Prerequisite 1 (e.g., specific tool installed)
- [ ] Prerequisite 2 (e.g., specific permissions)
- [ ] Prerequisite 3 (e.g., configuration completed)

## Installation

To use this agent in your repository:

1. Copy the agent directory to your repository:
   ```bash
   cp -r path/to/this/agent .github/agents/
   ```

2. Configure required settings (if any):
   ```bash
   # Add configuration steps
   ```

3. Verify installation:
   ```bash
   # Add verification steps
   ```

## Configuration

### Required Configuration

- `CONFIG_OPTION_1`: Description of what this configures
- `CONFIG_OPTION_2`: Description of what this configures

### Optional Configuration

- `OPTIONAL_CONFIG_1`: Description (default: value)
- `OPTIONAL_CONFIG_2`: Description (default: value)

## Usage

### Basic Usage

```bash
# Example command or invocation
```

Description of what happens.

### Advanced Usage

```bash
# Example of more complex usage
```

Description of advanced features.

### Example 1: [Specific Scenario]

**Input:**
```
Sample input
```

**Output:**
```
Expected output
```

**Explanation:** What the agent did and why.

### Example 2: [Another Scenario]

**Input:**
```
Another sample input
```

**Output:**
```
Expected output
```

**Explanation:** What the agent did and why.

## Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| option1 | string | "default" | What this option controls |
| option2 | number | 100 | What this option controls |
| option3 | boolean | true | What this option controls |

## Limitations

- **Limitation 1**: Description and workaround if available
- **Limitation 2**: Description and workaround if available
- **Limitation 3**: Description and workaround if available

## Troubleshooting

### Issue: Common Problem 1

**Symptoms:** What the user sees

**Cause:** Why this happens

**Solution:**
1. Step 1
2. Step 2
3. Step 3

### Issue: Common Problem 2

**Symptoms:** What the user sees

**Cause:** Why this happens

**Solution:**
1. Step 1
2. Step 2
3. Step 3

## Performance Considerations

- Expected execution time: X seconds for typical use case
- Resource usage: Memory, CPU, etc.
- Scaling characteristics: How it performs with larger inputs

## Security Notes

- Security consideration 1
- Security consideration 2
- Required permissions: List of required permissions

## Version History

See [CHANGELOG.md](./CHANGELOG.md) for detailed version history.

## Contributing

To contribute improvements to this agent:

1. Follow the [contribution guidelines](../../CONTRIBUTING.md)
2. Test your changes thoroughly
3. Update documentation
4. Submit a pull request

## Support

- **Issues**: Report issues in the main repository
- **Questions**: Ask in the #custom-agents Slack channel
- **Email**: agents-support@company.com

## License

Proprietary - Internal use only

## Author

**Name**: Your Name  
**Team**: Your Team  
**Email**: your.email@company.com  
**Last Updated**: 2025-10-26
```

## CHANGELOG Template

Create a `CHANGELOG.md` file:

```markdown
# Changelog

All notable changes to this agent will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Future features planned

## [1.0.0] - 2025-10-26

### Added
- Initial release
- Feature 1
- Feature 2
- Feature 3

### Changed
- N/A (initial release)

### Deprecated
- N/A (initial release)

### Removed
- N/A (initial release)

### Fixed
- N/A (initial release)

### Security
- N/A (initial release)
```

## Directory Structure

Your agent directory should look like:

```
your-agent-name/
├── agent.yml              # Agent definition (required)
├── README.md              # Documentation (required)
├── CHANGELOG.md           # Version history (required)
├── examples/              # Usage examples (optional)
│   ├── example1.md
│   └── example2.md
├── tests/                 # Test cases (recommended)
│   ├── test1.md
│   └── test2.md
└── assets/                # Supporting files (optional)
    ├── diagram.png
    └── config.example.yml
```

## Next Steps

1. Fill in all sections of the template
2. Test your agent thoroughly
3. Get peer review
4. Submit for governance review
5. Address any feedback
6. Celebrate when approved! 🎉

## Questions?

Contact: governance-team@company.com
