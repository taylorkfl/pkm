---
type: concept
repo: keyframe-live-avatar-demos
status: growing
lifecycle_stage: design
importance: high
last_reviewed: 2026-06-29
aliases:
  - VS Code Avatar Extension Demo
  - Coding Agent Avatar Demo
  - Developer Avatar Client
tags:
  - code-pkm
  - repo/keyframe-live-avatar-demos
  - pkm/type/concept
  - pkm/domain/product
---

# Developer IDE Avatar Extension Demo

## Objective
Demonstrate a Keyframe Labs live avatar integrated with developer tools, starting with VS Code, while evaluating whether open-source agent plugins are enough or whether a dedicated client is more efficient.

## Description
### What
The avatar appears inside the developer workflow as a coding companion. It can explain code, narrate an agent's plan, review diffs, ask clarifying questions, and hand tasks to a coding agent. The user can talk naturally while the avatar has access to selected files, diagnostics, terminal state, and agent session context.

### Why
This demo teaches a new production pattern: Keyframe avatars can become the human-facing layer on top of agentic developer tools. The avatar does not need to replace the coding agent. It can make the agent easier to supervise, easier to trust, and easier to use in long-running development sessions.

### When
Use this demo for developer-tooling customers, AI coding platforms, internal engineering copilots, onboarding, code review, pair programming, and enterprise teams evaluating agent supervision.

### How
Recommended architecture:

1. Build a VS Code extension as the primary client surface.
2. Render the Keyframe persona in a VS Code webview panel or webview view.
3. Use VS Code APIs for selected text, active file, workspace context, commands, diagnostics, and chat participant flows.
4. Keep Keyframe and LLM provider secrets server-side, issuing ephemeral session credentials to the extension.
5. Add agent adapters behind the extension: OpenCode first, MCP second, Odysseus third, Hermes AI after the exact project is identified.
6. Let the avatar explain and propose actions before executing anything that edits files, runs commands, or sends code to an external service.

### Feasibility Summary
| Target | Feasibility | Recommended Role | Reasoning |
| --- | --- | --- | --- |
| VS Code extension | High | Primary client | VS Code supports custom webviews, commands, chat participants, marketplace distribution, and deep workspace APIs. |
| OpenCode plugin | High for agent-side tools; medium for avatar UI | Adapter, not primary UI | OpenCode has plugins, custom tools, MCP support, an SDK, and a server API. It is excellent for routing agent actions, but the avatar UI should likely live in VS Code or a standalone client. |
| OpenCode SDK/server | High | Programmatic bridge | OpenCode exposes a headless server and type-safe SDK, making it a good backend agent adapter for a polished avatar client. |
| Odysseus | Medium | MCP/tool integration first; app contribution later | Odysseus advertises tools, MCP, local agents, memory, and a self-hosted UI. A first integration should be an MCP server or focused tool path; embedding the avatar deeply may require contributing to the app. |
| Hermes AI | Unknown | Research target | The exact Hermes AI agent project is ambiguous from public sources. Do not scope a plugin until the repository and extension surface are confirmed. |
| New dedicated client | High | Best long-term product surface | A dedicated client can normalize avatar UI, permissions, session state, and adapters across VS Code, OpenCode, Odysseus, and future agents. |

Current recommendation: build the VS Code extension first and design it as a client with pluggable agent adapters. Use OpenCode as the first adapter because its plugin, custom-tool, SDK, and server surfaces are explicit. Treat MCP as the portability layer for Odysseus and unknown future agents. Build a separate standalone client only if the VS Code proof shows the avatar interaction is valuable outside the IDE.

## Scope
| Area | MVP | Improvement |
| --- | --- | --- |
| IDE surface | VS Code webview with live avatar and start/stop controls | Sidebar view, panel view, command palette, inline actions, and chat participant |
| Workspace context | Active file, selected text, diagnostics, and file references | Full repo map, symbols, tests, diffs, terminal state, and issue context |
| Agent integration | OpenCode server or SDK adapter | OpenCode plugin, MCP server, Odysseus integration, Hermes adapter if feasible |
| Avatar role | Pair-programming explainer | Reviewer, onboarding guide, debugging partner, release captain |
| Permissions | User approval before file edits or command execution | Policy profiles, audit logs, enterprise controls, and safe mode |
| Artifacts | Plan summary and diff explanation | PR summary, test report, architecture note, and follow-up task list |

## Use Case
Use this demo when explaining how Keyframe avatars can make coding agents more understandable, supervised, and human-feeling inside an engineer's normal workflow.

## Stage In Lifecycle
Design. The evidence supports a VS Code-first prototype and an OpenCode-first adapter, while Hermes AI and deep Odysseus integration need additional repo-specific investigation.

## Importance
High. Developer tools are a natural place to show avatars as an interface layer for complex agent behavior, especially when customers need trust, visibility, and approvals.

