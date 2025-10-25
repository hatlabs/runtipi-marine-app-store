# Runtipi Marine App Store

A curated collection of marine and boating applications for the Runtipi platform, designed for the Halos operating system.

## Overview

This custom app store provides marine-focused applications including navigation tools, data logging, monitoring systems, and boat systems integration. All apps are optimized for Raspberry Pi hardware (ARM64) and designed to work in offline/marine environments.

## Repository Structure

- **apps/**: Individual app directories
  - Each app folder contains:
    - `config.json`: App metadata and configuration
    - `docker-compose.json`: Container service definition
    - `metadata/`: App visuals and documentation
      - `description.md`: Detailed app description
      - `logo.jpg`: App logo (1:1 aspect ratio)

- **__tests__/**: Validation test suite
  - `apps.test.ts`: Automated app structure validation

- **scripts/**: Build and validation utilities

## Marine App Categories

- **Navigation**: Chart plotters, routing, GPS tools
- **Monitoring**: System monitors, sensor displays
- **Data Logging**: Time-series databases, data collection
- **Communications**: VHF, AIS, weather routing
- **Weather**: Forecasts, GRIB viewers
- **Utilities**: System tools, maintenance
- **Integration**: Signal K, NMEA tools, protocol converters

## Adding Apps

See [CLAUDE.md](CLAUDE.md) for detailed instructions on:
- App configuration format
- Adding new apps
- Testing and validation
- Integration with Halos

## Quick Start

```bash
# Install dependencies
bun install  # or: npm install

# Run validation tests
bun test     # or: npm test
```

## Integration with Halos

This app store is pre-configured in Halos marine images. Apps are accessible through the Runtipi web interface at port 80/443.

**App Store URL**: `https://github.com/hatlabs/runtipi-marine-app-store`

## Documentation

- **Detailed Guide**: See [CLAUDE.md](CLAUDE.md) for comprehensive documentation
- **Runtipi Docs**: [Create Your Own App Store Guide](https://runtipi.io/docs/guides/create-your-own-app-store)
- **Halos Project**: [halos-distro](https://github.com/hatlabs/halos-distro)
