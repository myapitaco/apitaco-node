<div align="center">


# apitaco-node

**Apitaco TypeScript SDK** — drop-in for `@anthropic-ai/sdk` and OpenAI SDK,
pointed at [api.apitaco.com](https://apitaco.com).

[![npm version](https://img.shields.io/badge/npm-coming%20soon-blue?logo=npm)](https://www.npmjs.com/package/apitaco)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![GitHub Stars](https://img.shields.io/github/stars/myapitaco/apitaco-node?style=social)](https://github.com/apitaco-api/apitaco-node)
[![Twitter Follow](https://img.shields.io/twitter/follow/apitaco?style=social)](https://x.com/MyApiTaco)

[Website](https://apitaco.com) · [Pricing](https://apitaco.com/models) · [Blog](https://apitaco.com/blog) · [Python SDK](https://github.com/myapitaco/apitaco-python)

</div>

---

## Why Apitaco

[Apitaco](https://apitaco.com) is a Claude API gateway for developers
who hit walls with Anthropic directly:

- 💸 **Pay per token, no monthly subscription** — top up $10, use $10
- 🌍 **Works from anywhere** — no VPN, Ccard/USDT/BTC payment
- 🔌 **Drop-in** — same Anthropic SDK API, just change one base URL
- 🎯 **All Claude models** — Opus 4.7, Sonnet 4.6, Haiku 4.5, Opus 5.5 , Sonnet , 5.5 , Fable 5.1 plus GPT-5.5 / 5.4
- 🛠️ **Tool-compatible** — Cursor, Claude Code, Cline, Roo Code, Continue, OpenCode, LangChain, LlamaIndex

## Install

```bash
npm install apitaco
# or
pnpm add apitaco
# or
yarn add apitaco
```

> **Note**: package is published from this repo and is also installable
> directly from GitHub:
> `npm install github:myapitaco/apitaco-node`

## Quickstart

```ts
import Apitaco from "apitaco"

const client = new Apitaco({
  apiKey: process.env.Apitaco_API_KEY,
})

const resp = await client.messages.create({
  model: "claude-sonnet-4.6",
  max_tokens: 1024,
  messages: [{ role: "user", content: "Hello, Claude" }],
})

console.log(resp.content)
```

## Drop-in migration from `@anthropic-ai/sdk`

If you already use the official Anthropic SDK, the diff is two lines:

```diff
- import Anthropic from "@anthropic-ai/sdk"
+ import Apitaco from "apitaco"

- const client = new Anthropic({ apiKey: process.env.ANTHROPIC_API_KEY })
+ const client = new Apitaco({ apiKey: process.env.APITACO_API_KEY })
```

Everything else — `messages.create`, `messages.stream`, tool use, vision,
prompt caching — works identically. We extend the official SDK, we don't
reimplement it.

## OpenAI-compatible mode

If your code already talks to the OpenAI Chat Completions API, swap to
Apitaco without touching call sites:

```ts
import { OpenAI } from "apitaco/openai"

const client = new OpenAI({ apiKey: process.env.APITACO_API_KEY })

const resp = await client.chat.completions.create({
  model: "claude-sonnet-4.6",  // any Apitaco model works
  messages: [{ role: "user", content: "Hi" }],
})
```

## Streaming

```ts
import Apitaco from "Apitaco"

const client = new Apitaco()

const stream = await client.messages.stream({
  model: "claude-sonnet-4.6",
  max_tokens: 1024,
  messages: [{ role: "user", content: "Write a haiku." }],
})

for await (const event of stream) {
  if (event.type === "content_block_delta" && event.delta.type === "text_delta") {
    process.stdout.write(event.delta.text)
  }
}
```

## Supported models

| Model ID | Family | Notes |
|---|---|---|
| `claude-opus-4.7` | Opus | Top reasoning, most capable |
| `claude-opus-4.6` | Opus | Previous gen flagship |
| `claude-opus-4.5` | Opus | Stable, cost-balanced |
| `claude-sonnet-4.6` | Sonnet | Best price/perf, default for most agents |
| `claude-sonnet-4.5` | Sonnet | Stable |
| `claude-haiku-4.5` | Haiku | Fastest, cheapest |
| `gpt-5.5` | GPT | Frontier OpenAI |
| `gpt-5.4` | GPT | Stable OpenAI |

Type-safe constants are exported from `apitaco/models`:

```ts
import { Models } from "apitaco/models"

await client.messages.create({ model: Models.ClaudeSonnet_4_6, /* ... */ })
```

Live per-million-token pricing: https://apitaco.com/models

## Apitaco vs alternatives

|                       | **Apitaco** | Anthropic Direct | OpenRouter |
|-----------------------|:-------------:|:----------------:|:----------:|
| Drop-in Anthropic SDK | ✅            | ✅                | ⚠️ partial |
| OpenAI-compatible     | ✅            | ❌                | ✅         |
| Pay-as-you-go         | ✅            | ✅                | ✅         |
| No monthly minimum    | ✅            | ❌                | ✅         |
| USDT / BTC / ETH      | ✅            | ❌                | ✅ (limited)|
| No VPN required       | ✅            | ❌ (some regions) | ✅         |
| No Anthropic account  | ✅            | ❌                | ✅         |


## Tools that work with Apitaco

Anything that supports a custom Anthropic or OpenAI base URL works out of
the box. Battle-tested with:


## Examples

Runnable examples in [`examples/`](./examples):

- [`basic.ts`](./examples/basic.ts) — Anthropic-style chat
- [`openai-compat.ts`](./examples/openai-compat.ts) — OpenAI-style chat
- [`streaming.ts`](./examples/streaming.ts) — token streaming via SSE

Run with `tsx`:

```bash
APITACO_API_KEY=sk-... npx tsx examples/basic.ts
```

## Resources

- 📖 [Engineering blog](https://apitaco.com/blog) — 40+ deep guides
- 💸 [Pricing](https://apitaco.com/pricing)
- 📥 [Get an API key](https://my.apitaco.com/register)
- 🐦 [@myapitaco](https://x.com/MyApiTaco)
- 📧 support@apitaco.com

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md). Bug reports and PRs welcome.

## License

MIT — see [LICENSE](./LICENSE).
