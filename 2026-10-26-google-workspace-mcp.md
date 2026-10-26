---
title: Google workspace mcp
date: 2026-10-26 04:03 +0000
tags: []
---

To configure Google Workspace as an MCP server in LibreChat, you connect each workspace product (Gmail, Drive, Calendar, etc.) as an OAuth-enabled remote MCP server. [1] (https://www.librechat.ai/docs/mcp_servers/google_workspace)
The setup involves two phases: configuring a project in the Google Cloud Console and adding the configuration to LibreChat. [1] (https://www.librechat.ai/docs/mcp_servers/google_workspace), [2] (https://developers.google.com/workspace/guides/configure-mcp-servers)

Phase 1: Google Cloud Console Setup

1. Create/Select a Project: Go to the Google Cloud Console and create a new project (e.g., LibreChat Workspace MCP). [1] (https://www.librechat.ai/docs/mcp_servers/google_workspace)
2. Enable APIs & MCP Services: Search for and enable the specific APIs you intend to use (e.g., Google Drive API, Gmail API, Google Calendar API). [1] (https://developers.google.com/workspace/guides/configure-mcp-servers)
3. Configure the Chat App (Optional): If you plan to use Google Chat MCP, search for the Google Chat API, go to Manage > Configuration, name it Chat MCP, and turn off "Enable interactive features". [1] (https://www.librechat.ai/docs/mcp_servers/google_workspace), [2] (https://developers.google.com/workspace/guides/configure-mcp-servers)
4. Configure OAuth Consent Screen:
	• Go to Google Auth Platform > Branding.
	• Set your App Name (e.g., LibreChat Workspace).
	• Under Audience, select Internal if you have a corporate Google Workspace account. Select External if you are using a personal @gmail.com account (and manually add your email to the Test Users list). [1] (https://www.librechat.ai/docs/mcp_servers/google_workspace), [2] (https://developers.google.com/workspace/guides/configure-mcp-servers)
5. Create OAuth Client ID:
	• Go to Clients > Create Client.
	• Select Web Application.
	• Authorized Redirect URIs: Add your LibreChat callback URL:
		• Local Development: http://localhost:3080/api/mcp/google/oauth/callback
		• Production: https://your-domain.com
	• Save and copy your Client ID and Client Secret. [1] (https://www.librechat.ai/docs/mcp_servers/salesforce), [2] (https://developers.google.com/workspace/guides/configure-mcp-servers), [3] (https://www.librechat.ai/docs/mcp_servers/google_workspace)

Phase 2: LibreChat Configuration

Add your credentials to your .env file first to securely manage them, then update your librechat.yaml file. [1] (https://www.youtube.com/watch?v=q_yCWQChQUY)

1. Update your .env file:

env
GOOGLE_WORKSPACE_CLIENT_ID=your_client_id_here
GOOGLE_WORKSPACE_CLIENT_SECRET=your_client_secret_here
Use code with caution.

2. Update your librechat.yaml:

Each Google Workspace tool operates as its own remote server endpoint. Below is an example adding Google Drive and Gmail under mcpServers: [1] (https://www.librechat.ai/docs/mcp_servers/google_workspace), [2] (https://www.librechat.ai/docs/configuration/librechat_yaml/object_structure/config)
yaml
mcpServers:
  google-drive:
    type: "streamable-http"
    url: "https://googleapis.com"
    auth:
      type: "oauth2"
      clientId: "{{GOOGLE_WORKSPACE_CLIENT_ID}}"
      clientSecret: "{{GOOGLE_WORKSPACE_CLIENT_SECRET}}"
      authorizationUrl: "https://google.com"
      tokenUrl: "https://googleapis.com"
      scopes:
        - "https://googleapis.com"

  google-gmail:
    type: "streamable-http"
    url: "https://googleapis.com"
    auth:
      type: "oauth2"
      clientId: "{{GOOGLE_WORKSPACE_CLIENT_ID}}"
      clientSecret: "{{GOOGLE_WORKSPACE_CLIENT_SECRET}}"
      authorizationUrl: "https://google.com"
      tokenUrl: "https://googleapis.com"
      scopes:
        - "https://googleapis.com"
Use code with caution.
Note: If your librechat.yaml employs a strict mcpSettings.allowedDomains security whitelist, make sure to add drivemcp.googleapis.com and gmailmcp.googleapis.com to it so LibreChat is permitted to communicate out to them. [1] (https://www.librechat.ai/docs/mcp_servers/google_workspace)

Phase 3: Authenticating as a User

Once you restart LibreChat: [1] (https://www.librechat.ai/docs/configuration/librechat_yaml/object_structure/config)
1. Open LibreChat and open your chat window or the Agent Builder.
2. Click the + (palette icon) next to the message field or open the MCP Settings sidebar panel.
3. Click Connect next to the Google Workspace server.
4. A browser window will open asking you to log into your Google account and approve the access permissions. [1] (https://www.librechat.ai/docs/features/mcp), [2] (https://www.librechat.ai/docs/mcp_servers/google_workspace)
Once approved, your LLM will be able to search your drive, write draft emails, and read threads directly inside the chat interface. [1] (https://www.librechat.ai/docs/mcp_servers/google_workspace)
Would you like the exact configurations and URL endpoints for the other Google Workspace apps like Calendar, Chat, or People?
