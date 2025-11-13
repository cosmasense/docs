# CosmaSense Documentation

Official documentation for CosmaSense - an AI-powered local file search engine.

## About CosmaSense

CosmaSense is an open-source local file indexing and search engine that finds your files using natural language queries. It runs 100% locally on your device, giving you powerful semantic search capabilities without compromising your privacy.

**Think of it as Google for your file system.**

### Key Features

- 🔍 **Hybrid Search** - Combines semantic (vector) and keyword (FTS5) search
- 🤖 **AI-Powered** - Automatically generates summaries and keywords
- 🔒 **100% Local** - All processing happens on your device
- ⚡ **Real-time** - Watches directories and auto-indexes changes
- 📁 **20+ File Types** - PDF, DOCX, images, code, spreadsheets, and more

### Repository Links

- **Main Repository**: [cosmasense/cosma](https://github.com/cosmasense/cosma)
- **Documentation Site**: [cosmasense/docs](https://github.com/cosmasense/docs)

## Development

This documentation is built with [Mintlify](https://mintlify.com).

### Local Preview

Install the Mintlify CLI:

```bash
npm i -g mint
```

Run the preview server:

```bash
mint dev
```

View your local preview at `http://localhost:3000`.

### Publishing Changes

Changes pushed to the main branch are automatically deployed to production.

## Documentation Structure

```
.
├── index.mdx                    # Homepage
├── quickstart.mdx               # Installation guide
├── development.mdx              # Backend architecture
├── api-reference/               # API endpoints
│   ├── introduction.mdx
│   └── endpoint/
│       ├── create.mdx          # Watch directory
│       ├── get.mdx             # Search files
│       ├── delete.mdx          # Get watched directories
│       └── webhook.mdx         # SSE updates
└── docs.json                   # Navigation config
```

## Contributing

We welcome contributions! Please see our [contributing guidelines](https://github.com/cosmasense/cosma/blob/main/CONTRIBUTING.md).

## License

This documentation is open source under the MIT License.

## Need Help?

- 📖 [View Documentation](https://docs.cosmasense.com)
- 🐛 [Report Issues](https://github.com/cosmasense/cosma/issues)
- 💬 [Discussions](https://github.com/cosmasense/cosma/discussions)
