# Secrets Scanner Agent

A security agent that scans code repositories for accidentally committed secrets, API keys, passwords, and other sensitive information.

## Description

The Secrets Scanner agent performs comprehensive scans of your codebase to detect hardcoded secrets, credentials, API keys, and other sensitive data that should not be in version control. It provides detailed reports with severity levels and actionable remediation steps.

## Use Cases

This agent is useful for:

- **Pre-Commit Scanning**: Catch secrets before they're committed
- **Pull Request Reviews**: Verify PRs don't contain secrets
- **Security Audits**: Regular scans of entire repositories
- **Incident Response**: Quick verification after potential exposure
- **Compliance**: Meet security and compliance requirements

## Prerequisites

Before using this agent, ensure:

- [ ] You have read access to the repository
- [ ] You understand your organization's secret management policies
- [ ] You know how to rotate credentials if secrets are found

## Installation

To use this agent in your repository:

1. Copy the agent directory to your repository:
   ```bash
   cp -r agents/security/secrets-scanner .github/agents/
   ```

2. Optionally, set up as a pre-commit hook or CI/CD check

## Detected Secret Types

### High Priority
- ✅ AWS Access Keys and Secret Keys
- ✅ GitHub Personal Access Tokens
- ✅ Slack API Tokens
- ✅ Google Cloud API Keys
- ✅ Azure Connection Strings
- ✅ Private SSH/SSL Keys
- ✅ Database Credentials
- ✅ JWT Secrets

### Medium Priority
- ✅ Generic API Keys
- ✅ OAuth Tokens
- ✅ SMTP Credentials
- ✅ FTP Credentials
- ✅ Webhook URLs with tokens

### Low Priority (Context-Dependent)
- ⚠️ Password variables
- ⚠️ Email addresses
- ⚠️ Phone numbers

## Usage

### Basic Repository Scan

```
Scan this repository for any accidentally committed secrets
```

### Scan Specific Files

```
Check the config/ directory for any exposed credentials
```

### Scan Recent Changes

```
Scan the changes in the last 5 commits for secrets
```

### Verify Remediation

```
Verify that the API keys have been removed from all files
```

### Example 1: Full Repository Scan

**Input:**
```
Scan this repository for secrets and provide a detailed report
```

**Output:**
```
# Secrets Scan Report

## Summary
- Files Scanned: 247
- Secrets Found: 3
- Critical: 2
- Warnings: 1

## Critical Findings

### Finding 1: AWS Access Key
**File**: config/production.yml
**Line**: 23
**Type**: AWS Access Key (AKIA)
**Pattern**: AKIA****************ABCD
**Severity**: 🔴 CRITICAL

**Impact**: Full AWS account access
**Action Required**: 
1. Rotate this key immediately in AWS IAM
2. Remove from git history (see remediation below)
3. Audit AWS CloudTrail for unauthorized access

---

### Finding 2: GitHub Personal Access Token
**File**: scripts/deploy.sh
**Line**: 15
**Type**: GitHub PAT
**Pattern**: ghp_****************************WXYZ
**Severity**: 🔴 CRITICAL

**Impact**: Full repository access
**Action Required**:
1. Revoke token in GitHub settings immediately
2. Generate new token
3. Store in GitHub Secrets or environment variable
4. Clean from git history

## Warnings

### Warning 1: Potential Database Password
**File**: src/config.example.js
**Line**: 45
**Type**: Password String
**Pattern**: password: "example123"
**Severity**: 🟡 WARNING

**Note**: This appears to be an example file. Verify it's not used in production.

## Remediation Steps

### Immediate Actions (Do Now)
1. ✅ Rotate all exposed credentials immediately
2. ✅ Verify no unauthorized access occurred
3. ✅ Remove secrets from latest commit

### Git History Cleanup
To remove secrets from git history:

```bash
# Using git-filter-repo (recommended)
git filter-repo --path config/production.yml --invert-paths

