# Security & Privacy Policy

## Supported Versions

Currently, Kimi CLI only provides security support for the latest version.

## Reporting a Vulnerability

Please report a vulnerability via the [MoonshotAI/kimi-cli - Security](https://github.com/MoonshotAI/kimi-cli/security) page, or open an [issue](https://github.com/MoonshotAI/kimi-cli/issues) if it can be published to public.

---

## Privacy Policy

This fork of Kimi CLI is **privacy-focused**. We have made significant changes to minimize data collection.

### What Data Is NOT Collected

The following telemetry has been **removed/anonymized**:

| Data Point | Original Behavior | Current Behavior |
|------------|------------------|------------------|
| Device Name | Your actual hostname sent to servers | `"anonymous"` |
| Device Model | OS version & architecture sent | `"unknown"` |
| OS Version | Full version string sent | `"unknown"` |
| Device ID | Persistent UUID tracked per device | Zeroed UUID (`00000000-...`) |
| User-Agent | Python/aiohttp versions leaked | Minimal `KimiCLI/<version>` |

### What Data Is Still Sent

| Data | Purpose | When Sent |
|------|---------|-----------|
| **IP Address** | Required for TCP connections | All API calls (use VPN if concerned) |
| **API Key** | Authentication with LLM providers | Only when making LLM requests |
| **Session ID** | LLM prompt caching (performance) | With Kimi API requests only |
| **Clipboard Data** | User-initiated paste operations | Only to LLM when you paste (Ctrl-V) |

### Data Collection by Feature

#### Using `/login` (Kimi Code OAuth)
- Sends **anonymized** device headers
- Session IDs used for prompt caching
- See [OAuth module](src/kimi_cli/auth/oauth.py)

#### Using Custom API Keys (OpenAI/Anthropic/etc)
- **No device telemetry** sent
- Only your API key and queries go to the provider
- Recommended for maximum privacy

#### Using Web Mode (`kimi web`)
- Local web server only (no external connections)
- Clipboard access is local-only

### How to Maximize Privacy

1. **Don't use `/login`** - Configure your own API provider instead:
   ```toml
   # ~/.kimi/config.toml
   [providers.openai]
   type = "openai_legacy"
   base_url = "https://api.openai.com/v1"
   api_key = "sk-..."
   ```

2. **Use a VPN** - Hides your IP address from servers

3. **Clear sessions** - Remove `~/.kimi/sessions/` periodically

4. **Review logs** - Check `~/.kimi/log/` for any unexpected outbound connections

### Local-Only Features

These features never send data externally:
- Shell command execution
- File operations (read/write/grep)
- Clipboard image/video paste (data goes to LLM only)
- Todo management
- Local session storage

### Third-Party Services

When using built-in tools, data may be sent to:
- **LLM Providers** (Kimi/OpenAI/Anthropic/Google): Your prompts and context
- **Search/Fetch Services** (if configured): Your search queries and URLs

We do not use:
- Analytics services (Google Analytics, Mixpanel, etc.)
- Error tracking (Sentry, etc.)
- Telemetry/tracking pixels

### Changes from Upstream

This fork differs from the original MoonshotAI/kimi-cli by:
1. Removing device fingerprinting from OAuth headers
2. Replacing default aiohttp User-Agent
3. Adding privacy documentation

See commit history for specific changes.

---

*Last updated: 2026-02-19*
