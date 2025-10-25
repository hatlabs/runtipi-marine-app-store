# Signal K Server

Signal K is a modern and open data format for marine use, designed to be a universal standard for marine data exchange. It replaces the aging NMEA 0183 and extends NMEA 2000 with a modern, internet-friendly format.

The Signal K Server is a full-featured Node.js-based reference implementation that acts as a central hub for all your boat's data. It collects, processes, and distributes marine sensor data in real-time, making it accessible to various displays, instruments, and applications both on board and remotely.

## Features

- **Universal Data Format**: Compatible with NMEA 0183, NMEA 2000, and other marine data sources
- **Real-time Streaming**: WebSocket API for live data distribution
- **RESTful API**: HTTP API for data access and control
- **Extensible**: Plugin system for adding functionality
- **Data Logging**: Record and replay vessel data
- **Web Interface**: Built-in configuration and monitoring dashboard
- **Open Source**: Community-driven development

## Default Configuration

- **Web Interface**: Port 3000 (HTTP)
- **WebSocket**: Port 3000 (same port, different protocol)
- **TCP Stream**: Port 3858

After installation, access the admin interface at `http://your-device-ip:3000`.

## First Steps

1. Access the admin interface
2. Configure your data connections (NMEA 0183, NMEA 2000, etc.)
3. Enable desired plugins
4. Start collecting and visualizing your vessel data

## Documentation

- Official Signal K documentation: https://signalk.org/
- Server documentation: https://github.com/SignalK/signalk-server

## Important Note

This app requires hardware access (serial ports, USB devices) for NMEA connections. Advanced configuration may be needed for full functionality with marine hardware.
