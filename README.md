# CrowdSec AI Bots Whitelist

A [CrowdSec](https://crowdsec.net) postoverflow whitelist collection that prevents legitimate AI crawlers from being banned.

## Why

CrowdSec's community blocklist and HTTP scenarios flag AI crawlers as attackers because they exhibit crawler-like behavior (high request rates, broad path scanning). This collection whitelists their official IP ranges so they pass through cleanly.

## Covered bots

| Bot | Operator | IP source | Update cadence |
|-----|----------|-----------|----------------|
| GPTBot | OpenAI | [openai.com/gptbot.json](https://openai.com/gptbot.json) | Daily (automated) |
| OAI-SearchBot | OpenAI | [openai.com/searchbot.json](https://openai.com/searchbot.json) | Daily (automated) |
| PerplexityBot | Perplexity AI | [perplexity.ai/perplexitybot.json](https://www.perplexity.ai/perplexitybot.json) | Daily (automated) |
| Applebot | Apple | [search.developer.apple.com/applebot.json](https://search.developer.apple.com/applebot.json) | Daily (automated) |
| YandexBot | Yandex | [yandex.com/ips](https://yandex.com/ips) | Manual (JS-rendered page) |

IP ranges in `data/` are refreshed daily by GitHub Actions and committed automatically.

## Installation

```bash
# Install the postoverflow
cscli postoverflows install trymultify/ai-bots-whitelist \
  --download-only \
  --config-dir /etc/crowdsec

# Or copy manually
cp postoverflows/s01-whitelist/trymultify/ai-bots-whitelist.yaml \
  /etc/crowdsec/postoverflows/s01-whitelist/

# Reload CrowdSec
systemctl reload crowdsec
# or in Docker:
docker compose restart crowdsec-agent
```

### Docker Compose (volume mount)

```yaml
crowdsec-agent:
  volumes:
    - ./crowdsec-ai-bots-whitelist/postoverflows:/etc/crowdsec/postoverflows/s01-whitelist/trymultify:ro
```

## Verify

```bash
cscli postoverflows list | grep ai-bots
```

## Contributing

PRs welcome — especially for bots with official IP sources that aren't covered yet.
