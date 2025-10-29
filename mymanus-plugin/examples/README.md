# MyManus Plugin Examples

This directory contains detailed examples demonstrating the capabilities of the MyManus plugin for Claude Code.

## Overview

Each example shows:
- **Task Description**: What the user asks for
- **Expected Behavior**: How Claude responds with the plugin
- **Key Features**: Which plugin capabilities are demonstrated
- **Comparison**: Behavior with vs without the plugin

## Available Examples

### [01 - Research Task](./01-research-example.md)

**Task:** Comprehensive research on quantum computing with citations

**Demonstrates:**
- Autonomous planning and task breakdown
- Multi-source web research
- Source verification and citation
- Long-form report writing (3000+ words)
- Progress tracking with TodoWrite
- Systematic information gathering

**Best for learning:** How the plugin handles research and documentation tasks

**Complexity:** ⭐⭐⭐ (Moderate)

---

### [02 - Coding Project](./02-coding-example.md)

**Task:** Build a complete Python CLI tool for directory monitoring and backup

**Demonstrates:**
- Project architecture planning
- Professional code organization
- Comprehensive error handling
- Automatic test creation
- Documentation generation
- Code validation and testing
- Best practices application

**Best for learning:** How the plugin approaches software development tasks

**Complexity:** ⭐⭐⭐⭐ (Advanced)

---

### [03 - Web Automation & Data Extraction](./03-web-automation-example.md)

**Task:** Extract structured data from Hacker News using browser automation

**Demonstrates:**
- Playwright MCP integration
- Browser automation for complex interactions
- Intelligent data extraction
- Data validation and quality checks
- Multiple output formats (CSV + Markdown)
- Automatic analysis and statistics
- Error handling for web scraping

**Best for learning:** How the plugin uses browser automation and handles web data

**Complexity:** ⭐⭐⭐⭐ (Advanced)

---

## Quick Start

### Trying the Examples

