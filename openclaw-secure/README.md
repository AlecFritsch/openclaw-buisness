# OpenClaw Secure

Hardened OpenClaw Docker image for OpenClaw Business. Used as the base for agent containers.

## Security Fixes (in OpenClaw fork)

All security fixes are now implemented natively in the **OpenClaw fork** (clone separately, pass `OPENCLAW_FORK_URL` at build). See `security-patches/README.md` for details.

| CVE / Fix | Implementation |
|-----------|----------------|
| **CVE-2026-25253** (CVSS 9.1) | Gateway URL allowlisting |
| **CVE-2026-24763** (CVSS 8.8) | PATH sanitization (`ENV PATH=...`) |
| **CVE-2026-25157** (CVSS 8.1) | SSH mode disabled |
| **device-local** | 172.16–31 as local when `OPENCLAW_GATEWAY_BIND=lan` |
| **pairing-silent** | Auto-approve for not-paired / scope-upgrade |
| **lan-ws** | ws:// to RFC1918 allowed |

## Bundled Plugins

| Plugin | Path | Description |
|--------|------|-------------|
| **superchat** | `/opt/superchat` | Superchat API (WhatsApp, Email, etc.) |
| **knowledge** | `/opt/knowledge` | RAG search via platform backend |
| **mcp-connect** | `/opt/mcp-connect` | Smithery Connect tools (Intercom, Slack, etc.) |

Plugins require `PLATFORM_BACKEND_URL` and `PLATFORM_AGENT_ID` (or legacy `HAVOC_BACKEND_URL` / `HAVOC_AGENT_ID`).

## Build

```bash
chmod +x build.sh
OPENCLAW_FORK_URL=https://github.com/YOUR_ORG/openclaw-saas-fork.git ./build.sh
```

`OPENCLAW_FORK_URL` is required — the fork must include SaaS/Docker changes (device-local, pairing-silent, etc.).

## Usage

```bash
docker run -d \
  --name openclaw-agent \
  -p 18789:18789 \
  -v /path/to/workspace:/home/node/.openclaw \
  -e ANTHROPIC_API_KEY=sk-ant-... \
  -e PLATFORM_BACKEND_URL=http://host.docker.internal:8080 \
  -e PLATFORM_AGENT_ID=your-agent-id \
  openclaw-secure:latest
```

## Additional Hardening

- Non-root user execution
- Read-only filesystem
- Capability dropping (--cap-drop=ALL)
- Resource limits (2GB RAM, 1 CPU)
- Network isolation
- Tmpfs for /tmp (noexec, nosuid)

## License

MIT
