# Wati MCP: changes needed for Smithery publishing (OAuth)

## Summary

`smithery mcp publish` fails against all three Wati MCP servers because the 401 response carries a **relative** `resource_metadata` URL in its `WWW-Authenticate` header. The OAuth spec (RFC 9728) requires an absolute URL. Smithery's CLI is strict and rejects it. Fixing the header is the one required change.

Error seen:

```json
{
  "error": "auth_required",
  "message": "OAuth discovery failed: Failed to parse resource challenge for 'https://mcp.wati.io/mcp': Invalid resource metadata '/.well-known/oauth-protected-resource'"
}
```

## Required change

Return an absolute URL in the `WWW-Authenticate` header of the 401 response from `POST /mcp`.

| | Value |
|---|---|
| Current | `Bearer resource_metadata="/.well-known/oauth-protected-resource"` |
| Needed | `Bearer resource_metadata="https://mcp.wati.io/.well-known/oauth-protected-resource"` |

Apply to every region, using each host's own origin:

| Server | Expected header value |
|---|---|
| global | `https://mcp.wati.io/.well-known/oauth-protected-resource` |
| EU | `https://eu-mcp.wati.io/.well-known/oauth-protected-resource` |
| KSA | `https://ksa-mcp.wati.io/.well-known/oauth-protected-resource` |

All three currently send the relative form (checked 2026-10-16).

Build the URL from the configured public base URL, not from the request `Host` header, so it stays correct behind Cloudflare and the load balancer.

## Likely follow-ups (not confirmed as blockers)

These only matter if Smithery still fails after the header fix.

1. **`resource` value in the protected-resource metadata.** It currently returns `"resource": "https://mcp.wati.io"` (no `/mcp`). RFC 9728 says `resource` should match the resource the client is accessing, here `https://mcp.wati.io/mcp`. A strict client may reject the mismatch. Changing it to include `/mcp` is the spec-correct value.
2. **Path-suffixed metadata URL.** `/.well-known/oauth-protected-resource/mcp` returns 404. Some clients try the path-suffixed form first. Serving it (same JSON) is cheap insurance.
3. **Client registration.** Smithery's docs say it registers via Client ID Metadata Documents (CIMD). The authorization-server metadata does not advertise CIMD support (`client_id_metadata_document_supported`). It does advertise dynamic client registration (`registration_endpoint`: `https://mcp.wati.io/register`), so Smithery may fall back to that. Not verified. If registration fails, supporting CIMD is the fix.

## Already fine

- `/.well-known/oauth-protected-resource` returns valid JSON on all three hosts, with `authorization_servers` pointing at the matching host.
- `/.well-known/oauth-authorization-server` is present: authorization code grant, PKCE `S256`, refresh tokens, `/authorize`, `/token`, `/register`.

## How to verify

```bash
curl -si -X POST https://mcp.wati.io/mcp \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json, text/event-stream' \
  -d '{}' | grep -i '^www-authenticate'
```

Expect the absolute URL in `resource_metadata`. Repeat for `eu-mcp` and `ksa-mcp`. Then run:

```bash
npx -y @smithery/cli@latest mcp publish "https://mcp.wati.io/mcp" -n wati-io/wati-mcp
```

The first publish will pause for a person to authorize with Wati in a browser. Finish it with `--resume`.

## Where the change lives

Not in this repo (`wati-mcp` holds docs and registry config only). The server code is most likely `astra-insight-mcp`. Search it for `resource_metadata` or `oauth-protected-resource`.
