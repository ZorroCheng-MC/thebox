# AI Development Architecture - version 0.95

This document outlines the architecture of our AI development environment, which consists of four main layers: User Layer, Interface Layer, Routing Layer, and Provider Layer.

## Architecture Overview

### User Layer
The user layer is the interface which end-user interfacing with the system. This includes tools such as thebox.hkmci.net, VSCode, Obsidian, n8n workflows, and Audiobot UI.

#### Development Tools
- **VSCode**: Primary code editor for development
- **Obsidian**: Markdown-based documentation and note-taking

#### Web Chat UI
- **Chat UI**: User interface for chat interactions
- **Audiobot UI**: User interface for audio interactions

#### Workflow Management Tools
- **n8n Automation**: Workflow automation platform

### Interface Layer
The interface layers are the extensions, plugins, and AI API call interfaces of the users' tools to communicate with different AI services.

#### Extensions
- **Cline & Aider**: VSCode extension for AI assistance
- **Companion**: Obsidian extension for AI integration

#### n8n Nodes
- **Embedding Node**: Handles text embeddings
- **Chat Model Node**: Manages chat interactions

#### Connections
- **Cloudflare Tunnel**: Secure connection for web interfaces
- **Zoombot**: Interface for Zoom interactions

### Routing Layer
The routing layer handles the API calls from the interface layer to the different AI service providers. It ensures all tools can communicate with different providers by translating API calls and managing decentralized API keys.

- **LLM Router**: Central routing system (litellm) managing model traffic and distribution. Currently handles Azure GitHub model translation for free OpenAI API calls.
- **OpenWebUI**: Supports individual API key management for end-users, allowing access to all company AI APIs.

### Provider Layer
- **OpenAI**: Provides GPT models
- **Anthropic Claude**: Provides Sonet and Haiku models
- **GitHub Models**: Azure service providing free OpenAI GPT4o services
- **Ollama Local Models**: Self-hosted models like Llama, Qwen, and Mistral
- **Whisper AI**: Provides audio processing capabilities

## Connection Flow
1. User tools connect through the interface layer to the routing layer
2. The routing layer provides API services to the available providers
3. Provider layer offers both commercial and self-hosted options

## Architecture Diagram
<!-- Local Development Version -->

```mermaid
graph TD
%% Define subgraphs with clear spacing and alignment

subgraph User_Layer["User Layer"]
direction LR
chat_ui["Chat UI"]
audiobot_ui["Audiobot UI"]
vscode["VSCode (Code/Doc Editor)"]
obsidian["Obsidian (Doc Editor)"]
auditobot["Auditobot"]
end

subgraph Interface_Layer["Interface Layer"]
direction LR
cline["Cline & Aider"]
companion["Companion"]
cloudflare_tunnel["Cloudflare Tunnel"]
zoombot["Zoombot"]
n8n["n8n Automation"]
end

chat_ui --> cloudflare_tunnel
cloudflare_tunnel --> owui

%% Connect components
vscode -->|Extension| cline
obsidian -->|Extension| companion
auditobot --> n8n
n8n --> litellm
cline --> owui
audiobot_ui --> whisper
zoombot --> whisper

subgraph Routing_Layer["Routing Layer"]
direction LR
litellm["LLM Router"]
owui["OpenWebUI"]
style litellm fill:#f9f,stroke:#333,stroke-width:2px
style owui fill:#f0f8ff,stroke:#4682b4,stroke-width:2px
end

subgraph Provider_Layer["Provider Layer"]
direction LR
openai["OpenAI API"]
claude["Anthropic Claude"]
github["GitHub Models"]
ollama["Ollama Local Models"]
whisper["Whisper AI"]
%% Style provider boxes
style ollama fill:#e1f5fe,stroke:#01579b
style github fill:#e8f5e9,stroke:#1b5e20
style openai fill:#fff3e0,stroke:#e65100
style claude fill:#f3e5f5,stroke:#4a148c
style whisper fill:#f5f5dc,stroke:#8b4513
end

%% Connections between layers with clear routing
%% Client to Routing
cline --> litellm

%% Direct connections to providers
cline --> claude
cline --> openai
cline --> ollama
companion --> ollama
companion --> openai

%% OpenWebUI direct connections to providers
owui --> openai
owui --> claude
owui --> github
owui --> ollama

%% Routing to Providers
litellm --> github
litellm --> ollama

%% Styling
classDef layerStyle fill:#ffffff,stroke:#333,stroke-width:3px
classDef default fill:#ffffff,stroke:#333,stroke-width:1px
class User_Layer,Interface_Layer,Routing_Layer,Provider_Layer layerStyle

%% Spacing between layers
User_Layer ~~~ Interface_Layer
Interface_Layer ~~~ Routing_Layer
Routing_Layer ~~~ Provider_Layer

```


## Key Benefits
1. **Flexibility**: Multiple AI providers and tools
2. **Integration**: Seamless connection between development tools
3. **Scalability**: Support for both local and cloud-based models
4. **Control**: Central routing for traffic management
5. **Extensibility**: Easy to add new tools and providers
