# AutomateAI MCP | Enterprise MCP Aggregation & Orchestration Suite <!-- omit in toc -->

<div align="center">

</div>

**AutomateAI MCP** is an enterprise MCP solution that lets you deploy and configure MCP servers into a single secure MCP solution. Need customizable security, department-specific access to software or data, and shared enterprise-wide access for some resources but not others? You've found the right system. 

Company's big and small can setup and deploy the AutomateAI MCP as core cloud or locally hosted infrastructure to host dynamically deployed MCP servers through a unified access point and then customize the entire infrastructures access capabilities, configurations, and connections with both local and remote mcp servers and external software solutions. 

## 📖 Concepts

### 🖥️ **MCP Server**
A JSON-based MCP configuration that tells the system how to start a local MCP server.

```json
"HackerNews": {
  "type": "STDIO",
  "command": "uvx",
  "args": ["mcp-hn"]
}
```

#### 🔐 **Environment Variables & Secrets (STDIO MCP Servers)**

For **STDIO MCP servers**, AutomateAI MCP supports three ways to handle environment variables and secrets:

**1. Raw Values** - Direct string values (not recommended for secrets):
```
API_KEY=your-actual-api-key-here
DEBUG=true
```

**2. Environment Variable References** - Use `${ENV_VAR_NAME}` syntax:
```
API_KEY=${OPENAI_API_KEY}
DATABASE_URL=${DB_CONNECTION_STRING}
```

**3. Auto-matching** - If the expected environment variable name in your tool matches the container's environment variable, you can omit it entirely. AutomateAI MCP will automatically pass through matching environment variables.

> **🔒 Security Note**: Environment variable references (`${VAR_NAME}`) are resolved from the AutomateAI container's environment at runtime. This keeps actual secret values out of your configuration and git repository.

> **⚙️ Development Note**: For local development with `pnpm run dev:docker`, ensure your environment variables are listed in `turbo.json` under `globalEnv` to be passed to the development processes. This is not required for production Docker deployments.

### 🏷️ **AutomateAI Namespace**
- Group one or more MCP servers into a namespace
- Enable/disable MCP servers or at tool level
- Apply middlewares to MCP requests and responses
- Override tool names/titles/descriptions per namespace and attach custom MCP annotations (e.g. `{ "annotations": { "readOnlyHint": false } }`)

### 🌐 **AutomateAI Endpoint**
- Create endpoints and assign namespace to endpoints
- Multiple MCP servers in the namespace will be aggregated and emitted as a AutomateAI MCP endpoint
- Choose between API-Key Auth (in header or query param) or standard OAuth in MCP Spec June 18, 2025
- Host through **SSE** or **Streamable HTTP** transports in MCP and **OpenAPI** endpoints for clients 

### ⚙️ **Middleware**
- Intercepts and transforms MCP requests and responses at namespace level
- **Built-in example**: "Filter inactive tools" - optimizes tool context for LLMs
- **Future ideas**: tool logging, error traces, validation, scanning

### 🔍 **Inspector**
Similar to the official MCP inspector, but with **saved server configs** - AutomateAI MCP automatically creates configurations so you can debug MCP endpoints immediately.

### ✏️ **Tool Overrides & Annotations**
- Open a namespace → **Tools** tab to see every tool coming from connected MCP servers.
- Each saved tool can be expanded and edited inline: update the display **name/title/description** or provide a JSON blob with namespace-specific annotations (for example `{ "annotations": { "readOnlyHint": false } }`).
- Badges in the table ("Overridden", "Annotations") show which tools currently have custom metadata. Hover them to read a tooltip describing what was overridden.
- Annotation overrides are merged with whatever the upstream MCP server returns, so you can safely add custom UI hints without losing provider metadata.
