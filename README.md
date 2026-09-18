# hermes-labs-ai/copilot-plugins

Compatibility feed for existing GitHub Copilot plugin installations.

The canonical catalog is
[`hermes-labs-ai/plugins`](https://github.com/hermes-labs-ai/plugins). This
repository keeps the historical `hermes-labs-copilot` marketplace namespace
and install route stable; it does not own plugin metadata or product code.

`.claude-plugin/marketplace.json` is generated from the canonical
`plugins/catalog.json` with:

```bash
node path/to/plugins/scripts/generate.mjs \
  --only-compat \
  --compat-output .claude-plugin/marketplace.json
```

The verification workflow checks that the committed feed is byte-for-byte
identical to a fresh build from the canonical catalog. Do not edit the feed
by hand.

## Existing install route

```bash
copilot plugin marketplace add hermes-labs-ai/copilot-plugins
copilot plugin install hermes-blind@hermes-labs-copilot
```

New installations may use `hermes-labs-ai/plugins` directly; both routes keep
the `hermes-labs-copilot` marketplace identifier.

## Scope

Plugin implementations, releases, skills, MCP servers, and hooks remain in
the product repositories referenced by each entry. Capability and host
compatibility claims are maintained only in the canonical catalog.
