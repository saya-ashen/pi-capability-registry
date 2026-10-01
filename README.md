# Pi Capability Registry

A small, experimental public registry for capability-driven Minimal Pi applications.
GitHub Raw serves static files; no registry server, installation hook or GitHub authentication is required.

## Additional Python release

The independent [python-v1 release](releases/python-v1/README.md) adds python-kit and its shell-kit preparation dependency for registry-only clients. It does not alter the original index or invalidate existing text-only trust configurations.

A separate opt-in [managed Python release](releases/python-managed-v1/README.md) provides a pinned private runtime when a matching host Python is unavailable. It requires a new, explicit host-artifact download permission and does not change existing authorizations.

## Original index package

| Package | Capability | Tool |
|---|---|---|
| `remote-text@0.1.0` | `text.statistics` | `text_statistics` |

Counts Unicode code points and whitespace-delimited nonempty segments. For `hello 世界`, the result is `{ "codePoints": 8, "words": 2 }`.
This is not grapheme counting, tokenizer output or language-aware word segmentation.

The complete package source is readable inside the JSON bundle's `files` entries. It uses only Node built-ins and creates/removes a task-local preparation marker.

## Distribution

- Index: https://raw.githubusercontent.com/saya-ashen/pi-capability-registry/main/registry/index.json
- Index SHA-256 for this initial publication: `6485d35439ea3d0df0cf71cc8e96a688b0fbb9433a08ad21bff5fbd4eea0f726`
- Package SHA-256: `372cb2fc800be90a1312c7dceaa92554625716b8bca209c314558c3353dff254`

An index contains full package manifests, absolute same-origin bundle URLs and bundle SHA-256 values.
A bundle contains `{ "schemaVersion": 1, "files": [{ "path": "...", "content": "..." }] }`.
JSON bytes must not be reformatted after hashing. `.gitattributes` disables Git line-ending normalization for registry JSON.

## Trust is local

Clients pin the index hash and explicitly allow exact package versions and bundle hashes in a LOCAL configuration. Discovering a package does not authorize it.
Downloads are verified before the existing `check → prepare → check` lifecycle. Only `ready` capabilities expose tools.

For a compatible Minimal Pi client, an opt-in local configuration for this reviewed example is:

```json
{
  "schemaVersion": 1,
  "indexUrl": "https://raw.githubusercontent.com/saya-ashen/pi-capability-registry/main/registry/index.json",
  "indexSha256": "6485d35439ea3d0df0cf71cc8e96a688b0fbb9433a08ad21bff5fbd4eea0f726",
  "cacheDir": "./cache",
  "allow": [
    {
      "package": "remote-text@0.1.0",
      "sha256": "372cb2fc800be90a1312c7dceaa92554625716b8bca209c314558c3353dff254"
    }
  ]
}
```

Review the source before opting in; use `allow: []` for discovery without execution authorization.
Do not commit local trust files, caches, credentials or `.env` files to this repository.

## Boundaries

- Experimental protocol v1; the original text-only index and an additive Python dependency release, not a general marketplace.
- HTTPS, fixed hashes, no redirects; no package signatures, auto-updates or private-registry authentication.
- Hashes provide integrity, not proof that code is safe. Packages are NOT sandboxed.
- `main` may change. A changed index requires deliberate local pin updates; old pins must fail rather than silently accept new code.
- Keep old content-addressed bundles when adding versions. This repository does not run an automated publishing workflow.
