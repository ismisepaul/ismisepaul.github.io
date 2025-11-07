---
layout: post
title:  "Setting up Gemini with Cycode MCP"
date:   2025-11-07 09:36:34 +0000
categories: ai mcp llm
tag: gemini cycode mcp
---

This is a quick getting up and running wth [Google Gemini](https://deepmind.google/models/gemini/) and Cycode's [MCP server](https://cycode.com/blog/introducing-cycodes-mcp-server/). Assumes MacOS.


# Gemini


```bash
brew install gemini-cli
```

## Authenticating

I'm authenticating using the Vertex AI Authentication so I need to have the `gcloud` CLI tool installed, which mean a 
certain version of Python is needed. Ensure you have Python `3.11` installed as `3.12` and above have removed the `imp` 
module that `gcloud` uses. You'll see this error:

```python
Traceback (most recent call last):
  File "/Users/ismisepaul/opt/google-cloud-sdk/lib/gcloud.py", line 132, in <module>
    main()
    ~~~~^^
  File "/Users/ismisepaul/opt/google-cloud-sdk/lib/gcloud.py", line 90, in main
    from googlecloudsdk.core.util import encoding
  File "/Users/ismisepaul/opt/google-cloud-sdk/lib/googlecloudsdk/__init__.py", line 23, in <module>
    from googlecloudsdk.core.util import importing
  File "/Users/ismisepaul/opt/google-cloud-sdk/lib/googlecloudsdk/core/util/importing.py", line 23, in <module>
    import imp
ModuleNotFoundError: No module named 'imp'
```

Fix:

```bash
brew install python@3.11
```

Add the following line (check the location is correct)

```bash
vim ~/.zshrc
```

`export CLOUDSDK_PYTHON="/opt/homebrew/opt/python@3.11/bin/python3.11"`


Install

```bash
brew install google-cloud-sdk
```

Authenticate

```bash
gcloud auth application-default login
```

Setup environment variables for authentication

```bash
mkdir -p ~/.gemini
```

```bash
cat > ~/.gemini/.env <<'EOF'
GOOGLE_CLOUD_PROJECT=<YOUR_GCLOUD_PROJECT_NAME>
GOOGLE_GENAI_USE_VERTEXAI=true
GOOGLE_CLOUD_LOCATION=global
EOF
```

```bash
# Test connection
gemini -m gemini-2.5-flash -p "ping"
```

# Cycode

- Ref: https://mcp.so/server/cycode

Install the CLI 

```bash
brew install cycode
```

Authenticate 

```bash
cycode auth
```

Create the MCP settings.json

```bash
vim .gemini/settings.json
```

Add the following to the file, replacing with your secrets and environment details

- Go to "My Profile" to see your tenantId
- Go to https://app.cycode.com/personal-access-token for secret key

```json
{
  "mcpServers": {
    "cycode": {
      "command": "uvx",
      "args": ["cycode", "mcp"],
      "env": {
        "CYCODE_CLIENT_ID": "<CYCODE-tenantId>",
        "CYCODE_CLIENT_SECRET": "your-cycode-secret-key",
        "CYCODE_API_URL": "https://api.cycode.com",
        "CYCODE_APP_URL": "https://app.cycode.com"
      }
    }
  }
}

```

Ensure `uv` is installed to ensure Gemini can communicate to the MCP server

```bash
bew install uv
```

Start the server

```bash
cycode mcp 
```


Verify 

```bash
gemini mcp list
```

```
Configured MCP servers:

✓ cycode: uvx cycode mcp (stdio) - Connected
```

# Running an SCA Scan

Create a project folder

```bash
mkdir -p gemini-projects/cycode
cd gemini-projects/cycode
```

Pull an open source project

```bash
git clone https://github.com/elastic/synthetics.git
```

Run the SCA tool 

```bash
# Interactive Gemini Session
gemini
# Run Cycode SCA Scan
cycode_sca_scan directory=/Users/pmccann/IdeaProjects/elastic/synthetics
```