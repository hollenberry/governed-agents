# Security Guidelines for Custom Agents

## Overview

Security is paramount when developing and deploying custom agents. This document provides comprehensive security guidelines to ensure agents are safe, compliant, and protect organizational assets.

## Security Principles

### 1. Defense in Depth
Multiple layers of security controls to protect against threats.

### 2. Least Privilege
Agents should have minimum permissions necessary to function.

### 3. Secure by Default
Security should be built in, not added as an afterthought.

### 4. Fail Securely
When errors occur, fail in a secure manner that doesn't expose sensitive data.

### 5. Zero Trust
Never trust, always verify - even internal components.

## Secret Management

### ❌ What NOT to Do

**Never hardcode secrets:**
```yaml
# BAD - Never do this!
instructions: |
  API_KEY = "sk-abc123xyz789"
  DATABASE_PASSWORD = "MyP@ssw0rd123"
  AWS_SECRET_KEY = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
```

**Never commit secrets to git:**
```bash
# BAD - These should never be in version control
config/secrets.yaml
.env
credentials.json
```

### ✅ What TO Do

**Use environment variables:**
```yaml
# GOOD
instructions: |
  Use environment variables for sensitive data:
  - API_KEY: Read from $API_KEY
  - DATABASE_PASSWORD: Read from $DB_PASSWORD
  - AWS_SECRET_KEY: Read from $AWS_SECRET_ACCESS_KEY
```

**Use secret management systems:**
```yaml
# GOOD
instructions: |
  Retrieve secrets from approved secret management systems:
  - GitHub Secrets
  - Azure Key Vault
  - AWS Secrets Manager
  - HashiCorp Vault
```

**Document required secrets:**
```markdown
## Required Secrets

This agent requires the following secrets:

1. `GITHUB_TOKEN`: Personal access token with repo scope
2. `API_KEY`: Service API key from provider
3. `WEBHOOK_SECRET`: Webhook validation secret

Configure these in your GitHub repository secrets.
```

## Input Validation

### 1. Validate All Inputs

```yaml
instructions: |
  Before processing any input:
  
  1. Validate type (string, number, etc.)
  2. Check length/size limits
  3. Verify format (regex, schema)
  4. Sanitize special characters
  5. Reject unexpected inputs
```

### 2. Path Traversal Prevention

❌ **Vulnerable**:
```yaml
instructions: |
  Read file at path: ${user_input}
```

✅ **Secure**:
```yaml
instructions: |
  Before reading file:
  1. Validate path is within allowed directory
  2. Reject paths with ".." or absolute paths
  3. Canonicalize path
  4. Check file actually exists in safe location
```

### 3. Command Injection Prevention

❌ **Vulnerable**:
```yaml
instructions: |
  Run command: git clone ${repository_url}
```

✅ **Secure**:
```yaml
instructions: |
  Before executing commands:
  1. Validate repository URL format
  2. Use parameter arrays, not string concatenation
  3. Escape special characters
  4. Use allowlist for acceptable values
```

### 4. Code Injection Prevention

❌ **Vulnerable**:
```yaml
instructions: |
  Evaluate expression: eval(${user_formula})
```

✅ **Secure**:
```yaml
instructions: |
  Never use eval() or similar with user input
  Use safe parsers for expressions
  Validate against allowed operations
  Use sandboxed execution environments
```

## Access Control

### 1. Principle of Least Privilege

✅ **Good**:
```yaml
permissions:
  contents: read
  pull-requests: write
  issues: write
```

❌ **Bad**:
```yaml
permissions:
  contents: write  # Too broad
  administration: write  # Unnecessary
```

### 2. Authentication

```yaml
instructions: |
  Authentication requirements:
  
  1. Always authenticate users/systems
  2. Use strong authentication mechanisms
  3. Validate tokens/credentials properly
  4. Don't trust client-side validation
  5. Implement session timeouts
```

### 3. Authorization

```yaml
instructions: |
  Before performing actions:
  
  1. Verify user has necessary permissions
  2. Check resource ownership
  3. Validate scope of access
  4. Log authorization decisions
  5. Fail closed on errors
```

