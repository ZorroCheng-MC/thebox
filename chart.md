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
