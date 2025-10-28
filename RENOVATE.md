# Renovate Automation Guide

This repository uses [Renovate](https://docs.renovatebot.com/) to automatically check for and propose updates to Docker images used in marine apps.

## Overview

Renovate runs daily (02:00 UTC) via GitHub Actions and:
1. **Detects** new versions of Docker images in `apps/*/docker-compose.json`
2. **Creates** separate pull requests for each app update
3. **Auto-updates** `config.json` with new version and incremented `tipi_version`
4. **Runs tests** to validate changes before creating the PR

## How It Works

### Docker Image Detection

Renovate uses a custom regex manager to extract Docker image information from `docker-compose.json` files:

```json
{
  "services": [
    {
      "image": "influxdb:2.7.11"  // Renovate detects this
    }
  ]
}
```

When a new version (e.g., `2.7.12`) is available, Renovate will:
1. Update `docker-compose.json` → `"image": "influxdb:2.7.12"`
2. Run `scripts/update-config.ts` to update `config.json`:
   - `version`: `"2.7.12"`
   - `tipi_version`: incremented by 1
   - `updated_at`: current timestamp
3. Run tests with `bun test`
4. Create a PR if all tests pass

### PR Grouping Strategy

Each app gets its own PR for updates. For example:
- PR: "chore(deps): update influxdb to 2.7.12"
- PR: "chore(deps): update grafana/grafana-oss to 12.1.2"

This allows independent review and deployment of each app update.

## Version Constraints

You can control which versions Renovate will propose by adding constraints in `renovate.json`.

### Current Constraints

#### InfluxDB - Patch updates only (2.7.x)
```json
{
  "matchPaths": ["apps/influxdb/**"],
  "allowedVersions": "~2.7.0"
}
```
- **Allows**: 2.7.11 → 2.7.12 → 2.7.13
- **Blocks**: 2.7.11 → 2.8.0 or 3.0.0

#### Grafana - Major version 12 only
```json
{
  "matchPaths": ["apps/grafana/**"],
  "allowedVersions": "/^12\\./"
}
```
- **Allows**: 12.1.1 → 12.1.2 → 12.2.0 → 12.9.9
- **Blocks**: 12.x.x → 13.0.0

#### OpenCPN - Clean semver tags only
```json
{
  "matchPaths": ["apps/opencpn/**"],
  "allowedVersions": "/^\\d+\\.\\d+\\.\\d+$/"
}
```
- **Allows**: 5.12.4 → 5.12.5 → 5.13.0 → 6.0.0
- **Blocks**: Complex PPA/build tags like `5.12.4-dfsg-1.bpo24.04.ppa1.build.1`
- **Why**: OpenCPN publishes multiple tags per release; this ensures Renovate only tracks simple semver

### Constraint Syntax Options

#### Semver Ranges (npm-style)

| Syntax | Meaning | Example Allowed Versions |
|--------|---------|-------------------------|
| `~2.7.0` | Patch updates in 2.7 | 2.7.0, 2.7.1, 2.7.99 |
| `^2.7.0` | Minor updates in 2.x | 2.7.0, 2.8.0, 2.99.0 |
| `<3.0.0` | Any version below 3.0 | 1.0.0, 2.99.99 |
| `>=2.7.0 <3.0.0` | Range constraint | 2.7.0 to 2.99.99 |

#### Regex Patterns

| Pattern | Meaning | Example Allowed Versions |
|---------|---------|-------------------------|
| `/^12\./` | Major version 12 | 12.0.0, 12.1.5, 12.99.99 |
| `/^v2\./` | Versions starting with v2. | v2.0.0, v2.17.3 |
| `/^\d+\.\d+$/` | Only major.minor (no patch) | 1.2, 5.10 |
| `/^\d+\.\d+\.\d+$/` | Clean semver only (no suffixes) | 5.12.4, 6.0.0 (blocks 5.12.4-rc1) |

#### Literal Values

| Value | Meaning |
|-------|---------|
| `"latest"` | Only the latest tag |
| `"main"` | Only the main tag |
| `"1.2.3"` | Exact version only |

### Adding a New Constraint

To add a version constraint for an app, add a new rule to `packageRules` in `renovate.json`:

```json
{
  "description": "Pin signalk to v2.x (no v3 updates)",
  "matchPaths": ["apps/signalk/**"],
  "matchDatasources": ["docker"],
  "allowedVersions": "/^v2\\./",
  "enabled": true
}
```

**Important**: Rules are evaluated in order, and the first matching rule wins.

### Disabling Updates for an App

To completely disable Renovate for a specific app:

```json
{
  "description": "Disable avnav updates (manually managed)",
  "matchPaths": ["apps/avnav/**"],
  "enabled": false
}
```

## Testing Renovate Locally

You can test Renovate configuration changes without waiting for the scheduled workflow:

### 1. Manual Workflow Trigger

Go to: [Actions → Renovate](https://github.com/hatlabs/runtipi-marine-app-store/actions/workflows/renovate.yml)

Click "Run workflow" and optionally set `LOG_LEVEL=debug` for verbose output.

### 2. Local Testing with Renovate CLI

```bash
# Install Renovate CLI
npm install -g renovate

# Dry run (no PRs created)
LOG_LEVEL=debug renovate --dry-run --platform=local

# Or use npx
npx -p renovate renovate --dry-run --platform=local
```

## Troubleshooting

### Renovate is not creating PRs

**Check workflow runs**: [Actions → Renovate](https://github.com/hatlabs/runtipi-marine-app-store/actions/workflows/renovate.yml)

Common issues:
- **No updates available**: All apps are already on latest allowed versions
- **Version constraints too strict**: Check `allowedVersions` in packageRules
- **Rate limiting**: GitHub/Docker Hub API limits reached
- **Configuration errors**: Check renovate.json syntax with [schema validator](https://docs.renovatebot.com/renovate-schema/)

### PR created but tests fail

Check the PR's GitHub Actions test run. Common failures:
- **Schema validation**: Invalid config.json or docker-compose.json
- **Bun dependency issues**: Dependencies out of sync
- **Script errors**: update-config.ts failed

Fix the issue, then Renovate will automatically update the PR on the next run.

### Want to update an app manually

1. Update `docker-compose.json` with new version
2. Run the update script:
   ```bash
   bun ./scripts/update-config.ts apps/appname/docker-compose.json 1.2.3
   ```
3. Test:
   ```bash
   bun test
   ```
4. Commit and push

### Renovate ignoring a new version

Check if:
1. **Version constraint** blocks it (see `allowedVersions` in renovate.json)
2. **Package disabled** (check `enabled: false` in packageRules)
3. **Datasource issue**: Docker registry may be unavailable or image private
4. **Version format mismatch**: Non-semver tags may not be detected properly

Enable debug logging to see what Renovate is finding:
- Run workflow manually with `LOG_LEVEL=debug`
- Check logs for the specific package

## Configuration Files

| File | Purpose |
|------|---------|
| `renovate.json` | Main Renovate configuration |
| `.github/workflows/renovate.yml` | Scheduled workflow (daily 02:00 UTC) |
| `scripts/update-config.ts` | Post-upgrade hook to sync config.json |
| `__tests__/apps.test.ts` | Validation tests run after updates |

## Best Practices

### Version Pinning Strategy

1. **Production-critical apps**: Pin to minor version (`~2.7.0`)
2. **Stable mature apps**: Allow minor updates (`^2.7.0` or `/^12\./`)
3. **Experimental apps**: Allow all updates (no constraint)
4. **Custom builds**: Pin to branch/tag (`main`, `stable`)

### When to Update Constraints

- **After major version testing**: Once you've validated a new major version works, update the constraint
- **Breaking changes upstream**: Pin to current major if upstream introduces compatibility issues
- **Hardware dependencies**: Pin versions if specific versions are required for ARM64/hardware support

### Monitoring Updates

- **Watch the repository** to receive notifications when Renovate creates PRs
- **Review PRs promptly** to avoid falling behind on security updates
- **Test updates** in your environment before merging
- **Check upstream changelogs** linked in Renovate PRs to understand changes

## Advanced Configuration

### Custom Commit Messages

Renovate uses semantic commits by default:
```
chore(deps): update influxdb to 2.7.12
```

To customize, modify the `semanticCommitType` and `semanticCommitScope` in the packageRule.

### Schedule Changes

To change when Renovate runs, edit `.github/workflows/renovate.yml`:

```yaml
on:
  schedule:
    - cron: '0 2 * * *'  # Daily at 02:00 UTC
```

### Automerge

Currently disabled (`automerge: false`) for all updates. To enable for specific updates:

```json
{
  "matchPaths": ["apps/influxdb/**"],
  "matchUpdateTypes": ["patch"],
  "automerge": true
}
```

**Warning**: Only enable automerge for well-tested, low-risk updates.

## Resources

- [Renovate Documentation](https://docs.renovatebot.com/)
- [Configuration Options](https://docs.renovatebot.com/configuration-options/)
- [Version Constraints](https://docs.renovatebot.com/configuration-options/#allowedversions)
- [Custom Managers](https://docs.renovatebot.com/modules/manager/regex/)
- [Runtipi App Store Guidelines](./CLAUDE.md)

## Getting Help

- **Renovate issues**: Check [Renovate logs](https://github.com/hatlabs/runtipi-marine-app-store/actions/workflows/renovate.yml)
- **App issues**: See [CLAUDE.md](./CLAUDE.md) for app development guide
- **Repository issues**: Open an issue in this repository
