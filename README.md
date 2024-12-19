# RAG Documentation MCP Server

An MCP server implementation that provides tools for retrieving and processing documentation through vector search, enabling AI assistants to augment their responses with relevant documentation context.

<a href="https://glama.ai/mcp/servers/54hsrjhmq9"><img width="380" height="200" src="https://glama.ai/mcp/servers/54hsrjhmq9/badge" alt="mcp-ragdocs MCP server" /></a>

## Features

- Vector-based documentation search and retrieval
- Support for multiple documentation sources
- Semantic search capabilities
- Automated documentation processing
- Real-time context augmentation for LLMs

## Tools

### search_documentation
Search through stored documentation using natural language queries. Returns matching excerpts with context, ranked by relevance.

**Inputs:**
- `query` (string): The text to search for in the documentation. Can be a natural language query, specific terms, or code snippets.
- `limit` (number, optional): Maximum number of results to return (1-20, default: 5). Higher limits provide more comprehensive results but may take longer to process.

### list_sources
List all documentation sources currently stored in the system. Returns a comprehensive list of all indexed documentation including source URLs, titles, and last update times. Use this to understand what documentation is available for searching or to verify if specific sources have been indexed.

### extract_urls
Extract and analyze all URLs from a given web page. This tool crawls the specified webpage, identifies all hyperlinks, and optionally adds them to the processing queue.

**Inputs:**
- `url` (string): The complete URL of the webpage to analyze (must include protocol, e.g., https://). The page must be publicly accessible.
- `add_to_queue` (boolean, optional): If true, automatically add extracted URLs to the processing queue for later indexing. Use with caution on large sites to avoid excessive queuing.

### remove_documentation
Remove specific documentation sources from the system by their URLs. The removal is permanent and will affect future search results.

**Inputs:**
- `urls` (string[]): Array of URLs to remove from the database. Each URL must exactly match the URL used when the documentation was added.

### list_queue
List all URLs currently waiting in the documentation processing queue. Shows pending documentation sources that will be processed when run_queue is called. Use this to monitor queue status, verify URLs were added correctly, or check processing backlog.

### run_queue
Process and index all URLs currently in the documentation queue. Each URL is processed sequentially, with proper error handling and retry logic. Progress updates are provided as processing occurs. Long-running operations will process until the queue is empty or an unrecoverable error occurs.

### clear_queue
Remove all pending URLs from the documentation processing queue. Use this to reset the queue when you want to start fresh, remove unwanted URLs, or cancel pending processing. This operation is immediate and permanent - URLs will need to be re-added if you want to process them later.

## Usage

The RAG Documentation tool is designed for:

- Enhancing AI responses with relevant documentation
- Building documentation-aware AI assistants
- Creating context-aware tooling for developers
- Implementing semantic documentation search
- Augmenting existing knowledge bases

## Configuration

### Usage with Claude Desktop

Add this to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "rag-docs": {
      "command": "npx",
      "args": [
        "-y",
        "@hannesrudolph/mcp-ragdocs"
      ],
      "env": {
        "OPENAI_API_KEY": "",
        "QDRANT_URL": "",
        "QDRANT_API_KEY": ""
      }
    }
  }
}
```

You'll need to provide values for the following environment variables:
- `OPENAI_API_KEY`: Your OpenAI API key for embeddings generation
- `QDRANT_URL`: URL of your Qdrant vector database instance
- `QDRANT_API_KEY`: API key for authenticating with Qdrant

## License

This MCP server is licensed under the MIT License. This means you are free to use, modify, and distribute the software, subject to the terms and conditions of the MIT License. For more details, please see the LICENSE file in the project repository.

## Acknowledgments

This project is a fork of [qpd-v/mcp-ragdocs](https://github.com/qpd-v/mcp-ragdocs), originally developed by qpd-v. The original project provided the foundation for this implementation.
