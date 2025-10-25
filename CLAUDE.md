# Runtipi Marine App Store

This repository contains a custom Runtipi app store curated for marine and boating applications.

## Git Workflow Policy

For git workflow policies that apply to all repositories in the Halos monorepo, see the Git Workflow Policy section in @halos-pi-gen/CLAUDE.md

## Overview

Starting with Runtipi v4.0.0, custom app stores allow users to maintain specialized application collections. This marine app store provides curated applications for marine environments, including navigation, monitoring, data logging, and boat systems integration.

**Official Documentation:** https://runtipi.io/docs/guides/create-your-own-app-store

## Repository Structure

```
runtipi-marine-app-store/
├── apps/                       # App definitions
│   └── app-name/               # Each app has its own directory
│       ├── config.json         # App metadata and configuration
│       ├── docker-compose.json # Container service definition
│       └── metadata/
│           ├── logo.jpg        # App logo (1:1 aspect ratio)
│           └── description.md  # Detailed app description
├── __tests__/                  # Validation test suite
│   └── apps.test.ts
├── scripts/                    # Build and validation scripts
└── config.js                   # App store configuration
```

**Important:** The app folder name MUST match the `id` field in `config.json`.

## App Configuration Format

### config.json

Required fields:
```json
{
  "name": "Display Name",
  "id": "app-folder-name",
  "available": true,
  "short_desc": "Brief description",
  "author": "author-name",
  "port": 8080,
  "categories": ["navigation", "monitoring", "utilities"],
  "description": "Detailed description",
  "tipi_version": 3,
  "version": "v1.0.0",
  "source": "https://github.com/...",
  "exposable": true,
  "supported_architectures": ["arm64", "amd64"],
  "dynamic_config": true,
  "form_fields": []
}
```

**Key fields:**
- `id`: Unique identifier, must match folder name
- `port`: External port number (must be unique across all apps)
- `min_tipi_version`: Minimum Runtipi version (e.g., "4.0.0")
- `categories`: Suggested categories: `navigation`, `monitoring`, `data-logging`, `communications`, `weather`, `utilities`
- `supported_architectures`: Include both `arm64` (for Raspberry Pi) and `amd64` when possible
- `exposable`: Set to `true` if the app should be accessible externally
- `dynamic_config`: Set to `true` for apps using modern configuration format

### docker-compose.json

Defines container services using Runtipi's dynamic compose format:
```json
{
  "services": [
    {
      "name": "service-name",
      "image": "registry/image:version",
      "isMain": true,
      "internalPort": "80"
    }
  ]
}
```

**Best practices:**
- Use specific version tags, avoid `latest`
- Set `isMain: true` for the primary service
- Specify `internalPort` (container's internal port, not external)

### metadata/

- **logo.jpg**: Square logo image (1:1 aspect ratio recommended)
- **description.md**: Markdown file with detailed app description, features, and usage notes

## Marine-Specific Categories

Suggested category organization for marine apps:

- **navigation**: Chart plotters, routing, GPS tools
- **monitoring**: System monitors, sensor displays
- **data-logging**: Time-series databases, data collection
- **communications**: VHF, AIS, weather routing
- **weather**: Weather forecasts, GRIB viewers
- **utilities**: System tools, maintenance utilities
- **integration**: Signal K, NMEA tools, protocol converters

## Adding a New App

1. **Create app directory**: `apps/app-id/` (use lowercase, hyphens for multi-word names)
2. **Create config.json**: Define app metadata (see format above)
3. **Create docker-compose.json**: Define container service(s)
4. **Add metadata**:
   - Save logo as `metadata/logo.jpg` (square, 1:1 aspect)
   - Write description in `metadata/description.md`
5. **Test locally**: Run validation tests with `npm test` or `bun test`
6. **Commit and push**: Create feature branch, commit changes, push to GitHub
7. **Create PR**: CI will automatically validate the app structure

## Validation and Testing

The repository includes automated tests that validate:
- File structure (required files present)
- JSON schema compliance
- Port number uniqueness
- Valid architecture specifications
- Folder name matches `id` field

**Run tests locally:**
```bash
# Using npm
npm install
npm test

# Using bun
bun install
bun test
```

## Integration with Halos

This app store is referenced in the Halos image build process:

1. **Pre-installation**: The app store URL is configured during image build
2. **First boot**: Runtipi automatically recognizes the marine app store
3. **User access**: Apps are visible in Runtipi's web interface under custom app stores

**App store URL:** `https://github.com/hatlabs/runtipi-marine-app-store`

## Best Practices

1. **Offline compatibility**: Marine environments often lack internet connectivity
   - Pre-load Docker images in Halos builds when possible
   - Document any first-run connectivity requirements

2. **ARM64 support**: All apps should support `arm64` architecture (Raspberry Pi)

3. **Port management**: Track assigned ports to avoid conflicts
   - Document port assignments
   - Use ports in ranges that don't conflict with standard marine services:
     - Signal K: 3000
     - InfluxDB: 8086
     - Grafana: 3001
     - Cockpit: 9090

4. **Version pinning**: Always use specific version tags in Docker images

5. **Resource efficiency**: Consider resource constraints on Raspberry Pi hardware
   - Test memory usage
   - Optimize for ARM64 performance

6. **Documentation**: Write clear, detailed `description.md` files
   - Installation steps
   - Configuration requirements
   - Integration notes for marine systems

## Maintenance Workflow

1. **Update existing app**: Edit app files, bump version in `config.json`
2. **Add new app**: Follow "Adding a New App" steps above
3. **Test changes**: Run validation suite locally
4. **Version control**: Commit to feature branch, create PR
5. **Review**: CI validates structure, maintainers review functionality
6. **Merge**: Merge to main branch
7. **User update**: Users click "Update App Stores" in Runtipi settings

## Common Marine Apps to Consider

- **Signal K**: Marine data hub (likely already in base Halos-Marine)
- **OpenCPN**: Chart plotter and navigation
- **InfluxDB**: Time-series database (likely in base Halos-Marine)
- **Grafana**: Visualization and dashboards (likely in base Halos-Marine)
- **Node-RED**: Flow-based automation
- **Kip**: Instrument display for Signal K
- **VictronConnect**: Victron energy system monitoring
- **Weather routing tools**
- **AIS processors**
- **NMEA multiplexers/converters**

## Related Documentation

- **Runtipi docs**: https://runtipi.io/docs/guides/create-your-own-app-store
- **Halos monorepo**: @halos-distro/CLAUDE.md
- **Signal K**: https://signalk.org/
- **Marine data standards**: NMEA 0183, NMEA 2000, Signal K