1. **Install the Plugin**: Follow the [main installation guide](../README.md#installation)

2. **Start Claude Code**: Ensure the plugin is loaded

3. **Copy Example Prompts**: Use the task descriptions from any example

4. **Observe Behavior**: Watch how Claude automatically:
   - Creates task plans
   - Executes steps systematically
   - Handles errors
   - Validates results
   - Provides comprehensive output

### Example Session

```bash
# Start Claude Code
claude-code

# Try the research example
> Research the current state of quantum computing in 2024-2025.
> Focus on: major breakthroughs, leading companies, practical
> applications, and remaining challenges. Create a comprehensive
> report with citations.

# Watch as Claude:
# 1. Creates a TodoWrite plan automatically
# 2. Performs systematic web searches
# 3. Verifies sources
# 4. Compiles a comprehensive report
# 5. Tracks progress throughout
```

## Example Selection Guide

### Choose Example 01 (Research) if you want to:
- ✅ Understand autonomous planning
- ✅ See systematic information gathering
- ✅ Learn about multi-source research
- ✅ See long-form writing capabilities
- ✅ Understand citation and reference management

### Choose Example 02 (Coding) if you want to:
- ✅ See professional software development
- ✅ Understand project organization
- ✅ Learn about automatic testing
- ✅ See error handling best practices
- ✅ Understand documentation generation

### Choose Example 03 (Web Automation) if you want to:
- ✅ See browser automation in action
- ✅ Understand complex web interactions
- ✅ Learn about data extraction workflows
- ✅ See data validation techniques
- ✅ Understand when to use WebFetch vs Playwright

## Common Patterns Across Examples

### 1. Autonomous Planning

All examples demonstrate automatic task planning:

```
User provides goal
↓
Claude creates detailed TodoWrite plan
↓
Claude executes each step
↓
Claude marks progress
↓
Claude delivers complete result
```

**No user intervention needed during execution.**

### 2. Progress Transparency

All examples show real-time progress updates:

```
[In Progress] Searching for quantum computing breakthroughs
[Completed] Searching for quantum computing breakthroughs
[In Progress] Accessing source URLs
...
```

**User always knows what's happening.**

### 3. Quality Validation

All examples include automatic validation:

- Research: Source verification, fact-checking
- Coding: Tests, code execution, error checking
- Web Automation: Data validation, completeness checks

**Claude validates before marking tasks complete.**

### 4. Professional Output

All examples produce production-quality results:

- Well-structured and organized
- Comprehensive and detailed
- Properly formatted
- Includes metadata and documentation
- Multiple output formats where beneficial

**Output is ready for real-world use.**

## Customization Ideas

Each example can be customized to explore different capabilities:

### Research Example Variations

```
# Focus on specific aspect
"Research quantum computing applications in drug discovery"

# Different length
"Research quantum computing (1500 words, executive summary style)"

# Different format
"Research quantum computing and create a slide deck outline"

# Multiple topics
"Compare quantum computing, neuromorphic computing, and
photonic computing - which is most promising for AI?"
```

### Coding Example Variations

```
# Different language
"Build the same tool in Rust"

# Additional features
"Add email notifications when files are backed up"

# Different architecture
"Implement using async/await patterns"

# Add deployment
"Containerize the application with Docker"
```

### Web Automation Variations

```
# Different site
"Extract product information from Amazon search results"

# Multi-step workflow
"Search GitHub, find popular ML repos, visit each, and
extract contributor information"

# Analysis focus
"Extract and analyze sentiment from Reddit comments on a topic"

# Comparison task
"Compare pricing across 5 e-commerce sites for a product"
```

## Advanced Usage

### Combining Capabilities

The plugin's power multiplies when combining capabilities:

**Research + Coding:**
```
"Research the best practices for implementing OAuth2, then
build a Python library following those practices with
comprehensive tests and documentation."
```

**Web Automation + Analysis:**
```
"Extract the last 100 posts from a tech blog, analyze the
topics covered, and create a visualization of topic trends
over time."
```

**Coding + Web Automation + Research:**
```
"Research the most popular web scraping techniques, then build
a tool that demonstrates these techniques by extracting and
comparing data from three different news sites."
```

## Troubleshooting Examples

### If Examples Don't Work as Expected

1. **Verify Plugin Installation**: Ensure system prompt is loaded
   ```bash
   # Check if .claude/project-instructions.md exists
   ls -la .claude/
   ```

2. **Check MCP Configuration**: Verify Playwright is configured
   ```bash
   # Test Playwright availability
   # Try a simple browser task
   ```

3. **Review TodoWrite**: If tasks aren't being tracked, check TodoWrite availability

4. **Check Logs**: Look for error messages in Claude Code output

### Common Issues

**Issue:** Claude asks too many questions instead of proceeding autonomously

**Solution:** Ensure the system prompt is loaded correctly. The prompt should make Claude more autonomous.

---

**Issue:** Browser automation doesn't work

**Solution:** Verify Playwright MCP is installed and configured. Check the [troubleshooting guide](../README.md#troubleshooting) in the main README.

---

**Issue:** Tasks aren't broken down into steps

**Solution:** For simple tasks, this is expected. TodoWrite is only used for complex multi-step tasks. Try a more complex example.

---

## Contributing Examples

Have an interesting use case? Contribute additional examples!

1. Follow the format of existing examples
2. Include:
   - Clear task description
   - Expected behavior walkthrough
   - Key features demonstrated
   - Comparison with/without plugin
3. Test thoroughly before submitting
4. Submit PR to the main repository

## Next Steps

- **Try the Examples**: Start with Example 01 and work your way through
- **Customize**: Modify examples for your specific needs
- **Explore**: Create your own use cases based on patterns learned
- **Share**: Contribute successful examples back to the community

## Additional Resources

- [Main Plugin README](../README.md): Full installation and configuration guide
- [System Prompt](../system-prompt.md): The prompt that powers the behavior
- [MCP Configuration](../mcp-config.json): Browser automation setup
- [MyManus Documentation](../../README.md): Original MyManus project

---

**Ready to see autonomous AI in action?** Pick an example and try it out!