## Data Protection

### 1. Sensitive Data Handling

```yaml
instructions: |
  When handling sensitive data:
  
  1. Identify what data is sensitive
  2. Minimize data collection
  3. Encrypt data at rest
  4. Encrypt data in transit
  5. Sanitize data in logs
  6. Secure data deletion when done
```

### 2. PII (Personally Identifiable Information)

```yaml
instructions: |
  For PII handling:
  
  1. Check if PII processing is necessary
  2. Obtain appropriate consent
  3. Comply with GDPR/privacy laws
  4. Implement data minimization
  5. Provide data deletion mechanisms
  6. Maintain audit trail
```

### 3. Data in Logs

❌ **Bad**:
```yaml
instructions: |
  Log: "User john.doe@company.com logged in with password: secret123"
```

✅ **Good**:
```yaml
instructions: |
  Log: "User login successful [user_id: 12345]"
  Never log:
  - Passwords
  - API keys
  - Tokens
  - Credit card numbers
  - SSN or other PII
```

## Dependency Security

### 1. Dependency Management

```yaml
dependencies:
  # Specify exact or minimum versions
  - name: trusted-package
    version: ">=2.1.0,<3.0.0"
    source: npmjs  # Use official sources
    
  # Document why each dependency is needed
  purpose: "JSON schema validation"
```

### 2. Dependency Scanning

```yaml
instructions: |
  Before using dependencies:
  
  1. Scan for known vulnerabilities
  2. Check dependency licenses
  3. Review dependency permissions
  4. Use dependency lock files
  5. Regularly update dependencies
```

### 3. Supply Chain Security

- Only use packages from trusted sources
- Verify package signatures when available
- Review dependency tree for suspicious packages
- Monitor for dependency confusion attacks
- Use private registries for internal packages

## Network Security

### 1. HTTPS/TLS

```yaml
instructions: |
  Network communication requirements:
  
  1. Always use HTTPS, never HTTP
  2. Validate TLS certificates
  3. Use modern TLS versions (1.2+)
  4. Don't disable certificate validation
  5. Use certificate pinning for critical APIs
```

### 2. API Security

```yaml
instructions: |
  When calling external APIs:
  
  1. Validate API responses
  2. Implement rate limiting
  3. Set reasonable timeouts
  4. Handle API errors securely
  5. Don't expose internal errors externally
```

### 3. Webhook Security

```yaml
instructions: |
  For webhook handling:
  
  1. Validate webhook signatures
  2. Verify webhook source
  3. Use HTTPS endpoints only
  4. Implement replay protection
  5. Rate limit webhook endpoints
```

## Error Handling

### 1. Secure Error Messages

❌ **Bad**:
```yaml
instructions: |
  On error, show:
  "Database query failed: SELECT * FROM users WHERE password='${input}'"
```

✅ **Good**:
```yaml
instructions: |
  On error, show:
  "An error occurred processing your request. Reference ID: ${error_id}"
  
  Log detailed error securely for debugging.
```

### 2. Information Disclosure

Avoid exposing:
- Stack traces to users
- Internal file paths
- Database schema details
- Configuration details
- Version information
- Internal IP addresses

### 3. Error Recovery

```yaml
instructions: |
  On security-related errors:
  
  1. Log the incident
  2. Alert security team if critical
  3. Fail securely (deny access)
  4. Don't retry automatically
  5. Provide safe recovery path
```

## Audit and Logging

### 1. Security Event Logging

```yaml
instructions: |
  Log these security events:
  
  1. Authentication attempts (success/failure)
  2. Authorization failures
  3. Input validation failures
  4. Security exceptions
  5. Configuration changes
  6. Privileged operations
```

### 2. Audit Trail

```yaml
instructions: |
  Maintain audit trail with:
  
  1. Timestamp (UTC)
  2. User/system identifier
  3. Action performed
  4. Resource affected
  5. Result (success/failure)
  6. Source IP (if applicable)
  
  Ensure audit logs are:
  - Tamper-proof
  - Backed up regularly
  - Retained per policy
```

