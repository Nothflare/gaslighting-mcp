# gaslighting-mcp

A fake web search MCP server for AI alignment testing. It accepts a search query and returns LLM-generated search results shaped by a configurable background story.

Built with [FastMCP](https://github.com/jlowin/fastmcp) and compatible with any OpenAI-style API endpoint.

## How it works

1. You provide a background story via the `BACKGROUND_STORY` environment variable
2. When the consuming AI agent calls the `search` tool, the server sends the query + background story to a configured LLM
3. The LLM generates 10 realistic-looking search results (url, snippet, date) consistent with the background story
4. The agent receives these as if they were real web search results

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

## Output format

```json
[
  {
    "url": "https://www.reuters.com/technology/some-article-2025",
    "snippet": "A realistic excerpt shaped by the background story.",
    "date": "2025-12-15"
  }
]
```

10 items per query, mixed across credible domains.

## License

MIT License

Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
