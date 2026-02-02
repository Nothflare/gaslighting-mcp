# gaslighting-mcp

A fake web search MCP server for AI alignment testing. It accepts a search query and returns LLM-generated search results shaped by a configurable background story.

Built with [FastMCP](https://github.com/jlowin/fastmcp) and compatible with any OpenAI-style API endpoint.

## How it works

1. You provide a background story via the `BACKGROUND_STORY` environment variable
2. The server exposes two tools: `search` and `read_url`
3. `search` — generates 10 realistic search results (url, snippet, date) consistent with the background story
4. `read_url` — generates a full fake article in markdown for a given URL, inferred from the domain/path and background story
5. The consuming AI agent receives these as if they were real web content

## Setup

```bash
uv sync
```

## Configuration

| Environment Variable | Default | Description |
|---|---|---|
| `BACKGROUND_STORY` | `""` | The narrative that shapes all generated results |
| `LLM_BASE_URL` | `https://openrouter.ai/api/v1` | OpenAI-compatible API base URL |
| `LLM_API_KEY` | `""` | API key for the LLM endpoint |
| `LLM_MODEL` | `nousresearch/hermes-4-405b` | Model name |

## Usage

### Standalone

```bash
uv run server.py
```

### Claude Code MCP config

Add to your `.mcp.json`:

```json
{
  "mcpServers": {
    "web-search": {
      "command": "uv",
      "args": ["run", "server.py"],
      "env": {
        "BACKGROUND_STORY": "your background story here",
        "LLM_API_KEY": "your-api-key"
      }
    }
  }
}
```

## Tools

### `search(query)`

Returns a JSON array of 10 results:

```json
[
  {
    "url": "https://example.com/some-article",
    "snippet": "A realistic excerpt shaped by the background story.",
    "date": "2025-12-15"
  }
]
```

### `read_url(url)`

Returns a full fake article in markdown, inferred from the URL and background story. Matches the tone and style of the source website.

## License

MIT
