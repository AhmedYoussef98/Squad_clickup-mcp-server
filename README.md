# ClickUp MCP Server (Squad Fork)

A Model Context Protocol (MCP) server for integrating ClickUp tasks with AI applications. This server allows AI agents to interact with ClickUp tasks, spaces, lists, and folders through a standardized protocol.

> **Free & Open Source**: This is a community-maintained fork, kept free and open-source for everyone.

## Features

- 🔄 **Resource Management**
  - List and read ClickUp tasks as resources
  - View task details, status, and assignments
  - Access task history and relationships

- 📂 **Workspace Organization**
  - Create and manage spaces
  - Create, update, and delete folders
  - Create and manage lists (in spaces or folders)
  - Flexible identification using IDs or names

- ✨ **Task Operations**
  - Create single or bulk tasks
  - Move tasks between lists
  - Duplicate tasks
  - Set priorities and due dates
  - Assign team members

- 📊 **Information Retrieval**
  - Get complete hierarchy of spaces, folders, and lists with IDs
  - List available statuses
  - Find items by name (case-insensitive)
  - View task relationships

- 🔍 **Smart Name Resolution**
  - Use names instead of IDs for lists and folders
  - Global search across all spaces
  - Case-insensitive matching
  - Automatic location of items

- 📝 **AI Integration**
  - Generate task descriptions with AI
  - Summarize tasks and analyze priorities
  - Get AI-powered task recommendations

- 🔒 **Security**
  - Secure API key management
  - Environment-based configuration

## Installation

### Local Installation

1. **Clone the repository:**
```bash
git clone https://github.com/AhmedYoussef98/Squad_clickup-mcp-server.git
cd Squad_clickup-mcp-server
```

2. **Install dependencies:**
```bash
npm install
```

3. **Build the project:**
```bash
npm run build
```

4. **Configure your AI client** (see Configuration section below)

## Configuration

### Get Your ClickUp Credentials

