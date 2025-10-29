# MyManus Plugin for Claude Code

Transform Claude Code into an autonomous agent with MyManus capabilities - planning, reasoning, execution, and evaluation - all without writing a single line of code.

## Overview

This plugin brings the powerful agentic behavior of [MyManus](https://github.com/emsi/MyManus) to [Claude Code](https://claude.com/claude-code), Anthropic's official CLI for Claude. It combines MyManus's structured agent loop methodology with Claude Code's robust development tools to create a truly autonomous AI assistant.

### What You Get

**From MyManus:**
- 🎯 Structured agent loop (Plan → Execute → Observe → Iterate)
- 🧠 Multi-module architecture (Planner, Knowledge, Datasource)
- 🌐 Browser automation capabilities via Playwright
- 📋 Systematic task planning and progress tracking
- 📝 Professional writing and research workflows
- 🔄 Autonomous error handling and strategy adaptation

**From Claude Code:**
- ⚡ Fast, native development tools (Bash, Read, Write, Edit, Glob, Grep)
- 🔍 Powerful web research (WebFetch, WebSearch)
- 🤖 Specialized agents for complex tasks
- 📊 Built-in task management (TodoWrite)
- 🔌 MCP server integration
- 🖥️ Cross-platform support (Linux, macOS, Windows)

**Together:**
A powerful, autonomous AI agent that can research, code, analyze data, write reports, and automate complex workflows - all while keeping you informed of progress every step of the way.

## Key Features

### 1. Autonomous Agent Behavior
Unlike standard Claude interactions, the MyManus plugin enables true autonomous operation:
- Plans complex tasks independently
- Executes multi-step workflows without constant supervision
- Adapts strategies when encountering obstacles
- Validates results before marking tasks complete

### 2. Browser Automation
Full web automation via Playwright MCP server:
- Navigate complex websites
- Fill forms and interact with dynamic content
- Handle JavaScript-heavy applications
- Perform multi-step web research workflows
- Close cookie banners and popups automatically

### 3. Structured Task Management
Built-in planning and tracking:
- Automatic task breakdown for complex requests
- Real-time progress updates via TodoWrite
- Clear status reporting (pending → in_progress → completed)
- Transparent strategy changes and error handling

### 4. Professional Research & Writing
Specialized workflows for information work:
- Multi-source fact-checking and validation
- In-depth research with comprehensive documentation
- Long-form article and report writing
- Proper citation and reference management

### 5. Systematic Coding
Organized development practices:
- Project structure planning
- Code organization and documentation
- Testing and validation
- Error handling and debugging

## Installation

### Prerequisites

1. **Claude Code**: Install from [claude.com/claude-code](https://claude.com/claude-code)
2. **Node.js**: Required for Playwright MCP server (v18 or later recommended)
3. **Git**: For cloning this repository

### Step 1: Clone the Repository

```bash
git clone https://github.com/emsi/MyManus.git
cd MyManus/mymanus-plugin
```

### Step 2: Configure System Prompt

You have two options for adding the system prompt to your Claude Code project:

#### Option A: Project-Level Configuration (Recommended)

1. Create or open your Claude Code project directory
2. Create a `.claude/` directory if it doesn't exist:
   ```bash
   mkdir -p .claude
   ```
3. Copy the system prompt to your project:
   ```bash
   cp /path/to/MyManus/mymanus-plugin/system-prompt.md .claude/project-instructions.md
   ```
4. Claude Code will automatically load this prompt for the project

#### Option B: Global Configuration

1. Locate your Claude Code configuration directory:
   - Linux: `~/.config/claude-code/`
   - macOS: `~/Library/Application Support/claude-code/`
   - Windows: `%APPDATA%/claude-code/`
2. Add the system prompt to your global settings
3. Note: This will apply to all Claude Code sessions

### Step 3: Configure Playwright MCP Server

#### For Linux/macOS:

1. Ensure Node.js and npx are installed:
   ```bash
   node --version
   npx --version
   ```

2. Add Playwright MCP server to Claude Code's MCP configuration:
   - Open Claude Code settings/preferences
   - Navigate to MCP Servers section
   - Add the following configuration:

   ```json
   {
     "playwright": {
       "runtime": "node",
       "command": "npx",
       "args": [
         "-y",
         "@automatalabs/mcp-server-playwright"
       ]
     }
   }
   ```

3. **Linux/WSL2 Only**: If you're using Linux or WSL2, you may need to set the DISPLAY variable:
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
   Ensure you have an X server running (on WSL2, use VcXsrv or X410).

#### For Windows:

Windows users typically don't need the DISPLAY environment variable. Use this configuration:

```json
{
  "playwright": {
    "runtime": "node",
    "command": "npx",
    "args": [
      "-y",
      "@automatalabs/mcp-server-playwright"
    ]
  }
}
```

### Step 4: Restart Claude Code

After adding the system prompt and MCP configuration:
1. Save all settings
2. Restart Claude Code completely
3. Verify Playwright MCP tools are available

### Step 5: Verify Installation

Test the installation with a simple command:

```
Please use the browser to navigate to example.com and tell me what you see.
```

If Playwright is configured correctly, Claude Code should launch a browser, navigate to the page, and describe the content.

## Usage

### Basic Usage

Once installed, simply start using Claude Code normally. The MyManus system prompt will guide Claude's behavior to be more autonomous and structured.

**Example simple task:**
```
Research the latest Python web frameworks and create a comparison table.
```

Claude will:
1. Create a todo list with research steps
2. Use web search to find current information
3. Visit multiple sources for comprehensive data
4. Compile findings into a structured table
5. Mark each step complete as it progresses

**Example complex task:**
```
Build a simple web app that fetches weather data and displays it with charts.
```

Claude will:
1. Plan the architecture (backend, frontend, API integration)
2. Create project structure
3. Write backend code for API calls
4. Create frontend with visualization
5. Test the application
6. Provide deployment instructions
7. Keep you updated throughout via TodoWrite

### Advanced Features

#### 1. Web Research with Browser Automation

For complex web interactions:
```
Use the browser to find the top 5 machine learning courses on Coursera,
including their ratings, duration, and enrollment numbers.
```

Claude will use Playwright to:
- Navigate to Coursera
- Handle cookie banners
- Search for ML courses
- Extract detailed information
- Compile results systematically

#### 2. Long-Form Writing

For research reports and articles:
```
Write a 3000-word article about the history of artificial intelligence,
with proper citations and references.
```

Claude will:
- Research from multiple authoritative sources
- Create an outline
- Write individual sections
- Cite sources properly
- Compile a complete, well-structured article

#### 3. Code Projects

For development tasks:
```
Create a Python script that monitors a directory for new files
and automatically backs them up to a specified location.
```

Claude will:
- Plan the implementation
- Create proper project structure
- Write well-documented code
- Add error handling
- Create tests
- Provide usage documentation

### Tips for Best Results

1. **Be Specific**: Clear task descriptions lead to better planning
2. **Allow Autonomy**: Trust the agent loop to handle details
3. **Check Progress**: Watch TodoWrite updates to track progress
4. **Provide Feedback**: Correct course when needed, but let Claude adapt
5. **Complex Tasks**: Break huge projects into major phases yourself, let Claude handle each phase autonomously

## Differences from Standalone MyManus

| Feature | Standalone MyManus | MyManus Plugin for Claude Code |
|---------|-------------------|-------------------------------|
| **Platform** | Claude Desktop with custom setup | Claude Code CLI |
| **Installation** | Manual sandbox setup required | Simple plugin installation |
| **File Tools** | MCP filesystem server | Native Read/Write/Edit tools |
| **Shell Access** | MCP shell server (sandboxed) | Native Bash tool |
| **Browser** | Playwright MCP | Playwright MCP + WebFetch |
| **Task Tracking** | Manual todo.md files | TodoWrite tool |
| **Sandboxing** | Custom claude_sandbox.sh | Claude Code's built-in security |
| **Web Search** | Browser-based only | Native WebSearch + browser |
| **Best For** | Desktop users, visual workflows | Developers, CLI enthusiasts |

### When to Use Each

**Use Standalone MyManus if:**
- You prefer desktop GUI interaction
- You need visual project selection
- You want the original sandboxed environment
- You're already using Claude Desktop

**Use MyManus Plugin for Claude Code if:**
- You prefer CLI/terminal interfaces
- You're a developer or power user
- You want native integration with dev tools
- You need faster tool execution
- You want to version control your projects with git

## Troubleshooting

### Playwright MCP Not Working

**Symptom**: Browser doesn't launch or Playwright tools are unavailable

**Solutions**:
1. Verify Node.js installation: `node --version`
2. Check MCP configuration is saved correctly
3. Restart Claude Code completely
4. Check logs for MCP server errors
5. On Linux/WSL2: Ensure X server is running and DISPLAY is set
6. Try manual installation: `npx -y @automatalabs/mcp-server-playwright`

### System Prompt Not Loading

**Symptom**: Claude doesn't exhibit autonomous behavior

**Solutions**:
1. Verify system-prompt.md is in the correct location
2. Check Claude Code project settings
3. Ensure the prompt isn't overridden by user settings
4. Restart Claude Code after adding the prompt

### Tasks Not Being Tracked

**Symptom**: No TodoWrite updates appear

**Solutions**:
1. Verify TodoWrite tool is available in Claude Code
2. Check if the task is complex enough (simple tasks don't need tracking)
3. Ensure the system prompt is loaded correctly

### Browser Shows DISPLAY Error (Linux/WSL2)

**Symptom**: "Error: DISPLAY environment variable not set"

**Solutions**:
1. Install X server (VcXsrv on Windows for WSL2, or native on Linux)
2. Set DISPLAY variable: `export DISPLAY=:0`
3. Add DISPLAY to MCP config env section
4. Test X server: `xclock` (should show a clock window)

## Examples

See the [examples/](./examples/) directory for detailed use case demonstrations:

- **Research**: Multi-source fact-checking and report writing
- **Coding**: Building complete applications with testing
- **Analysis**: Data processing and visualization
- **Automation**: Workflow automation and scripting

## Configuration Options

### Customizing the System Prompt

You can modify `system-prompt.md` to adjust behavior:

- **Language Settings**: Change default working language
- **Writing Style**: Adjust verbosity and formatting preferences
- **Tool Preferences**: Prioritize certain tools over others
- **Error Handling**: Change how errors are reported and handled

### Adding More MCP Servers

The plugin is designed to work with additional MCP servers. See `mcp-config.json` for optional servers and instructions on adding new ones.

Popular additions:
- **Database MCP**: For direct database access
- **API MCP**: For specific API integrations
- **Cloud Services MCP**: For AWS, GCP, Azure operations

## Contributing

Found a bug or have a suggestion? Please contribute!

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

This plugin inherits the AGPL-3.0 license from MyManus. See [LICENSE](../LICENSE) for details.

## Support

- **Issues**: Report bugs at [GitHub Issues](https://github.com/emsi/MyManus/issues)
- **Discussions**: Join discussions about features and use cases
- **Documentation**: Full MyManus documentation at [main README](../README.md)

## Acknowledgments

- **MyManus**: Original project by [@emsi](https://github.com/emsi)
- **Claude Code**: Official CLI by [Anthropic](https://anthropic.com)
- **MCP**: Model Context Protocol by Anthropic
- **Playwright**: Browser automation by Microsoft

---

**Ready to transform Claude Code into an autonomous agent?** Start with the [Installation](#installation) section above!
