# Delimit GitHub Action

Detect breaking API changes in pull requests. Zero configuration required.

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Delimit-blue)](https://github.com/marketplace/actions/delimit-api-governance)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

## Quick Start

Add to `.github/workflows/api-check.yml`:

```yaml
name: API Contract Check
on: pull_request

jobs:
  delimit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: delimit-ai/delimit-action@v1
```

Zero configuration. Delimit auto-detects your OpenAPI/Swagger specs.

## What It Detects

### Breaking Changes (Fails CI)

- Endpoint removed
- Required field removed from response
- Field type changed (e.g., string to number)
- Required parameter added to existing endpoint
- Enum value removed
- HTTP method removed

### Safe Changes (Passes)

- New endpoints added
- Optional fields added
- Enum values added
- Documentation changes

## Configuration

```yaml
- uses: delimit-ai/delimit-action@v1
  with:
    # Advisory mode: comments only, won't fail CI
    mode: advisory

    # Or specify specs manually (auto-detected if omitted)
    old_spec: api/openapi.yaml
    new_spec: api/openapi.yaml
```

### Custom Policies

Create `.delimit/policies.yml` in your repo:

```yaml
rules:
  - id: no_endpoint_removal
    change_types: [endpoint_removed]
    severity: error
    message: "Endpoints cannot be removed without deprecation"
```

## Supported Formats

- OpenAPI 3.0, 3.1
- Swagger 2.0
- JSON and YAML
- Multiple specs in monorepos

## First Run Behavior

Delimit never fails on first run. It establishes a baseline and starts checking on subsequent PRs.

```
Auto-detected: api/openapi.yaml
Baseline established
Running in advisory mode (non-blocking)
```

## CLI

For local development and pre-commit checks, use the [Delimit CLI](https://github.com/delimit-ai/delimit):

```bash
npm install -g delimit
```

## Links

- [Delimit CLI](https://github.com/delimit-ai/delimit) — Local development tool
- [Website](https://delimit.ai) — Documentation
- [Issues](https://github.com/delimit-ai/delimit-action/issues) — Bug reports

## License

MIT