## Risks
- VS Code webviews are powerful but resource-heavy and should be used only when the avatar adds enough value.
- Webview media behavior, microphone permissions, autoplay, and WebRTC behavior must be tested across VS Code and forks.
- An avatar that can trigger agent actions needs clear permission boundaries to avoid accidental edits, command execution, or secret exposure.
- OpenCode plugin hooks may help with context and tools but are not a full replacement for a polished visual client.
- Odysseus is AGPL-licensed and self-hosted, so distribution and derivative-work implications need review before embedding product code directly.
- Hermes AI is ambiguous without an exact repo, making confident plugin scoping premature.
- The avatar should not slow down fast coding workflows; it needs concise speech, interrupt controls, and silent mode.

## Input / Output
| Input | Output |
| --- | --- |
| Active file, selected code, diagnostics, workspace paths, and user prompt | Spoken explanation, suggested plan, or queued agent action |
| OpenCode session state, messages, todos, tools, or diff | Avatar narration and supervision UI |
| MCP or app-specific tool results | Grounded context for avatar responses |
| Agent edits, terminal output, and test results | Review summary, next steps, and user approval prompts |

## Resources
- [[Demo Roadmap]]
- [[Demo Tech Stack]]
- [[Shared Supporting Features]]
- [[Demo Questions]]
- [[Repositories/monorepo/Concepts/Per-Session Context|Per-Session Context]]
- [[Repositories/monorepo/Concepts/Voice Agent Details|Voice Agent Details]]
- [[Repositories/monorepo/Workflows/Real-Time Persona Session|Real-Time Persona Session]]

External references:
- VS Code Webview API: https://code.visualstudio.com/api/extension-guides/webview
- VS Code Chat Participant API: https://code.visualstudio.com/api/extension-guides/ai/chat
- VS Code MCP developer guide: https://code.visualstudio.com/api/extension-guides/ai/mcp
- OpenCode docs: https://opencode.ai/docs/
- OpenCode plugins: https://opencode.ai/docs/plugins/
- OpenCode custom tools: https://opencode.ai/docs/custom-tools/
- OpenCode server: https://opencode.ai/docs/server/
- OpenCode SDK: https://opencode.ai/docs/sdk/
- Odysseus landing page: https://pewdiepie-archdaemon.github.io/odysseus/
- Odysseus GitHub repository: https://github.com/pewdiepie-archdaemon/odysseus
- Model Context Protocol intro: https://modelcontextprotocol.io/docs/getting-started/intro
- Keyframe JavaScript SDK reference: https://docs.keyframelabs.com/sdk-reference/javascript
- Keyframe self managed quickstart: https://docs.keyframelabs.com/guides/getting-started/quickstart-self-managed

## Related Topics
- [[Zoom Active Participant Avatar Demo]]
- [[Shared Supporting Features]]
- [[Repositories/monorepo/Technologies/TypeScript|TypeScript]]
- [[Repositories/monorepo/Technologies/React|React]]
- [[Repositories/monorepo/Technologies/WebRTC|WebRTC]]
- [[Repositories/monorepo/Technologies/WebSockets|WebSockets]]
- [[Repositories/monorepo/Technologies/LiveKit|LiveKit]]

## Evidence
- User-provided idea on 2026-06-29: integrate the AI avatar with VS Code as an extension and investigate plugins for open-source agents such as Hermes AI, OpenCode, and Odysseus.
- VS Code webviews can create fully customizable extension UI and communicate with the extension through message passing: https://code.visualstudio.com/api/extension-guides/webview
- VS Code Chat Participant API supports domain-specific chat participants, slash commands, follow-ups, and deep integration with VS Code APIs: https://code.visualstudio.com/api/extension-guides/ai/chat
- OpenCode describes itself as an open-source AI coding agent available as terminal, desktop, or IDE extension: https://opencode.ai/docs/
- OpenCode plugins can hook events, add tools, and load from local files or npm packages: https://opencode.ai/docs/plugins/
- OpenCode custom tools can be project-local or global and can invoke scripts in any language: https://opencode.ai/docs/custom-tools/
- OpenCode exposes a server and SDK for programmatic control: https://opencode.ai/docs/server/ and https://opencode.ai/docs/sdk/
- Odysseus describes built-in tools, MCP, agents, memory, and a self-hosted local-first workspace: https://pewdiepie-archdaemon.github.io/odysseus/
- Odysseus repository features include chat and agents, tools, MCP, files, shell, skills, memory, and local/API model workflows: https://github.com/pewdiepie-archdaemon/odysseus
- MCP is an open standard for connecting AI applications to external systems and has broad support across clients including development tools: https://modelcontextprotocol.io/docs/getting-started/intro
- Keyframe JavaScript docs describe the low-level SDK, framework-agnostic Elements, hosted `PersonaEmbed`, self managed `PersonaView`, and React bindings: https://docs.keyframelabs.com/sdk-reference/javascript
