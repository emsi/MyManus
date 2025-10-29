# MCP Configuration for MyManus Plugin

The MyManus plugin automatically configures the Playwright MCP server for browser automation. This guide covers platform-specific setup and troubleshooting.

## Automatic Setup

The plugin includes a `.mcp.json` file that automatically configures Playwright when installed. **No manual configuration needed for macOS/Windows.**

### Linux/WSL2 Additional Setup

Linux and WSL2 require an X server for browser display. Add the DISPLAY environment variable to your MCP configuration:

1. Open Claude Code settings → MCP Servers
2. Locate the `playwright` server (auto-configured by the plugin)
3. Add environment variable:
   ```json
   "env": {
     "DISPLAY": ":0"
   }
   ```

**X Server Setup:**
- **Linux**: X server usually pre-installed with desktop environment
- **WSL2**: Install VcXsrv or X410 on Windows host
- Test X server: `xclock` (should show a clock window)

## Prerequisites

- **Node.js** v18 or later (includes npx)
- Verify: `node --version` and `npx --version`

## Troubleshooting

### Browser Doesn't Launch

- Verify Node.js: `node --version`
- Restart Claude Code after plugin installation
- Check Claude Code logs for MCP errors

### DISPLAY Error (Linux/WSL2 only)

- Install X server (VcXsrv for WSL2)
- Add `"DISPLAY": ":0"` to MCP config env section
- Start X server before Claude Code
- Test: `xclock` should display a clock

### Playwright Tools Not Available

- Restart Claude Code (MCP servers load on startup)
- Check plugin is installed: `/plugin list`
- Test Playwright manually: `npx -y @automatalabs/mcp-server-playwright`

## Resources

- [Playwright MCP Server](https://www.npmjs.com/package/@automatalabs/mcp-server-playwright)
- [Claude Code MCP Documentation](https://docs.claude.com/en/docs/claude-code/mcp)
