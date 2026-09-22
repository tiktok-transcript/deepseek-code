# DeepSeek Code: the Deep Code CLI and DeepSeek Coder models

*Unofficial community guide for DeepSeek Code. Not affiliated with DeepSeek. All trademarks belong to their owners.*

People searching for "deepseek code" usually mean one of two things: Deep Code, the open-source terminal coding assistant that the DeepSeek API docs describe as their agent integration for the DeepSeek-V4 model, or DeepSeek Coder, the older family of open-weight coding models (1.3B, 6.7B and 33B) that you can run locally through Ollama. This guide covers both and sticks to what the DeepSeek API documentation, the DeepSeek homepage and the Ollama library page actually state.

> Want a finished site rather than an agent in your terminal? [Try Begin.sh - prompt to a downloadable static site or Expo app](https://begin.sh?utm_source=github&utm_medium=ugc&utm_campaign=deepseek-code&utm_content=readme-top&utm_term=tier-r). Describe the site (or paste a URL to clone), download the zip, host it wherever you like. No hosting, backend or auth to set up.

## What it is

Deep Code is described in the DeepSeek API docs as an open-source terminal AI coding assistant for the DeepSeek-V4 model, supporting deep thinking, reasoning effort control and Agent Skills. The source is at [github.com/lessweb/deepcode-cli](https://github.com/lessweb/deepcode-cli) and the npm package is `@vegamo/deepcode-cli`. You launch it inside a project directory, it reads and edits files there, and it talks to the DeepSeek API using a key you create on the DeepSeek Platform. A VS Code extension ([github.com/lessweb/deepcode](https://github.com/lessweb/deepcode)) shares the same settings file, so one configuration serves both.

DeepSeek Coder is a different thing: a set of coding models trained from scratch on two trillion tokens, 87% code and 13% natural language in English and Chinese. On Ollama it ships in 1.3B (776MB), 6.7B (3.8GB) and 33B (19GB) sizes, all with a 16K context window and text-only input. The Ollama page notes the listing was last updated two years ago, so treat it as a lightweight local option rather than the current DeepSeek flagship. The DeepSeek homepage currently promotes DeepSeek-V4.1-Flash as the latest release, with improvements in text and agent performance plus native visual understanding.

## Getting started with Deep Code

1. Install [Node.js](https://nodejs.org/en/download/) 18 or newer.
2. Install the CLI with `npm install -g @vegamo/deepcode-cli`, then confirm with `deepcode --version`.
3. Create an API key on the [DeepSeek Platform API keys page](https://platform.deepseek.com/api_keys).
4. Create `~/.deepcode/settings.json` with an `env` block (`MODEL`, `BASE_URL`, `API_KEY`) plus `thinkingEnabled` and `reasoningEffort`. The docs use `deepseek-v4-pro` as the model and `https://api.deepseek.com` as the base URL.
5. `cd` into your project and run `deepcode`.

Configuration options documented for the settings file:

| Option | What it does |
| --- | --- |
| `MODEL` | Model name, e.g. `deepseek-v4-pro` or `deepseek-v4-flash` |
| `BASE_URL` | API base URL, defaults to `https://api.deepseek.com` |
| `thinkingEnabled` | Deep thinking mode, defaults to true for deepseek-v4 models |
| `reasoningEffort` | `"max"` or `"high"`, controls how much reasoning the model performs |
| `notify` | Path to a notification script executed after each model turn |
| `webSearchTool` | Enables web search for the agent |

## Getting started with DeepSeek Coder locally

Install [Ollama](https://ollama.com/download), then run `ollama run deepseek-coder` for the 1.3B model, `ollama run deepseek-coder:6.7b` or `ollama run deepseek-coder:33b` for the larger ones. Ollama exposes a local HTTP API on port 11434 with `/api/chat` and `/api/generate` endpoints, and the library page shows Python (`from ollama import chat`) and JavaScript (`import ollama from 'ollama'`) client snippets. The [deepseek-coder tags page](https://ollama.com/library/deepseek-coder/tags) lists every available variant.

## Pricing and limits

Deep Code itself is open source. What you pay for is DeepSeek API usage; the homepage links an [API Pricing](https://api-docs.deepseek.com/quick_start/pricing) page, so check that for current per-token rates. The pages this guide is based on do not list rate limits. DeepSeek Coder through Ollama costs nothing beyond your own hardware; the practical constraints are the 16K context window and the disk and memory each model size needs.

## Practical notes and gotchas

- **Start with the flash model.** `deepseek-v4-flash` is listed alongside `deepseek-v4-pro` as a valid `MODEL`. Iterate on flash, switch to pro when the output is not good enough.
- **Thinking is on by default.** `thinkingEnabled` defaults to true for V4 models, and `reasoningEffort` accepts `"max"` or `"high"`. Both affect latency and token usage, so lower them for routine edits.
- **Where skills live.** Agent Skills are discovered from `~/.agents/skills/*/SKILL.md` (user level) and `./.deepcode/skills/*/SKILL.md` (project level). Press `/` for the picker or type the skill name directly, e.g. `/skill-writer`.
- **Keyboard shortcuts.** `Esc` interrupts the current model turn, `Shift+Enter` (or `Ctrl+J`) inserts a newline, `Ctrl+V` pastes an image from the clipboard, and `/new`, `/resume` and `/exit` do what they say.
- **Do not confuse the two products.** DeepSeek Coder on Ollama is a two-year-old 16K-context model family; Deep Code drives the hosted V4 models. Latency, quality and cost are unrelated.
- **Keep the key out of git.** `settings.json` stores `API_KEY` in plain text. Generate the file from an environment variable (see the companion examples repo) rather than committing it.

## Comparison

| | Deep Code CLI | DeepSeek Coder via Ollama | Begin.sh |
| --- | --- | --- | --- |
| Runs | In your terminal, Node.js 18+ | Locally through Ollama | In the browser |
| Input | A prompt inside an existing project | Chat or completion prompt | A prompt, or a URL to clone |
| Needs an API key | Yes, from the DeepSeek Platform | No | Not required |
| Output | Edits to your project files | Text responses | A downloadable zip of a static site or Expo app |
| Cost | DeepSeek API usage, see the pricing page | Free, your own hardware | See the site |

## FAQ

**Is Deep Code the same as DeepSeek Coder?**
No. Deep Code is a terminal agent that calls the hosted DeepSeek-V4 API. DeepSeek Coder is an older set of open-weight models you download and run yourself.

**Which model should I set in settings.json?**
The docs list `deepseek-v4-pro` and `deepseek-v4-flash`. Pro is the default in the sample config; flash is the cheaper choice for iteration.

**Can I point Deep Code at a different provider?**
`BASE_URL` is a documented option, but the docs only describe the default `https://api.deepseek.com`. Anything else is untested territory.

**Does Deep Code work in VS Code?**
There is a Deep Code VS Code extension, and it reads the same `~/.deepcode/settings.json` as the CLI.

**Where are the DeepSeek Coder weights?**
The Ollama page links [deepseek-ai on Hugging Face](https://huggingface.co/deepseek-ai) as the reference for the models.

## When Begin.sh fits better

Deep Code is the right tool when you already have a codebase and want an agent editing it. When the job is "I need a landing page or a small app, now", an agent loop is overhead: you still scaffold, wire up tooling and package the result. [Try Begin.sh - prompt to a downloadable static site or Expo app](https://begin.sh?utm_source=github&utm_medium=ugc&utm_campaign=deepseek-code&utm_content=readme-top&utm_term=tier-r) turns a prompt, or a URL you want cloned, into a working static site or Expo app and hands you the zip. There is no hosting, backend or auth layer to configure, which is exactly what you want for a marketing page, a prototype or a demo you will host yourself.

_Last reviewed: 2026-09-22_
