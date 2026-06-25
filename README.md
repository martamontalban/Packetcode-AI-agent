 # PacketCode

  An LLM-based AI agent designed to assist telecommunications engineers by
  automating daily tasks through natural language interaction. Built as a CLI
  and web application that connects to Large Language Models via a secure
  internal gateway, enabling engineers to execute commands, analyse logs,
  navigate web portals, manage configurations, and schedule meetings — all from
  a single interface.

  > ⚠️ This repository contains project documentation only. The source code is
  proprietary and hosted on internal infrastructure.

  ## The Problem

  Engineers in Packet Core departments handle complex daily operations:
  accessing remote servers, analysing logs, navigating internal tools,
  consulting technical documentation, and coordinating with team members. These
  tasks require constant context switching between multiple applications and
  often involve repetitive manual procedures.

  ## The Solution

  PacketCode provides an intelligent assistant that understands natural
  language requests and executes multi-step tasks autonomously. Unlike a simple
  chatbot, it operates as an **agent** — capable of reasoning about a task,
  deciding which tools to use, executing them, and iterating until the task is
  complete.

  ## Architecture

  The system follows a client-server architecture where all logic runs locally
  on the user's machine:

  **User's Machine:**
  - **PacketCode** (Node.js / TypeScript) — the main application
  - **Agent Loop** — core reasoning cycle: send to LLM → receive response →
  execute tools → repeat
  - **Tools** (25+) — bash, read, write, SSH, SFTP, grep, glob, web fetch...
  - **MCP Client** — connects to external tool servers (e.g., browser
  automation via Playwright)

  **Remote Infrastructure:**
  - **Internal AI Gateway** — handles authentication, routing, and rate
  limiting
  - **GPU Server** — runs the LLM model and returns generated responses

  Communication between PacketCode and the Gateway is via HTTPS. All tool
  execution happens locally on the engineer's machine. The LLM never has direct
  access to the filesystem or network — it can only request actions through
  PacketCode's tools.

  ## Memory System

  LLMs are stateless — they forget everything between requests. PacketCode
  solves this with three memory layers:

  | Layer | Scope | Purpose |
  |-------|-------|---------|
  | **Session Memory** | Single conversation | Stores full message history as
  JSON; saves incrementally |
  | **Context Compaction** | Within session | Automatically summarises older
  messages when context window fills up (triggered at 61% capacity) |
  | **Persistent Memory** | Across sessions | Markdown files that the agent
  reads and writes autonomously to remember project knowledge |

  ## Features

  - **CLI + Web Interface** — Terminal-based chat and browser-based UI with the
  same capabilities
  - **25+ Built-in Tools** — File operations, shell commands, web fetching,
  SSH, SFTP, task management
  - **Skills** — Reusable prompt templates that specialise the agent for
  specific tasks
  - **Multi-Agent Teams** — Sequential and supervisor workflows with multiple
  agent instances
  - **Automations** — AI-free scripts for deterministic, repeatable workflows
  - **Scheduled Tasks** — Cron-based task scheduling for recurring operations
  - **Calendar Integration** — View events, find availability, and create
  meetings via Outlook
  - **Remote Access (SSH)** — Execute commands and transfer files on remote
  servers
  - **Marketplace** — Share and install skills, automations, and team
  configurations
  - **Three Modes** — Work (execute), Plan (read-only analysis), Build
  (unrestricted creation)
  - **Model Selection** — Choose between multiple LLMs based on task complexity

  ## Tech Stack

  - **Language:** TypeScript
  - **Runtime:** Node.js
  - **CLI Framework:** Ink + React
  - **Web Server:** Express.js + WebSocket
  - **Browser Automation:** Playwright (Microsoft Edge)
  - **LLM Communication:** OpenAI SDK (compatible API)
  - **Tool Protocol:** Model Context Protocol (MCP)
  - **SSH:** ssh2
  - **Distribution:** npm package (internal registry)

  ## Use Cases

  1. **5G Expert Knowledge** — Skills built from CPI documentation enable the
  agent to answer technical questions about network node configuration, mapping
  3GPP standards to specific product implementations.

  2. **Remote Log Analysis** — Engineers describe an issue and the agent
  connects via SSH to remote servers, navigates log directories, identifies
  error patterns, and presents a summary.

  3. **Meeting Scheduling** — The agent queries multiple attendees' calendars,
  finds mutual availability, and creates the meeting invitation automatically.

  ## Academic Context

  This project was developed as part of a Master's thesis (TFM) for the
  **Master in Big Data Analytics** at Universidad Carlos III de Madrid (UC3M),
  2025-2026.

  **Author:** Marta María Montalbán Luquero
  **Advisor:** Francisco Javier García Blas

  ## License

  This project is proprietary software. The documentation in this repository is
  shared for academic purposes under [CC BY-NC-ND
  4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/).
