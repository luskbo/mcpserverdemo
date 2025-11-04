## MCP server demo — quick start

This repository is a minimal example MCP server implemented with FastMCP. 
The instructions below show how to clone the project from GitHub, install the `uv` project manager, use `uv sync` to install dependencies, start the server, and add the server to VS Code as an MCP server.


Prerequisites
- macOS (instructions use zsh). Adjust for other OSes as needed.
- Python 3.11+ (project `pyproject.toml` requests Python >=3.11).

1) Clone the project from GitHub

Use your preferred clone URL from the repository page. Example (HTTPS):

```bash
git clone https://github.com/<owner>/mcpserverdemo.git
cd mcpserverdemo
```

Or with SSH:

```bash
git clone git@github.com:<owner>/mcpserverdemo.git
cd mcpserverdemo
```

2) Install uv (Astral uv)

uv is the recommended project manager used by the `mcp` package and this project. The easiest way to install it is using the official installer:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

After installing, make sure the uv binary is on your PATH. Common location (add this to your `~/.zshrc` if needed):

```bash
export PATH="$HOME/.local/bin:$PATH"
# then reload your shell
source ~/.zshrc
```

Alternative install methods (Homebrew, pip, etc.) are documented at https://docs.astral.sh/uv/getting-started/installation/.

Verify installation:

```bash
which uv
uv --version
```
```pwsh
Get-Command uv | Select-Object Source
```

3) Install dependencies with uv sync

This repository includes a `uv.lock` file. 
Use `uv sync` to install the locked dependencies into the project virtual environment (typically `.venv`):

```bash
cd /path/to/mcpserverdemo
uv sync
```

`uv sync` reads the lockfile and installs the exact packages. 
If you need to update or regenerate the lockfile, see `uv lock` in the uv docs.

4) Start the MCP server

There are a few ways to run the example server. 
The simplest is to run the script inside the uv-managed environment so dependencies are automatically available.

Run the server script directly via uv:

```bash
uv run --directory . mcp_server.py
```

Or use the `mcp` helper shipped by the `mcp` package (some example usages provided by the package):

```bash
# development mode (example)
uv run mcp dev mcp_server.py

# run the mcp CLI (shows available commands)
uv run mcp --help
```

As a fallback, you can activate the created `.venv` and run the script with Python:

```bash
source .venv/bin/activate
python mcp_server.py
```

5) Add the server as an MCP server in VS Code

You can add a local stdio MCP server by creating an `mcp.json` entry for your workspace or user settings. 
Place the file in your workspace (recommended) at `.vscode/mcp.json` with content like this (update the `command` path to the actual `uv` binary on your system):

I typically just use the add tool button in the Agent mode of your VSCode LLM client.

```jsonc
{
  "servers": {
    "mcpserverdemo-local": {
      "type": "stdio",
      "command": "/Users/youruser/.local/bin/uv",
      "args": [
        "run",
        "--directory",
        "${workspaceFolder}",
        "mcp_server.py"
      ]
    }
  },
  "inputs": []
}
```

Notes:
- Replace `/Users/youruser/.local/bin/uv` with the path from `which uv` on your machine.
- Using the `uv run` invocation ensures the script runs inside the correct project environment and picks up dependencies from `.venv`.
- You can also add the same entry to your global VS Code MCP user settings (path shown in VS Code user data) if you want the server available across workspaces.

6) Troubleshooting & tips

- If `uv` isn't found after installing, ensure the installer added the bin directory to your PATH (common locations: `~/.local/bin`, `/usr/local/bin`).
- If the server doesn't start, check `.venv` exists and that `uv sync` completed without errors. Activate `.venv` and try `python mcp_server.py` to see Python exceptions.
- The `mcp` package recommends using `uv` and provides helper commands; run `uv run mcp --help` to explore commands such as `mcp dev`, `mcp install`, and others.

More information
- uv docs: https://docs.astral.sh/uv/
- MCP specification and examples: https://modelcontextprotocol.io/
- FastMCP: https://github.com/modelcontextprotocol/fastmcp

If you'd like, I can also:
- Add a `.vscode/mcp.json` file to this repository with the example entry (I can create it pointing to `uv` assuming a typical macOS install location), or
- Add a simple shell script `start.sh` that runs `uv sync && uv run --directory . mcp_server.py` and make it executable.

---
Completion: README updated with uv + VS Code MCP instructions.
