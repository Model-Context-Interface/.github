# Model Context Interface (MCI)

Model Context Interface (MCI) is an open-source specification and set of lightweight language adapters that let AI agents discover and execute tools defined in a static JSON file. The JSON describes tools, metadata, and execution instructions (HTTP requests, CLI commands, file read and text). MCI adapters read the JSON, validate it and execute tools synchronously, returning structured results to the agent.

MCI is an alternative or a supplement to MCP (Model Context Protocol) that prioritizes synchronous execution, minimal runtime dependencies, and fine-grained context control.

Alongside with other cool things, MCI can help you:
- Avoid installing remote/third‑party servers — the JSON is static and adapters run locally.
- Enable synchronous tool execution for languages and developers that prefer synchronous (e.g., PHP, Python).
- Reduce token and runtime overhead by providing small, specific context files tailored per agent.
- Make APIs and CLI utilities immediately usable by LLMs through a simple, auditable JSON format.

## Main Features
- JSON Definition Format: tools, metadata, inputs, and an explicit execution section.
- Execution types: HTTP Request, CLI Command, File & Text.
- Secrets & environment interpolation using `{{env.VAR_NAME}}`.
- Authentication methods: API key (header), Bearer, Basic auth, OAuth2 (client credentials).
- Local validation against a schema before execution.
- Template Engine - Dynamic value substitution for environment variables and properties, as well as advanced templating directives like `@if`, `@foreach`, etc. to support complex prompting.

## How it Works (high level)
1. Author a small JSON file describing tools and execution instructions.
2. Adapter loads and validates JSON, exposes tool list to the agent.
3. Agent requests a tool execution.
4. Adapter executes the mapped action synchronously, substitutes env vars/secrets as well as properties of tool (`{{props.username}}`), then returns structured output.

## Example JSON

```json
{
  "schemaVersion": "1.0",
  "metadata": {
    "name": "Example MCI Tools",
    "description": "Example JSON file demonstrating various MCI execution types",
    "version": "1.0.0",
    "license": "MIT",
    "authors": [
      "MCI Development Team"
    ]
  },
  "tools": [
    {
      "name": "get_weather",
      "title": "Get Weather Information",
      "description": "Fetch current weather information for a location using HTTP GET request",
      "inputSchema": {
        "type": "object",
        "properties": {
          "location": {
            "type": "string",
            "description": "City name or location"
          }
        },
        "required": [
          "location"
        ]
      },
      "execution": {
        "type": "http",
        "method": "GET",
        "url": "https://api.example.com/weather",
        "params": {
          "location": "{{props.location}}",
          "units": "metric"
        },
        "headers": {
          "Accept": "application/json"
        },
        "timeout_ms": 5000
      }
    },
  ]
}
```

## Advantages
- Synchronous execution model fits many server-side languages and developer workflows.
- Secure by design: No remote or third-party servers required — JSON + local adapter reduces supply-chain attack surface.
- Fine-grained, per-agent context reduces unnecessary tooling exposure and token cost.
- Language-agnostic: any language that can make HTTP requests and run CLI commands can implement an adapter.
- Easy to build: Share MCI Schema reference and documentation of any REST API or CLI application to LLM and build your favorite toolset in minute
- Easy to share: All you need to move your toolset to another project, or share it to the world is a single JSON file.



## Disadvantages / Trade-offs
- Static JSON cannot express arbitrary runtime logic as flexibly as full server-side plugin architectures.
- Local adapters must be implemented/maintained per language for parity.


## Getting Started
1. Create a small tools JSON describing the actions you need.
2. Install/run the language adapter ([Python adapter](https://github.com/Model-Context-Interface/mci-py)).
3. Point your agent or application to the adapter and invoke tools synchronously.


## Planning

- Library for ease of sharing of tools
- Package manager to download/update toolsets faster

## Contributing & Contact
Owner: Revaz Ghambarashvili (MaestroError)  <revaz.gh@gmail.com>

Contributions welcome via PRs and issues. 
Please include schema updates, adapter implementations, and security hardening suggestions.
