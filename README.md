# Youtube MCP server

- [Youtube MCP server](#youtube-mcp-server)
  - [About](#about)
  - [What is MCP?](#what-is-mcp)
  - [What does this server do?](#what-does-this-server-do)
  - [Practical use cases](#practical-use-cases)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Configuration](#configuration)
    - [Claude Desktop Configuration](#claude-desktop-configuration)
  - [Development](#development)
    - [Getting started](#getting-started)
    - [Debugging the server in the Inspector](#debugging-the-server-in-the-inspector)
  - [Troubleshooting](#troubleshooting)
    - [Message 'Could not connect to MCP server mcp-youtube'](#message-could-not-connect-to-mcp-server-mcp-youtube)

## About

The server is a bridge between the Youtube API and the AI assistants and is based on the [Model Context Protocol](https://modelcontextprotocol.io).

<a href="https://glama.ai/mcp/servers/gzrh7914k6">
  <img width="380" height="200" src="https://glama.ai/mcp/servers/gzrh7914k6/badge" alt="Youtube Server MCP server" />
</a>

## What is MCP?

The Model Context Protocol (MCP) is a system that lets AI apps, like Claude Desktop, connect to external tools and data sources. It gives a clear and safe way for AI assistants to work with local services and APIs while keeping the user in control.

## What does this server do?

- [x] Download closed captions for the given video

## Practical use cases

- [x] Create a summary of the video

## Prerequisites

- [`uv` tool](https://docs.astral.sh/uv/getting-started/installation/)

## Installation

```bash
uv tool install git+https://github.com/sparfenyuk/mcp-youtube
```

> [!NOTE]
> If you have already installed the server, you can update it using `uv tool upgrade --reinstall` command.

> [!NOTE]
> If you want to delete the server, use the `uv tool uninstall mcp-youtube` command.

## Configuration

### Claude Desktop Configuration

Configure Claude Desktop to recognize the Youtube MCP server.

1. Open the Claude Desktop configuration file:
   - in MacOS, the configuration file is located at `~/Library/Application Support/Claude/claude_desktop_config.json`
   - in Windows, the configuration file is located at `%APPDATA%\Claude\claude_desktop_config.json`

   > __Note:__
   > You can also find claude_desktop_config.json inside the settings of Claude Desktop app

2. Add the server configuration

    ```json
    {
      "mcpServers": {
        "mcp-youtube": {
            "command": "mcp-youtube",
          }
        }
      }
    }
    ```

## Development

### Getting started

1. Clone the repository
2. Install the dependencies

   ```bash
   uv sync
   ```

3. Run the server

   ```bash
   uv run mcp-youtube --help
   ```

Tools can be added to the `src/mcp_youtube/tools.py` file.

How to add a new tool:

1. Create a new class that inherits from ToolArgs

   ```python
   class NewTool(ToolArgs):
       """Description of the new tool."""
       pass
   ```

   Attributes of the class will be used as arguments for the tool.
   The class docstring will be used as the tool description.

2. Implement the tool_runner function for the new class

   ```python
   @tool_runner.register
   async def new_tool(args: NewTool) -> t.Sequence[TextContent | ImageContent | EmbeddedResource]:
       pass
   ```

   The function should return a sequence of TextContent, ImageContent or EmbeddedResource.
   The function should be async and accept a single argument of the new class.

3. Done! Restart the client and the new tool should be available.

Validation can accomplished either through Claude Desktop or by running the tool directly.

### Debugging the server in the Inspector

The MCP inspector is a tool that helps to debug the server using fancy UI. To run it, use the following command:

```bash
npx @modelcontextprotocol/inspector uv run mcp-youtube
```

## Troubleshooting

### Message 'Could not connect to MCP server mcp-youtube'

If you see the message 'Could not connect to MCP server mcp-youtube' in Claude Desktop, it means that the server configuration is incorrect.

Try the following:

- Use the full path to the `mcp-youtube` binary in the configuration file