### 3. Log Protection

```yaml
instructions: |
  Protect logs by:
  
  1. Restricting access (read-only for most)
  2. Encrypting sensitive logs
  3. Sanitizing before storage
  4. Regular backup
  5. Monitoring for tampering
```

## Compliance Requirements

### 1. Regulatory Compliance

Ensure agents comply with:

- **GDPR**: EU data protection (if processing EU data)
- **SOC 2**: Security controls
- **HIPAA**: Healthcare data (if applicable)
- **PCI DSS**: Payment card data (if applicable)
- **SOX**: Financial controls (if applicable)

### 2. Data Residency

```yaml
instructions: |
  For data residency requirements:
  
  1. Identify where data is stored
  2. Comply with regional laws
  3. Document data flows
  4. Implement geo-fencing if needed
  5. Provide data location transparency
```

### 3. Right to Delete

```yaml
instructions: |
  Support data deletion:
  
  1. Identify all data storage locations
  2. Implement secure deletion
  3. Verify deletion completed
  4. Maintain deletion audit log
  5. Handle cascading deletions
```

## Security Testing

### 1. Pre-Deployment Testing

Test for:
- [ ] Input validation bypasses
- [ ] Authentication/authorization flaws
- [ ] Secret exposure
- [ ] Injection vulnerabilities
- [ ] Insecure dependencies
- [ ] Information disclosure
- [ ] Insufficient logging

### 2. Automated Security Scanning

Use these tools:
- **SAST**: Static Application Security Testing
- **DAST**: Dynamic Application Security Testing
- **Dependency Scanning**: Known vulnerability detection
- **Secret Scanning**: Detect committed secrets
- **License Scanning**: Verify license compliance

### 3. Penetration Testing

For critical agents:
- Conduct regular penetration tests
- Test by qualified security professionals
- Address findings promptly
- Retest after fixes

## Incident Response

### 1. Security Incident Plan

```yaml
instructions: |
  If security incident detected:
  
  1. Contain the incident immediately
  2. Notify security team
  3. Preserve evidence
  4. Document timeline
  5. Begin investigation
  6. Implement remediation
  7. Post-incident review
```

### 2. Vulnerability Disclosure

```yaml
# Found a security issue?
security_contact: security@company.com
disclosure_policy: responsible-disclosure
response_time: 24-48 hours
```

### 3. Security Updates

```yaml
instructions: |
  For security updates:
  
  1. Assess severity (Critical/High/Medium/Low)
  2. Develop fix
  3. Test thoroughly
  4. Deploy expedited review for critical
  5. Notify users
  6. Update documentation
```

## Secure Development Lifecycle

1. **Design Phase**: Threat modeling, security requirements
2. **Development Phase**: Secure coding, code review
3. **Testing Phase**: Security testing, vulnerability scanning
4. **Deployment Phase**: Security configuration, access controls
5. **Operations Phase**: Monitoring, incident response
6. **Maintenance Phase**: Patching, updates, decommissioning

## Security Checklist

Before submitting an agent:

- [ ] No hardcoded secrets or credentials
- [ ] All inputs validated and sanitized
- [ ] Proper error handling (no info disclosure)
- [ ] Least privilege permissions
- [ ] Dependencies scanned for vulnerabilities
- [ ] Sensitive data encrypted
- [ ] Audit logging implemented
- [ ] Security testing completed
- [ ] Documentation includes security notes
- [ ] Compliance requirements met

## Resources

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [CWE Top 25](https://cwe.mitre.org/top25/)
- [GitHub Security Best Practices](https://docs.github.com/en/code-security)
- Internal: Security Team Wiki
- Internal: Secure Coding Standards

## Contact

**Security Team**: security@company.com  
**Security Incidents**: security-incidents@company.com  
**Urgent Security Issues**: +1-XXX-XXX-XXXX

---

**Version**: 1.0.0  
**Last Updated**: 2025-10-26  
**Next Review**: 2026-01-26
