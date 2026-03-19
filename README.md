# ComplyForm GitHub Action

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-ComplyForm-blue?logo=github)](https://github.com/marketplace/actions/complyform-compliance-scan)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Latest Release](https://img.shields.io/github/v/release/complyform/complyform-action)](https://github.com/complyform/complyform-action/releases)

Scan your Terraform state for compliance gaps across 53 frameworks in your CI/CD pipeline.

## Quick Start

```yaml
- uses: complyform/complyform-action@v1
  with:
    cloud: gcp
    state-path: ./terraform.tfstate
```

## Full Example

Pro tier with license key, dashboard push, and framework filtering:

```yaml
- uses: complyform/complyform-action@v1
  with:
    cloud: gcp
    frameworks: soc2,hipaa
    state-path: ./terraform.tfstate
    output-format: json
    license-key: ${{ secrets.COMPLYFORM_LICENSE_KEY }}
    dashboard-push: true
    project-label: production
    api-key: ${{ secrets.COMPLYFORM_API_KEY }}
```

## Inputs

| Input | Required | Default | Description |
|---|---|---|---|
| `version` | No | `latest` | ComplyForm CLI version to install |
| `cloud` | **Yes** | — | Target cloud: `gcp`, `aws`, or `azure` |
| `frameworks` | No | `""` | Comma-separated framework IDs (e.g., `soc2,hipaa`) |
| `state-path` | No | `./terraform.tfstate` | Path to Terraform state file |
| `severity` | No | `all` | Minimum severity filter |
| `output-format` | No | `json` | Output format for gap report |
| `license-key` | No | `""` | ComplyForm license key (Pro+). Pass via `${{ secrets.COMPLYFORM_LICENSE_KEY }}` |
| `fail-on-findings` | No | `true` | Exit with failure if compliance findings exist |
| `checkov-custom-dir` | No | `""` | Path to custom Checkov policies directory (Pro+) |
| `opa-policy-dir` | No | `""` | Path to OPA/Rego policy directory (Team+) |
| `plan-json-path` | No | `""` | Path to `terraform show -json` plan output for OPA evaluation |
| `dashboard-push` | No | `false` | Push results to ComplyForm dashboard (Team+) |
| `project-label` | No | `""` | Dashboard project label |
| `api-key` | No | `""` | API key for dashboard push. Pass via `${{ secrets.COMPLYFORM_API_KEY }}` |

## Outputs

| Output | Description |
|---|---|
| `score` | Compliance score (0-100) |
| `findings-count` | Total finding count |
| `critical-count` | Critical finding count |
| `report-path` | Path to the generated report file |
| `exit-code` | ComplyForm exit code: `0` (pass), `1` (findings), `2` (runtime error) |

## Tier Gating

ComplyForm uses a tiered licensing model:

- **Community (Free)** — SOC 2 and CIS benchmarks
- **Pro** — 11 frameworks including HIPAA, PCI DSS, ISO 27001. Supports custom Checkov policies.
- **Team** — 24 frameworks plus dashboard push, OPA/Rego policy evaluation, and project labels.
- **Enterprise** — All 53 frameworks with full API access and SSO.

Features gated to a specific tier will silently skip if the active license does not include them.

## Links

- [ComplyForm Website](https://complyform.dev)
- [Documentation](https://docs.complyform.dev)
- [Pricing](https://complyform.dev/pricing)

## License

Apache 2.0 — see [LICENSE](LICENSE) for details.
