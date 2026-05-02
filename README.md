# OICRepository
NLIT 2026 Agentic AI Integration Session

## Prompt

## LangFlow MCP Config

{
  "mcpServers": {
    "OIC_Invoice_Creation_Project": {
      "disabled": true,
      "timeout": 300,
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "supergateway",
        "--streamableHttp",
    "https://newgen3fortestingocifunctions345-natdcshjumpstartprod-px.integration.us-phoenix-1.ocp.oraclecloud.com/mcp-server/v1/projects/NLIT/mcp",
        "--oauth2Bearer",
"Token"
      ]
    }
  }
}