# Or using BFG Repo Cleaner
bfg --delete-files config/production.yml
```

⚠️ **Warning**: This rewrites history. Coordinate with team.

### Prevent Future Incidents

1. Add to .gitignore:
```
.env
.env.*
config/production.yml
**/secrets.yml
```

2. Set up pre-commit hooks:
```bash
# Install pre-commit
pip install pre-commit

# Add secrets scanning hook
# (Configuration example provided)
```

3. Use proper secret management:
- GitHub Secrets for CI/CD
- AWS Secrets Manager for production
- Environment variables for local development

## Resources
- Git History Cleaning: https://github.com/newren/git-filter-repo
- Secret Management Best Practices: [Internal Wiki]
- Incident Response: security@company.com
```

### Example 2: Targeted Scan

**Input:**
```
Scan only .env files and configuration files for secrets
```

**Output:**
```
# Targeted Scan: Configuration Files

## Files Scanned
- .env.example
- config/database.yml
- config/api.yml
- .env.local (IGNORED - in .gitignore ✅)

## Findings

### No Critical Issues Found ✅

## Recommendations
- ✅ .env.local is properly gitignored
- ✅ .env.example contains only placeholders
- ✅ Config files use environment variable references

All configuration files are properly secured.
```

## Severity Levels

| Level | Icon | Description | Action |
|-------|------|-------------|--------|
| Critical | 🔴 | Active secrets exposed | Immediate rotation required |
| High | 🟠 | Likely secrets, needs review | Rotate if confirmed |
| Medium | 🟡 | Suspicious patterns | Manual verification needed |
| Low | 🔵 | Potential false positives | Review recommended |
| Info | ℹ️ | Best practice suggestions | Consider implementing |

## False Positives

The agent attempts to filter common false positives:

✅ **Automatically Ignored**:
- Example/placeholder values
- Test fixtures with dummy data
- Documentation examples clearly marked
- Values like "your-api-key-here"

⚠️ **Manual Review Needed**:
- Base64 encoded strings
- UUID-like strings
- Generic secret patterns in unusual files

## Performance Considerations

Scan times (approximate):
- **Small repo** (<100 files): 30 seconds
- **Medium repo** (100-1000 files): 1-2 minutes
- **Large repo** (>1000 files): 3-5 minutes

## Limitations

- **Cannot Detect**: Obfuscated or heavily encrypted secrets
- **Cannot Verify**: Whether detected secrets are still active
- **Cannot Remediate**: Does not automatically rotate secrets
- **Context Limited**: May flag false positives in test data

## Troubleshooting

### Issue: Too Many False Positives

**Solution**: Use more specific scan requests:
```
Scan for AWS and GitHub secrets only, ignoring test files
```

### Issue: Scan Takes Too Long

**Solution**: Scan specific directories:
```
Scan only the src/ and config/ directories
```

## Integration

### CI/CD Pipeline

Add to GitHub Actions workflow:
```yaml
- name: Scan for Secrets
  run: |
    # Invoke secrets scanner agent
    # Fail build if secrets found
```

### Pre-Commit Hook

```yaml
# .pre-commit-config.yaml
repos:
  - repo: local
    hooks:
      - id: secrets-scan
        name: Scan for secrets
        entry: scan-secrets.sh
        language: script
```

## Security Notes

This agent:
- Does not store or transmit secrets
- Only requires read access
- Operates within GitHub security boundaries
- Reports are not logged externally

## Compliance

Helps meet requirements for:
- SOC 2 (Access Control)
- PCI DSS (Credential Protection)
- GDPR (Data Protection)
- Internal security policies

## Version History

See [CHANGELOG.md](./CHANGELOG.md)

## Contributing

To improve secret detection patterns, submit a pull request following the [contribution guidelines](../../../CONTRIBUTING.md).

## Support

- **Security Incidents**: security-incidents@company.com (URGENT)
- **Questions**: #security Slack channel
- **Email**: security@company.com

## License

Proprietary - Internal use only

## Author

**Team**: Information Security  
**Email**: security@company.com  
**Last Updated**: 2025-10-26
