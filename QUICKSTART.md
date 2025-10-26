# Quick Start Guide

Welcome to the Enterprise Custom Agents repository! This guide will help you get started quickly.

## 🚀 For Users (Using Existing Agents)

### Step 1: Find an Agent

Browse the [catalog](catalog/README.md) or explore by category:
- [Development Agents](agents/development/)
- [Security Agents](agents/security/)
- [Documentation Agents](agents/documentation/)
- [Testing Agents](agents/testing/)
- [Operations Agents](agents/operations/)

### Step 2: Copy to Your Project

```bash
# Example: Copy the code-reviewer agent
cp -r agents/development/code-reviewer .github/agents/
```

### Step 3: Use the Agent

Invoke the agent in your GitHub Copilot session or workflow:

```
Use the code-reviewer agent to review my pull request
```

That's it! 🎉

---

## 👨‍💻 For Contributors (Creating New Agents)

### Step 1: Use the Template

```bash
# Copy the template
cp -r templates/agent-template agents/[category]/[your-agent-name]
```

### Step 2: Define Your Agent

Edit the `agent.yml` file:
- Set name, version, category
- Write clear instructions
- Document examples
- List limitations

### Step 3: Document It

Update the `README.md`:
- Describe what the agent does
- List use cases
- Provide examples
- Document configuration

### Step 4: Test It

Test your agent thoroughly:
- Try all examples
- Test edge cases
- Verify error handling
- Check security

### Step 5: Submit for Review

```bash
# Create a branch
git checkout -b agent/your-agent-name

# Add your files
git add agents/[category]/[your-agent-name]

# Commit
git commit -m "Add [your-agent-name] agent"

# Push and create PR
git push origin agent/your-agent-name
```

### Step 6: Address Feedback

Respond to review comments and make necessary updates.

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

---

## 📋 For Reviewers

### Review Checklist

When reviewing an agent submission:

1. **Functionality**
   - [ ] Agent purpose is clear
   - [ ] Instructions are comprehensive
   - [ ] Examples work as documented

2. **Documentation**
   - [ ] README is complete
   - [ ] Use cases are listed
   - [ ] Limitations are documented

3. **Security**
   - [ ] No hardcoded secrets
   - [ ] Input validation present
   - [ ] Error handling is secure

4. **Quality**
   - [ ] Code is well-structured
   - [ ] Follows best practices
   - [ ] Version number is correct

See [docs/approval-process.md](docs/approval-process.md) for the full review process.

---

## 🛠️ Common Tasks

### Update an Existing Agent

```bash
# Make your changes
edit agents/category/agent-name/agent.yml

# Update version number
# Update CHANGELOG.md

# Submit PR
git checkout -b update/agent-name
git add agents/category/agent-name
git commit -m "Update agent-name to v1.1.0"
git push origin update/agent-name
```

### Deprecate an Agent

1. Update the agent's README with deprecation notice
2. Mark as deprecated in catalog
3. Provide migration path to alternative
4. Set end-of-life date

### Report a Bug

Use the [bug report template](.github/ISSUE_TEMPLATE/bug_report.md):
1. Go to Issues → New Issue
2. Select "Bug Report"
3. Fill out the template
4. Submit

### Request a Feature

Use the [feature request template](.github/ISSUE_TEMPLATE/feature_request.md):
1. Go to Issues → New Issue
2. Select "Feature Request"
3. Describe your idea
4. Submit

---

## 📚 Key Resources

- **[README](README.md)**: Repository overview
- **[Contributing Guidelines](CONTRIBUTING.md)**: How to contribute
- **[Governance Policy](docs/governance-policy.md)**: Governance rules
- **[Approval Process](docs/approval-process.md)**: Review workflow
- **[Best Practices](docs/best-practices.md)**: Best practices guide
- **[Security Guidelines](docs/security-guidelines.md)**: Security requirements
- **[Catalog](catalog/README.md)**: All available agents

---

## 💡 Tips

- **Start Small**: Begin with the template and examples
- **Read the Docs**: Review governance and best practices
- **Test Thoroughly**: Test before submitting
- **Ask Questions**: Use GitHub Discussions or Slack
- **Iterate**: Expect feedback and be ready to improve

---

## 🆘 Getting Help

- **Slack**: #custom-agents channel
- **Email**: agents-support@company.com
- **Discussions**: GitHub Discussions in this repo
- **Office Hours**: Thursdays 2-3pm (check calendar)

---

## 🎯 Next Steps

**Using Agents?**
→ Browse the [catalog](catalog/README.md) and try an agent

**Creating Agents?**
→ Read [CONTRIBUTING.md](CONTRIBUTING.md) and use the [template](templates/agent-template/)

**Reviewing Agents?**
→ Review the [approval process](docs/approval-process.md)

---

Happy agent building! 🚀
