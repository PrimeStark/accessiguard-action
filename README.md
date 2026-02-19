# AccessiGuard GitHub Action

Automatically scan your website for WCAG accessibility issues on every pull request. 33 automated checks with AI-powered fix suggestions.

[![AccessiGuard](https://img.shields.io/badge/Powered%20by-AccessiGuard-blue)](https://accessiguard.app)

## Features

- **33 WCAG checks** — color contrast, ARIA, keyboard navigation, semantic HTML, and more
- **PR comments** — accessibility report posted directly on your pull request
- **Threshold gating** — fail builds when accessibility drops below your standard
- **Zero config** — works out of the box with any deployed URL
- **AI fix suggestions** — get code-level recommendations for every issue

## Quick Start

```yaml
# .github/workflows/accessibility.yml
name: Accessibility Check

on:
  pull_request:
    branches: [main]

jobs:
  accessibility:
    runs-on: ubuntu-latest
    steps:
      - uses: PrimeStark/accessiguard-action@v1
        with:
          url: 'https://your-site.com'
          threshold: 80
```

## Usage

### Basic — scan and report

```yaml
- uses: PrimeStark/accessiguard-action@v1
  with:
    url: 'https://your-site.com'
```

### With threshold — fail if score drops

```yaml
- uses: PrimeStark/accessiguard-action@v1
  with:
    url: 'https://staging.your-site.com'
    threshold: 80
    fail-on-threshold: true
```

### Multiple pages

```yaml
jobs:
  accessibility:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        page:
          - https://your-site.com
          - https://your-site.com/pricing
          - https://your-site.com/docs
    steps:
      - uses: PrimeStark/accessiguard-action@v1
        with:
          url: ${{ matrix.page }}
          threshold: 70
```

### With Vercel Preview Deployments

```yaml
name: Accessibility Check
on:
  deployment_status:

jobs:
  accessibility:
    if: github.event.deployment_status.state == 'success'
    runs-on: ubuntu-latest
    steps:
      - uses: PrimeStark/accessiguard-action@v1
        with:
          url: ${{ github.event.deployment_status.target_url }}
          threshold: 75
```

### Use outputs in later steps

```yaml
- uses: PrimeStark/accessiguard-action@v1
  id: a11y
  with:
    url: 'https://your-site.com'
    fail-on-threshold: false

- run: |
    echo "Score: ${{ steps.a11y.outputs.score }}"
    echo "Issues: ${{ steps.a11y.outputs.issues }}"
    echo "Passed: ${{ steps.a11y.outputs.passed }}"
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `url` | URL to scan | ✅ | — |
| `threshold` | Minimum score to pass (0-100) | ❌ | `0` |
| `fail-on-threshold` | Fail workflow if below threshold | ❌ | `true` |
| `comment` | Post results as PR comment | ❌ | `true` |
| `token` | GitHub token for PR comments | ❌ | `GITHUB_TOKEN` |

## Outputs

| Output | Description |
|--------|-------------|
| `score` | Accessibility score (0-100) |
| `issues` | Total number of issues found |
| `critical` | Number of critical issues |
| `passed` | Whether score met threshold (`true`/`false`) |

## What Gets Checked

AccessiGuard runs **33 automated WCAG checks** including:

- Color contrast (AA & AAA)
- Image alt text
- Form labels and associations
- Heading hierarchy
- ARIA attributes and roles
- Keyboard navigation
- Link and button accessibility
- Document language
- Meta viewport scaling
- Skip navigation links
- Table headers and scope
- Media captions
- Focus management
- Semantic HTML structure
- And more...

## PR Comment Example

The action posts a clean summary on your PR:

> ## ♿ AccessiGuard Accessibility Report
>
> | Metric | Value |
> |--------|-------|
> | **URL** | `https://your-site.com` |
> | **Score** | 🟡 **78**/100 |
> | **Issues Found** | 12 |
> | **Critical Issues** | 2 |
> | **Threshold** | 80 |
> | **Status** | ❌ Failed |

## CLI

Want to run scans locally?

```bash
npx accessiguard scan https://your-site.com
```

## Links

- [AccessiGuard Website](https://accessiguard.app)
- [CLI on npm](https://www.npmjs.com/package/accessiguard)
- [Report Issues](https://github.com/PrimeStark/accessiguard-action/issues)

## License

MIT
