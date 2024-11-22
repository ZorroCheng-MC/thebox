# AI Development Architecture - version 0.93

This document outlines the architecture of our AI development environment, which consists of three main layers: Client Layer, Routing Layer, and Provider Layer.

## Architecture Overview

### Client Layer
The client layer consists of several key components:

#### Development Tools
- **VSCode**: Primary code editor for development
- **Obsidian**: Markdown-based documentation and note-taking

#### Tools
- **OpenWebUI**: Web interface for AI interactions
- **n8n Automation**: Workflow automation platform

#### Extensions
- **Cline & Aider**: VSCode extension for AI assistance
- **Companion**: Obsidian extension for AI integration

#### n8n Nodes
- **Embedding Node**: Handles text embeddings
- **Chat Model Node**: Manages chat interactions

#### OpenWebUI Connections
- **Ollama**: Local model integration
- **OpenAI**: Direct OpenAI API access
- **OpenAI Compatible**: Compatible API endpoints
- **Claude**: Anthropic's Claude integration

### Routing Layer
- **LLM Router**: Central routing system (litellm) managing model traffic and distribution

### Provider Layer
- **OpenAI API**: Primary commercial API provider
- **Anthropic Claude**: Alternative commercial provider
- **GitHub Models**: Open-source model repository
- **Ollama Local Models**: Self-hosted model deployment

## Connection Flow
1. Client tools connect either directly to providers or through the routing layer
2. The routing layer (litellm) manages traffic distribution to appropriate providers
3. Provider layer offers both commercial and self-hosted options

## Architecture Diagram
<!-- Local Development Version -->

```mermaid
graph TD
%% Define subgraphs with clear spacing and alignment
subgraph Client_Layer["Client Layer"]
direction LR
%% IDE/Editor Group
subgraph Editors["Development Tools"]
vscode["VSCode (Code Editor)"]
obsidian["Obsidian (Markdown)"]
end
%% Tools Group
subgraph Tools["Tools"]
owui["OpenWebUI"]
n8n["n8n Automation"]
end
%% Extensions/Connections
subgraph Extensions["Extensions"]
cline["Cline & Aider"]
companion["Companion"]
end
%% n8n Nodes
subgraph N8N_Nodes["n8n Nodes"]
embedding["Embedding Node"]
chatmodel["Chat Model Node"]
end
%% OpenWebUI Connections
subgraph OWUI_Connections["OpenWebUI Connections"]
owui_ollama["Ollama"]
owui_openai1["OpenAI"]
owui_openai2["OpenAI Compatible"]
owui_claude["Claude"]
end
%% Connect components within Client Layer
vscode -->|Extension| cline
obsidian -->|Extension| companion
n8n -->|Node| embedding
n8n -->|Node| chatmodel
owui -->|Connection| owui_ollama
owui -->|Connection| owui_openai1
owui -->|Connection| owui_openai2
owui -->|Function| owui_claude
end

subgraph Routing_Layer["Routing Layer"]
direction LR
litellm["LLM Router"]
style litellm fill:#f9f,stroke:#333,stroke-width:2px
end

subgraph Provider_Layer["Provider Layer"]
direction LR
openai["OpenAI API"]
claude["Anthropic Claude"]
github["GitHub Models"]
ollama["Ollama Local Models"]
%% Style provider boxes
style ollama fill:#e1f5fe,stroke:#01579b
style github fill:#e8f5e9,stroke:#1b5e20
style openai fill:#fff3e0,stroke:#e65100
style claude fill:#f3e5f5,stroke:#4a148c
end

%% Connections between layers with clear routing
%% Client to Routing
embedding --> litellm
chatmodel --> litellm
cline --> litellm
owui_openai2 --> litellm

%% Direct connections to providers
cline --> claude
cline --> openai
cline --> ollama
owui_claude --> claude
owui_openai1 --> openai
owui_ollama --> ollama
companion --> ollama

%% Routing to Providers
litellm --> github
litellm --> ollama

%% Styling
classDef layerStyle fill:#ffffff,stroke:#333,stroke-width:3px
classDef default fill:#ffffff,stroke:#333,stroke-width:1px
class Client_Layer,Routing_Layer,Provider_Layer layerStyle

%% Spacing between layers
Client_Layer ~~~ Routing_Layer
Routing_Layer ~~~ Provider_Layer
```



## Key Benefits
1. **Flexibility**: Multiple AI providers and tools
2. **Integration**: Seamless connection between development tools
3. **Scalability**: Support for both local and cloud-based models
4. **Control**: Central routing for traffic management
5. **Extensibility**: Easy to add new tools and providers
