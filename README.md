# Bilinc Python SDK, CLI and MCP adapter

<!-- mcp-name: io.github.atakanelik34/bilinc -->

[![PyPI](https://img.shields.io/pypi/v/bilinc.svg)](https://pypi.org/project/bilinc/)
[![Python](https://img.shields.io/pypi/pyversions/bilinc.svg)](https://pypi.org/project/bilinc/)
[![License](https://img.shields.io/badge/license-BUSL--1.1-blue.svg)](LICENSE)

The public client for [Bilinc Cloud](https://bilinc.space), hosted memory for AI agents.
Save what your agents learn, recall it later, see who wrote each memory and what changed,
and roll back when something goes wrong.

This repository contains exactly what is published to PyPI as `bilinc`: the Python SDK,
the `bilinc` CLI and the stdio MCP adapter. The hosted service itself is not part of it.

## Install

```bash
pip install -U bilinc
bilinc login
bilinc quicktest
```

`bilinc login` opens your browser to sign in (free, no card required) and saves an API key
for this computer. On a machine without a browser, use `bilinc login --device`. For CI and
scripts, set `BILINC_API_KEY`.

Using Claude, ChatGPT or Codex? You can connect them directly, without Python:
https://bilinc.space/docs/mcp

## Python SDK

```python
from bilinc import CloudClient

client = CloudClient()  # reads BILINC_API_KEY or a key saved by `bilinc login`

written = client.commit("agent.goal", {"ship": "reliable memory"}, memory_type="semantic")
results = client.recall("agent goal", limit=5)

client.revise("agent.goal", {"ship": "verifiable memory"},
              reason="scope corrected", expected_version=written["entryVersion"])

snapshot = client.create_snapshot(label="before-autonomous-run")["snapshot"]
client.diff(snapshot["id"])

preview = client.rollback_preview(snapshot["id"], reason="undo bad agent run")
client.rollback(snapshot["id"], confirmation_token=preview["confirmationToken"],
                reason="undo bad agent run")
```

## CLI

```bash
bilinc status
bilinc commit --key agent.goal --value '{"ship":"reliable memory"}'
bilinc recall --query "agent goal"
bilinc list --prefix agent.
bilinc history agent.goal
bilinc snapshot create --label before-autonomous-run
bilinc export -o bilinc-export.json
```

## MCP (stdio)

```bash
python -m bilinc.cloud_mcp
```

Set `BILINC_API_KEY` in the MCP client's environment. Setup guides for each client:
https://bilinc.space/docs/mcp

## Links

- Website: https://bilinc.space
- Docs: https://bilinc.space/docs
- Pricing (free tier, no card): https://bilinc.space/pricing
- Changelog: https://bilinc.space/changelog
- PyPI: https://pypi.org/project/bilinc/

## License

Business Source License 1.1, licensed by ReARC Labs Inc. See [LICENSE](LICENSE).
