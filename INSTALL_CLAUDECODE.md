# MyManus Plugin Installation for Claude Code

This guide covers installing and using the MyManus plugin with [Claude Code](https://claude.com/claude-code), Anthropic's official CLI for Claude.

## Overview

The MyManus plugin brings autonomous agent capabilities to Claude Code through a **skill-based approach**. Unlike the original MyManus (for Claude Desktop), this plugin:

- ✅ **Uses Claude Code's built-in tools** (Bash, Read, Write, Edit, Glob, Grep)
- ✅ **Relies on Claude Code's MCP integration** (no separate file/shell servers)
- ✅ **Automatically configures Playwright MCP** for browser automation
- ⚠️ **No dedicated sandboxing** beyond Claude Code's standard capabilities

## Prerequisites

Before installing the MyManus plugin, ensure you have:

1. **Claude Code**: Install from [claude.com/claude-code](https://claude.com/claude-code)
2. **Node.js** v18 or later: Required for Playwright MCP server
   ```bash
   node --version  # Should show v18.0.0 or higher
   ```
3. **Git**: For accessing the plugin repository

## Installation

### Step 1: Add the MyManus Marketplace

Open Claude Code and run:

```bash
/plugin marketplace add https://github.com/emsi/MyManus.git
```

This adds the MyManus plugin marketplace to your Claude Code configuration.

### Step 2: Install the Plugin

```bash
/plugin install mymanus@mymanus
```

The plugin will be installed and the `.mcp.json` file will automatically configure the Playwright MCP server.

### Step 3: Restart Claude Code

**Important**: You must restart Claude Code for the plugin and MCP server to load.

```bash
# Exit Claude Code
exit

# Start Claude Code again
claude
```

### Step 4: Verify Installation

Check that the plugin loaded successfully:

```bash
/plugin list
```

You should see `mymanus` in the installed plugins list.

## Linux/WSL2 Additional Setup

If you're on Linux or WSL2, you need to configure the DISPLAY environment variable for browser automation:

### 1. Install X Server

- **Linux**: X server is usually pre-installed with desktop environments
- **WSL2**: Install [VcXsrv](https://sourceforge.net/projects/vcxsrv/) or X410 on Windows

### 2. Configure DISPLAY Variable

1. Open Claude Code settings/preferences
2. Navigate to **MCP Servers** section
3. Locate the `playwright` server (auto-configured by the plugin)
4. Add the environment variable:

```json
{
  "playwright": {
    "runtime": "node",
    "command": "npx",
    "args": ["-y", "@automatalabs/mcp-server-playwright"],
    "env": {
      "DISPLAY": ":0"
    }
  }
}
```

5. Restart Claude Code

### 3. Test X Server

Verify X server is running:

```bash
xclock  # Should display a clock window
```

## Using the MyManus Skill

The MyManus plugin provides autonomous agent capabilities through a **skill**. Skills in Claude Code are designed to be **automatically invoked** based on task context, but you can also **explicitly request** them.

### Automatic Invocation (Recommended)

When you give Claude a complex, multi-step task, it should automatically recognize and apply the MyManus skill. For example:

```
Research the latest developments in quantum computing in 2024,
write a comprehensive report with citations, and create a summary
presentation.
```

Claude will automatically use the MyManus skill's structured agent loop (Plan → Execute → Observe → Iterate) to handle this complex task.

### Explicit Invocation

If Claude doesn't automatically apply the MyManus skill for a complex task, you can explicitly request it:

**Option 1: Direct Request**
```
Use the MyManus skill to research [topic] and create a detailed report.
```

**Option 2: Mention Agent Behavior**
```
Act as an autonomous agent and research the top 10 AI startups in 2024,
analyze their products, and create a comparison table.
```

**Option 3: Reference the Agent Loop**
```
Using the agent loop methodology, plan and execute a web scraping task
to collect product data from example.com.
```

### When to Use MyManus Skill

The MyManus skill is ideal for:

- 🔍 **Complex research tasks** requiring multiple sources and fact-checking
- 📝 **Long-form writing** with citations and references
- 🌐 **Web automation** using browser interactions
- 💻 **Software development projects** with planning and testing
- 📊 **Data analysis** with multiple processing steps
- 🤖 **Multi-step workflows** requiring autonomous execution

### Skill Behavior

When active, the MyManus skill:

1. **Plans the task** using TodoWrite for transparent progress tracking
2. **Executes systematically** following the agent loop methodology
3. **Validates results** before marking tasks complete
4. **Adapts strategies** when encountering obstacles
5. **Reports progress** clearly at each step

## Important Differences from Claude Desktop MyManus

### File & Shell Access

- **Claude Desktop MyManus**: Uses dedicated MCP servers for sandboxed file/shell access
- **Claude Code MyManus**: Uses Claude Code's **native tools** (Bash, Read, Write, Edit)
- **Implication**: No additional sandboxing beyond Claude Code's standard security model

### Browser Automation

- **Both versions**: Use Playwright MCP server for browser automation
- **Claude Code version**: Auto-configured via `.mcp.json` (no manual setup for macOS/Windows)

### Security Model

⚠️ **Important**: The Claude Code plugin does **NOT** provide dedicated sandboxing. It operates with the same permissions and access level as Claude Code itself.

- File operations use Claude Code's native tools
- Shell commands run through Claude Code's Bash tool
- Browser automation runs on your local machine (not in a dedicated sandbox)

**Best Practices**:
- Review complex commands before execution
- Use in trusted development environments
- Be cautious with file operations outside project directories
- Monitor browser automation tasks

## Troubleshooting

### Plugin Not Loading

**Symptom**: Plugin doesn't appear in `/plugin list`

**Solutions**:
1. Verify installation: `/plugin list` and look for `mymanus`
2. Check marketplace: `/plugin marketplace list`
3. Restart Claude Code
4. Reinstall: `/plugin uninstall mymanus` then `/plugin install mymanus@mymanus`

### Browser Automation Not Working

**Symptom**: Browser-related tasks fail or no browser window appears

**Solutions**:
1. Verify Node.js: `node --version` (need v18+)
2. Check MCP configuration in Claude Code settings
3. Restart Claude Code after MCP configuration changes
4. Linux/WSL2: Ensure DISPLAY variable is set and X server is running

### Skill Not Being Used

**Symptom**: Claude doesn't use MyManus agent behavior for complex tasks

**Solutions**:
1. Explicitly request the skill: "Use the MyManus skill to..."
2. Mention autonomous behavior: "Act as an autonomous agent and..."
3. Reference specific capabilities: "Using browser automation, navigate to..."
4. Check plugin is enabled: `/plugin list` (should show `mymanus` as enabled)

### MCP Server Errors

**Symptom**: Playwright MCP server fails to start

**Solutions**:
1. Update npx cache: `npx clear-npx-cache`
2. Test manually: `npx -y @automatalabs/mcp-server-playwright`
3. Check Claude Code logs for detailed error messages
4. Verify Node.js installation: `which node` and `which npx`

## Advanced Configuration

### Using with Custom MCP Servers

The MyManus plugin works alongside any MCP servers you've configured in Claude Code. The plugin's `.mcp.json` only adds Playwright - your existing MCP configuration remains unchanged.

### Disabling Auto-MCP Configuration

If you want to manage Playwright MCP manually:

1. Configure Playwright in Claude Code settings before installing the plugin
2. The plugin's `.mcp.json` won't override existing configuration

### Development and Testing

To test plugin changes during development:

```bash
# Clone the repository
git clone https://github.com/emsi/MyManus.git
cd MyManus

# Add as local marketplace
/plugin marketplace add /path/to/MyManus

# Install from local source
/plugin install mymanus@local

# Make changes to mymanus-plugin/skills/mymanus/SKILL.md
# Then restart Claude Code to reload
```

## Getting Help

- **Plugin Issues**: [GitHub Issues](https://github.com/emsi/MyManus/issues)
- **Claude Code Documentation**: [code.claude.com/docs](https://code.claude.com/docs)
- **MCP Protocol**: [modelcontextprotocol.io](https://modelcontextprotocol.io/)

## Next Steps

- Read the [plugin README](./mymanus-plugin/README.md) for feature details
- Check out [usage examples](./mymanus-plugin/examples/README.md)
- Review the [MCP setup guide](./mymanus-plugin/MCP_SETUP.md) for troubleshooting

---

**Ready to go!** Start Claude Code and give it a complex task to see the MyManus autonomous agent in action.
