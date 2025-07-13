![memrok logo](assets/logo/2025-memrok-logo.svg)

# memrok

A truly self-hosted, privacy-first memory service that enables persistent context across AI assistants and conversations. Share your memories between Claude, Cursor, VS Code, and any MCP-compatible client - all while keeping your data on your own infrastructure.

## 🎯 What is memrok?

memrok is a self-hosted memory system that:

- **Stores memories locally** - All data stays on your infrastructure
- **Cross-assistant memory** - Share context between different AI clients
- **Persistent conversations** - Memories survive across sessions
- **Category-based access** - Control which assistants see which memories
- **Just works** - One Docker Compose command to get started

## ✨ Key Features

### 🧠 Memory Management
- **Knowledge Graph Structure** - Entities, relations, and observations like the official MCP memory server
- **Emotional Context** - Track emotional state when memories are created (happy, sad, frustrated, excited)
- **Smart Memory Creation** - AI assistants decide what to memorize and structure it appropriately
- **Category-based Access** - Control which assistants see which memory categories
- **Semantic Search** - Find memories by meaning, not just keywords
- **Automatic Deduplication** - Prevent duplicate memories across sessions

### 🔌 Integrations
- **MCP Server** - Standard Model Context Protocol implementation
- **Web Interface** - Modern UI for memory management
- **REST API** - Programmatic access

### 🚀 Self-Hosting Made Easy
- Single `docker-compose up` deployment
- Works with any reverse proxy (nginx, Traefik, Caddy)
- Configuration via environment variables

## 🏗️ Architecture

```
┌─────────────────┐    ┌─────────────────┐
│   AI Clients    │    │   Web Browser   │
│ Claude, Cursor, │    │   Dashboard     │
│ VS Code, etc.   │    │                 │
└─────────┬───────┘    └─────────┬───────┘
          │                      │
          │ (MCP Protocol)       │ (HTTP via Reverse Proxy)
          │                      │
    ┌─────▼──────────────────────▼─────┐
    │          YROREM Core             │
    │  ┌─────────────┐ ┌─────────────┐ │
    │  │ MCP Server  │ │   Web API   │ │
    │  │   (stdio)   │ │ (REST/HTTP) │ │
    │  └─────────────┘ └─────────────┘ │
    └──────────────┬───────────────────┘
                   │
    ┌──────────────▼───────────────────┐
    │         Storage Layer            │
    │ ┌─────────┐ ┌─────────────────┐  │
    │ │ Qdrant  │ │   PostgreSQL    │  │
    │ │(Vector) │ │ (Graph/Metadata)│  │
    │ └─────────┘ └─────────────────┘  │
    └──────────────────────────────────┘
```

### Memory Structure (Based on MCP Memory Server)

**Entities**: People, places, concepts, projects
```json
{
  "name": "project_alpha",
  "entityType": "project",
  "observations": [
    "deadline is March 2025",
    "budget is $50k", 
    "client seems stressed about timeline"
  ]
}
```

**Relations**: Connections between entities
```json
{
  "from": "john_smith", 
  "to": "project_alpha",
  "relationType": "works_on"
}
```

**Observations**: Facts with emotional context
```json
{
  "entityName": "john_smith",
  "content": "prefers morning meetings",
  "emotionalContext": "neutral",
  "category": "work"
}
```

## 🚀 Quick Start

### 1. Clone and Configure

```bash
git clone https://github.com/MichaelSchmidle/memrok.git
cd memrok
cp .env.example .env
```

### 2. Configure Environment

```bash
# .env
DOMAIN=memrok.yourdomain.com
JWT_SECRET=your-secure-random-string-here
POSTGRES_PASSWORD=secure-database-password
```

### 3. Deploy

```bash
# Start all services
docker-compose up -d

# Check status
docker-compose ps

# View logs
docker-compose logs -f
```

### 4. Get MCP Configuration

Visit your web interface at `http://localhost:3000` (or your domain) to get copy-paste MCP configurations for your clients.

## 🔧 Client Setup

The web interface will generate client-specific configurations you can copy and paste.

## 🛠️ Development

### Tech Stack

**Backend**
- **Runtime**: Bun
- **Framework**: Nitro (universal server framework)
- **Database**: PostgreSQL + Qdrant (vector)
- **Authentication**: JWT

**Frontend**
- **Framework**: Nuxt 3
- **UI**: Nuxt UI
- **State**: Pinia
- **Testing**: Vitest

**Infrastructure**
- **Containerization**: Docker + Docker Compose
- **Reverse Proxy**: Compatible with nginx, Traefik, Caddy

### Local Development

```bash
# Install dependencies
bun install

# Start development services (includes reverse proxy)
docker-compose -f docker-compose.dev.yml up -d

# Run frontend
bun dev

# Run backend
bun dev:api

# Run tests
bun test
```

## 🔒 Security & Privacy

### Data Flow Transparency

memrok is designed with complete transparency about data flow:

1. **Data Storage**: All memories stored locally on your infrastructure
2. **Data Processing**: Embedding generation and search happen locally  
3. **Memory Structure**: Knowledge graph with entities, relations, and emotional observations
4. **Category-based Sharing**: Control which AI assistants can access which memory categories
5. **No External Dependencies**: Core functionality works entirely offline

Example: Configure LM Studio to access all categories, but Claude Desktop only "work" and "general" categories.

### Security Features

- API key authentication
- Category-based access control for different AI clients
- Emotional context tracking for sensitive memories
- Audit logs for all memory access and modifications

## 📋 MVP Roadmap

### Core Features
- [ ] Knowledge graph memory structure (entities, relations, observations)
- [ ] Emotional context tracking in observations
- [ ] MCP server with memory tools (create_entities, add_observations, search_memories)
- [ ] Web interface for memory management and visualization
- [ ] Category-based access control
- [ ] Semantic search with Qdrant
- [ ] Docker deployment with reverse proxy support
- [ ] Client configuration generator

### Memory Tools (MCP)
- `create_entities` - Create people, places, concepts
- `create_relations` - Connect entities with relationships  
- `add_observations` - Store facts with emotional context
- `search_memories` - Semantic search across knowledge graph
- `open_memories` - Retrieve specific entity clusters

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## 📄 License

MIT License - see [LICENSE.md](LICENSE.md) for details.
