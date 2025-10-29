# MCP Server Setup for MyManus Plugin

The MyManus plugin works best with browser automation capabilities provided by the Playwright MCP server. This guide explains how to configure it.

## Why Playwright MCP?

The Playwright MCP server enables:
- **Full browser automation** for web research and complex interactions
- **JavaScript rendering** for dynamic websites
- **Form interactions** and multi-step navigation
- **Cookie/popup handling** for real-world websites
- **Screenshot and content extraction** capabilities

> **Note**: Claude Code already has WebFetch for simple page retrieval. Playwright adds advanced browser automation on top of that.

## Installation

### Prerequisites

- **Node.js** (v18 or later)
- **npx** (comes with Node.js)

Verify installation:
```bash
node --version
npx --version
```

### Configure in Claude Code

Add the Playwright MCP server to your Claude Code MCP configuration:

#### For macOS/Windows:

```json
{
  "mcpServers": {
    "playwright": {
      "runtime": "node",
      "command": "npx",
      "args": [
        "-y",
        "@automatalabs/mcp-server-playwright"
      ]
    }
  }
}
```

#### For Linux/WSL2:

Linux and WSL2 require an X server for browser display:

```json
{
  "mcpServers": {
    "playwright": {
      "runtime": "node",
      "command": "npx",
      "args": [
        "-y",
        "@automatalabs/mcp-server-playwright"
      ],
      "env": {
        "DISPLAY": ":0"
      }
    }
  }
}
```

**Linux/WSL2 Setup:**
1. Ensure X server is running
   - **Linux**: Usually pre-installed with desktop environment
   - **WSL2**: Install VcXsrv or X410 on Windows
2. Set DISPLAY environment variable (usually `:0`)
3. Test X server: `xclock` (should show a clock window)

## Configuration Location

Add the MCP server configuration through:

1. **Claude Code Settings/Preferences**
   - Open settings
   - Navigate to "MCP Servers" section
   - Add the configuration above
   - Save and restart Claude Code

2. **Manual Configuration File** (alternative)
   - Locate your Claude Code config directory:
     - Linux: `~/.config/claude-code/`
     - macOS: `~/Library/Application Support/claude-code/`
     - Windows: `%APPDATA%/claude-code/`
   - Edit the MCP configuration file
   - Add server to the `mcpServers` section

## Verification

After configuration and restart:

1. **Check Available Tools**
   - Playwright tools should appear in Claude Code's tool list
   - Look for browser-related operations

2. **Test Browser Automation**
   ```
   Use the browser to navigate to example.com and describe what you see.
   ```

   Claude should:
   - Launch a browser window
   - Navigate to the site
   - Extract and describe the content

## Troubleshooting

### Browser Doesn't Launch

**Symptom**: No browser window appears

**Solutions**:
- Verify Node.js is installed: `node --version`
- Check npx works: `npx --version`
- Restart Claude Code after configuration
- Check Claude Code logs for MCP server errors

### DISPLAY Error (Linux/WSL2)

**Symptom**: `Error: DISPLAY environment variable not set`

**Solutions**:
- Install X server (VcXsrv for WSL2)
- Set DISPLAY in MCP config: `"DISPLAY": ":0"`
- Test X server: `xclock`
- Start X server before Claude Code

### Playwright Tools Not Available

**Symptom**: Claude can't use browser automation

**Solutions**:
- Verify MCP configuration is saved correctly
- Restart Claude Code (MCP servers load on startup)
- Check MCP server logs in Claude Code
- Try manual Playwright test: `npx -y @automatalabs/mcp-server-playwright`

### Performance Issues

**Symptom**: Browser automation is slow

**Solutions**:
- Close other browser instances
- Increase system resources if running in VM
- Use headless mode if visual display isn't needed
- Check internet connection speed

## Optional: Other MCP Servers

The MyManus plugin primarily uses Claude Code's built-in tools, but you can add other MCP servers for additional capabilities:

### Filesystem MCP (Optional)

Only needed if you want sandboxed filesystem access:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/path/to/sandbox/directory"
      ]
    }
  }
}
```

> **Note**: Claude Code already has Read/Write/Edit tools, so this is usually unnecessary.

### Shell MCP (Optional)

Only needed for sandboxed shell access:

```json
{
  "mcpServers": {
    "shell": {
      "command": "uvx",
      "args": [
        "mcp-server-shell @ git+https://github.com/emsi/mcp-server-shell"
      ]
    }
  }
}
```

> **Note**: Claude Code already has a Bash tool, so this is usually unnecessary unless you need specific sandboxing.

## Best Practices

1. **Start Simple**: Begin with just Playwright MCP
2. **Test Incrementally**: Verify each MCP server works before adding more
3. **Monitor Performance**: MCP servers use system resources
4. **Update Regularly**: Keep MCP servers updated for bug fixes and features
5. **Check Logs**: Review Claude Code logs when troubleshooting

## Resources

- **Playwright MCP**: [@automatalabs/mcp-server-playwright](https://www.npmjs.com/package/@automatalabs/mcp-server-playwright)
- **MCP Protocol**: [Model Context Protocol Documentation](https://modelcontextprotocol.io/)
- **Claude Code MCP Guide**: [Official MCP Documentation](https://docs.claude.com/en/docs/claude-code/mcp)

---

**Once Playwright MCP is configured, the MyManus plugin can leverage full browser automation for research, data extraction, and complex web interactions!**