1. **API Key**: Get from [ClickUp Settings > Apps](https://app.clickup.com/settings/apps)
2. **Team ID**: Found in your ClickUp workspace URL or via the API

### For Claude Desktop

Add to your Claude Desktop configuration file:

**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`  
**Windows**: `%APPDATA%/Claude/claude_desktop_config.json`
```json
{
  "mcpServers": {
    "clickup": {
      "command": "node",
      "args": ["/FULL/PATH/TO/Squad_clickup-mcp-server/build/index.js"],
      "env": {
        "CLICKUP_API_KEY": "your_api_key_here",
        "TEAM_ID": "your_team_id_here"
      }
    }
  }
}
```

> **Important**: Replace `/FULL/PATH/TO/Squad_clickup-mcp-server` with the actual absolute path where you cloned the repository.

### For Cursor AI

1. Go to Settings → Features → MCP Servers
2. Add the following configuration:
```json
{
  "clickup": {
    "command": "node",
    "args": ["/FULL/PATH/TO/Squad_clickup-mcp-server/build/index.js"],
    "env": {
      "CLICKUP_API_KEY": "your_api_key_here",
      "TEAM_ID": "your_team_id_here"
    }
  }
}
```

### For Cline (VS Code)

Add to your Cline MCP settings:
```json
{
  "mcpServers": {
    "clickup": {
      "command": "node",
      "args": ["/FULL/PATH/TO/Squad_clickup-mcp-server/build/index.js"],
      "env": {
        "CLICKUP_API_KEY": "your_api_key_here",
        "TEAM_ID": "your_team_id_here"
      }
    }
  }
}
```

> **Security Note**: Your API key is stored locally and is never exposed to AI models or external services.

## Available Tools

### 1. workspace_hierarchy
Lists complete hierarchy of the ClickUp workspace
- Shows spaces, folders, and lists with their IDs
- Shows available statuses for each list
- Provides a tree view of your workspace organization
- No parameters required

### 2. create_task
Creates a new task in ClickUp

**Required:**
- `name`: Name of the task

**Optional:**
- `listId`: ID of the list (optional if listName provided)
- `listName`: Name of the list (optional if listId provided)
- `description`: Task description
- `status`: Task status
- `priority`: Priority level (1-4)
- `dueDate`: Due date (ISO string)
- `assignees`: Array of user IDs

### 3. create_bulk_tasks
Creates multiple tasks simultaneously in a list

**Required:**
- `tasks`: Array of task objects, each containing:
  - `name`: Name of the task (required)
  - `description`: Task description (optional)
  - `status`: Task status (optional)
  - `priority`: Priority level 1-4 (optional)
  - `dueDate`: Due date ISO string (optional)
  - `assignees`: Array of user IDs (optional)

**Optional:**
- `listId`: ID of the list (optional if listName provided)
- `listName`: Name of the list (optional if listId provided)

### 4. create_list
Creates a new list in a space

**Required:**
- `name`: Name of the list

**Optional:**
- `spaceId`: ID of the space (optional if spaceName provided)
- `spaceName`: Name of the space (optional if spaceId provided)
- `content`: List description
- `status`: List status
- `priority`: Priority level (1-4)
- `dueDate`: Due date (ISO string)

### 5. create_folder
Creates a new folder in a space

**Required:**
- `name`: Name of the folder

**Optional:**
- `spaceId`: ID of the space (optional if spaceName provided)
- `spaceName`: Name of the space (optional if spaceId provided)
- `override_statuses`: Whether to override space statuses

### 6. create_list_in_folder
Creates a new list within a folder

**Required:**
- `name`: Name of the list

**Optional:**
- `folderId`: ID of the folder (optional if using folderName)
- `folderName`: Name of the folder
- `spaceId`: ID of the space (required if using folderName)
- `spaceName`: Name of the space (alternative to spaceId)
- `content`: List description
- `status`: List status

### 7. move_task
Moves a task to a different list

**Required:**
- `taskId`: ID of the task to move

**Optional:**
- `listId`: ID of destination list (optional if listName provided)
- `listName`: Name of destination list (optional if listId provided)

### 8. duplicate_task
Creates a copy of a task in a specified list

**Required:**
- `taskId`: ID of the task to duplicate

**Optional:**
- `listId`: ID of destination list (optional if listName provided)
- `listName`: Name of destination list (optional if listId provided)

### 9. update_task
Updates an existing task

**Required:**
- `taskId`: ID of the task to update

**Optional:**
- `name`: New task name
- `description`: New description
- `status`: New status
- `priority`: New priority level (1-4)
- `dueDate`: New due date (ISO string)

## Available Prompts

### 1. summarize_tasks
Provides a comprehensive summary of tasks
- Groups tasks by status
- Highlights priorities and deadlines
- Suggests task relationships

### 2. analyze_priorities
Analyzes task priority distribution
- Identifies misaligned priorities
- Suggests priority adjustments
- Recommends task sequencing

### 3. generate_description
Helps generate detailed task descriptions
- Clear objectives
- Success criteria
- Required resources
- Dependencies
- Potential risks

## Name Resolution

Most tools support finding items by either ID or name:
- Spaces can be referenced by `spaceId` or `spaceName`
- Folders can be referenced by `folderId` or `folderName` (with space information)
- Lists can be referenced by `listId` or found within spaces/folders

Name matching is case-insensitive for convenience.

## Error Handling

The server provides clear error messages for common scenarios:
- Missing required parameters
- Invalid IDs or names
- Items not found
- Permission issues
- API errors

## Development

### Setup
```bash
git clone https://github.com/AhmedYoussef98/Squad_clickup-mcp-server.git
cd Squad_clickup-mcp-server
npm install
```

### Development Mode
```bash
npm run dev
```

### Build
```bash
npm run build
```

## Troubleshooting

### Server Not Connecting
1. Verify the path in your config is absolute (not relative)
2. Check that `npm run build` completed successfully
3. Restart your AI client after configuration changes

### Invalid API Key
1. Verify your API key at [ClickUp Settings](https://app.clickup.com/settings/apps)
2. Ensure no extra spaces in the key
3. Check that the Team ID is correct

### Tasks Not Creating
1. Run `workspace_hierarchy` to verify list IDs
2. Check that you have permissions in ClickUp
3. Verify list names match exactly (case-insensitive)

## Contributing

Contributions are welcome! Feel free to:
- Report bugs
- Suggest features
- Submit pull requests

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Questions?** Open an issue on [GitHub](https://github.com/AhmedYoussef98/Squad_clickup-mcp-server/issues).
