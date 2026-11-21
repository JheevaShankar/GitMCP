# Project and Git MCP Server

An MCP (Model Context Protocol) server for creating new projects, verifying they work correctly, managing Git repositories, and handling Git operations with automatic conflict resolution and auto-push capabilities.

## Features

### Project Management
- **Create Projects**: Create various project types (simple, Python, web)
- **Verify Projects**: Verify project structure and validity
- **Project Types**:
  - `simple`: Basic project with README and .gitignore
  - `python`: Full Python project with setup.py, tests, and package structure
  - `web`: Web project with HTML, CSS, JavaScript, and package.json

### Git Operations
- **Initialize Repositories**: Create Git repos and add remotes
- **Commit Changes**: Stage and commit changes with custom messages
- **Push to Remote**: Push commits to remote repositories
- **Pull with Auto-Resolution**: Pull changes with automatic conflict resolution
- **Conflict Handling**: Automatically handle merge conflicts
- **Auto Commit & Push**: Single operation to stage, commit, and push
- **Remote Management**: Create and manage multiple remotes

### Workflow
- **Complete Workflow**: Create project → Verify → Initialize Git → Commit
- **Status Tracking**: Monitor repository status in real-time

## Installation

```bash
# Clone or download the project
cd project-git-mcp-server

# Install with uv (recommended)
uv pip install -e .

# Or with pip
pip install -e .
```

## Usage

### Starting the Server

```bash
python -m project_git_mcp_server.server
```

Or with uv:

```bash
uv run -p python3 -m project_git_mcp_server.server
```

### Available Tools

#### 1. `create_project`
Create a new project with specified structure.

**Parameters:**
- `project_path` (string): Full path where project will be created
- `project_type` (string, optional): "simple", "python", or "web" (default: "simple")
- `project_name` (string, optional): Project name (default: "MyProject")

**Example:**
```
Create a Python project at /home/user/projects/myapp
project_type: python
project_name: myapp
project_path: /home/user/projects/myapp
```

#### 2. `verify_project`
Verify a project's structure and validity.

**Parameters:**
- `project_path` (string): Path to the project

#### 3. `init_git_repo`
Initialize a Git repository for a project.

**Parameters:**
- `project_path` (string): Path to the project
- `remote_url` (string, optional): Remote URL to add as origin

#### 4. `add_all_changes`
Stage all changes in the repository.

**Parameters:**
- `project_path` (string): Path to the project

#### 5. `commit_changes`
Commit staged changes.

**Parameters:**
- `project_path` (string): Path to the project
- `message` (string): Commit message

#### 6. `push_to_remote`
Push commits to remote repository.

**Parameters:**
- `project_path` (string): Path to the project
- `branch` (string, optional): Branch to push (default: "main")
- `force` (boolean, optional): Force push (default: false)

#### 7. `handle_merge_conflicts`
Handle merge conflicts automatically.

**Parameters:**
- `project_path` (string): Path to the project

#### 8. `pull_with_auto_resolve`
Pull changes with automatic conflict resolution.

**Parameters:**
- `project_path` (string): Path to the project
- `branch` (string, optional): Branch to pull (default: "main")

#### 9. `create_remote`
Create a new remote in the repository.

**Parameters:**
- `project_path` (string): Path to the project
- `remote_name` (string): Remote name (e.g., "origin", "upstream")
- `remote_url` (string): Remote URL

#### 10. `get_repo_status`
Get repository status.

**Parameters:**
- `project_path` (string): Path to the project

#### 11. `auto_commit_and_push`
Automatically stage, commit, and push all changes.

**Parameters:**
- `project_path` (string): Path to the project
- `commit_message` (string, optional): Commit message (default: "Auto commit")
- `branch` (string, optional): Branch to push (default: "main")

#### 12. `create_and_initialize_project`
Complete workflow: create, verify, and initialize Git.

**Parameters:**
- `project_path` (string): Full path for new project
- `project_type` (string, optional): "simple", "python", or "web"
- `project_name` (string, optional): Project name
- `git_remote_url` (string, optional): Remote URL

## Configuration

### VS Code Integration

Add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "project-git-mcp": {
      "type": "stdio",
      "command": "python",
      "args": ["-m", "project_git_mcp_server.server"]
    }
  }
}
```

### Claude Desktop Integration

Add to `~/.config/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "project-git-mcp": {
      "command": "python",
      "args": ["-m", "project_git_mcp_server.server"]
    }
  }
}
```

## Requirements

- Python 3.10+
- Git installed and configured with credentials
- GitPython 3.1.40+
- MCP SDK 1.2.0+

## Git Configuration

Before using the server, ensure Git is configured:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

## Workflow Examples

### Example 1: Create and Push a Python Project

1. Create project:
   - Tool: `create_project`
   - Path: `/home/user/my_app`
   - Type: `python`
   - Name: `my_app`

2. Verify project:
   - Tool: `verify_project`
   - Path: `/home/user/my_app`

3. Initialize Git and set remote:
   - Tool: `init_git_repo`
   - Path: `/home/user/my_app`
   - Remote: `https://github.com/username/my_app.git`

4. Auto commit and push:
   - Tool: `auto_commit_and_push`
   - Path: `/home/user/my_app`
   - Message: `Initial project setup`

### Example 2: Handle Changes with Auto-Resolve

When changes are made to your project:

1. Pull with auto-resolution:
   - Tool: `pull_with_auto_resolve`
   - Path: `/home/user/my_app`

2. Make your changes and auto commit+push:
   - Tool: `auto_commit_and_push`
   - Path: `/home/user/my_app`
   - Message: `Update features`

## Error Handling

The server handles various error scenarios:

- **Missing repository**: Creates or initializes if needed
- **Merge conflicts**: Automatically accepts remote version (theirs)
- **Push failures**: Provides detailed error messages
- **Invalid project**: Returns validation errors with details

## Logging

Logs are written to stderr for STDIO transport. Enable debug logging:

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## License

MIT

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Support

For issues and questions, please open an issue on GitHub.
