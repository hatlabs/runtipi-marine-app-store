# Runtipi Marine App Store

Custom Runtipi app store for marine and boating applications.

## Git Workflow Policy

**IMPORTANT:** Always ask before committing, pushing, tagging, or running destructive git operations.

## Adding a New App

1. Create `apps/app-id/` (lowercase, hyphens)
2. Add `config.json` (id must match folder name)
3. Add `docker-compose.json`
4. Add `metadata/logo.jpg` (1:1 ratio) and `metadata/description.md`
5. Test: `bun test`
6. Create PR (CI validates)

## Key Requirements

- **Port uniqueness**: Each app needs unique external port
- **ARM64 support**: Required for Raspberry Pi
- **Versioning**: Pin Docker versions (no `latest`)
- **Categories**: `navigation`, `monitoring`, `data-logging`, `communications`, `weather`, `utilities`, `integration`

## Testing

```bash
bun test
```
