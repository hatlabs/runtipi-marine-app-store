# AvNav

AvNav is a free and open-source navigation software designed specifically for recreational sailors and boaters. It transforms your device into a powerful marine chartplotter with comprehensive navigation features, making it an ideal solution for turning your Raspberry Pi into a dedicated navigation computer.

## Features

- **Full-Featured Chartplotter**: Display and navigate with various nautical chart formats
- **Waypoint Management**: Create, edit, and manage waypoints and routes
- **Track Recording**: Automatically record your tracks and replay past voyages
- **AIS Integration**: Display AIS targets for enhanced situational awareness
- **NMEA Support**: Connect to NMEA 0183 and NMEA 2000 devices via serial or network
- **Signal K Integration**: Seamlessly integrates with Signal K Server for modern data exchange
- **Anchor Watch**: Set anchor alarms to monitor your position while at anchor
- **Responsive Design**: Touch-optimized interface works on phones, tablets, and computers
- **Offline Operation**: Full functionality without internet connection once charts are loaded
- **Extensible**: Plugin system for additional features and customizations
- **MapProxy Plugin**: Built-in chart server for serving maps to other applications
- **O-Charts Support**: Full support for commercial o-charts chart format

## Default Configuration

- **Web Interface**: Port 8080 (HTTP)
- **Signal K Connection**: Can connect to Signal K on port 3000
- **Data Storage**: Persistent storage at `/var/lib/avnav` for charts, waypoints, tracks, and configuration
- **Network Mode**: Host networking for better device discovery and multicast support

After installation, access AvNav at `http://your-device-ip:8080`.

## Getting Started

1. **Access the Interface**: Navigate to port 8080 on your device
2. **Upload Charts**: Use the web interface to upload nautical charts (supports various formats including MBTiles, gemf, xml)
3. **Configure Data Sources**:
   - If Signal K is installed, AvNav will automatically connect
   - Add NMEA devices through the configuration interface
   - Configure AIS reception if available
4. **Start Navigating**: Create waypoints, plan routes, and track your position

## Chart Formats

AvNav supports multiple chart formats:
- **MBTiles**: Popular raster chart format
- **gemf**: Efficient chart storage format
- **XML**: Chart conversion format
- **Online Charts**: Can fetch charts from online sources

Upload charts through the web interface or place them in the charts directory.

## Integration with Signal K

AvNav is pre-configured to integrate with Signal K Server:
- Receives position, speed, course, and other vessel data
- Displays instrument data alongside the chart
- Automatically discovers Signal K on the local network
- Supports bi-directional data exchange

If Signal K is running on the same system, no additional configuration is needed.

## Hardware Connections

For direct NMEA connections:
- Connect NMEA 0183 devices via USB-to-serial adapters
- Configure serial ports in the AvNav settings
- Supports multiple simultaneous NMEA inputs

The app has access to `/dev` for serial device connectivity.

## Documentation

- **Official Documentation**: https://www.wellenvogel.net/software/avnav/docs/
- **GitHub Repository**: https://github.com/wellenvogel/avnav
- **Chart Sources**: Various free and commercial chart sources available

## Use Cases

- **Coastal Cruising**: Navigate with detailed coastal charts
- **Passage Making**: Plan and execute offshore passages
- **Racing**: Real-time position tracking and tactical displays
- **Anchor Watch**: Monitor your position while at anchor
- **Educational**: Learn navigation with real-time data visualization

## Notes

- Charts are not included and must be uploaded separately
- For optimal performance, use current charts appropriate for your area
- NMEA device access requires proper permissions and hardware connections
- This is navigation software for recreational use; always maintain proper lookout and use official charts for safety-critical navigation

## Community

AvNav is an active open-source project with a supportive community. Visit the project website and GitHub repository for additional plugins, documentation, and support.
