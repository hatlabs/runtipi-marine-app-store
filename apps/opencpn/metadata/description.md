# OpenCPN

OpenCPN is a free and open-source chartplotter and marine GPS navigation software designed for recreational sailors and boaters. This containerized version provides full desktop access through your web browser using Selkies remote desktop technology - no VNC client or special software required.

## Features

- **Browser-Based Access**: Access OpenCPN from any device with a modern web browser
- **Full Desktop Application**: Complete OpenCPN functionality, not a limited web version
- **Comprehensive Chart Support**: Display and navigate with various nautical chart formats (MBTiles, BSB, S-57, CM93, and more)
- **Waypoint & Route Management**: Create, edit, and manage waypoints, routes, and tracks
- **AIS Integration**: Display AIS targets for enhanced situational awareness
- **NMEA Support**: Connect to NMEA 0183 and NMEA 2000 data sources
- **Plugin Ecosystem**: Extend functionality with a wide range of available plugins
- **Route Planning**: Advanced route planning with tide, current, and weather data
- **Track Recording**: Automatically record and replay your tracks
- **GPU Acceleration**: Hardware-accelerated rendering for smooth chart display
- **File Management**: Easy chart upload/download through web interface
- **Responsive Interface**: Adapts to your browser window size

## Default Configuration

- **Web Interface**: Port 3020 (HTTP)
- **No Authentication**: Suitable for local networks (authentication can be added via environment variables)
- **GPU Acceleration**: Enabled by default if compatible hardware is available
- **Persistent Storage**: All configuration, charts, and user data stored in `/config` volume

After installation, access OpenCPN at `http://your-device-ip:3020`.

## Getting Started

1. **Access the Interface**: Navigate to port 3020 on your device
2. **Initial Setup**: OpenCPN will launch automatically - complete the setup wizard
3. **Upload Charts**:
   - Click the folder icon in the top toolbar
   - Use the upload feature to transfer chart files
   - In OpenCPN: Options → Charts → Add Directory → `/config/charts`
4. **Configure Data Sources**:
   - Add Signal K connection: Options → Connections → Add Connection → Network (TCP/UDP)
   - Configure NMEA devices as needed
   - Set up AIS reception if available

## Chart Formats Supported

OpenCPN supports multiple chart formats:
- **Raster Charts**: BSB/KAP, GeoTIFF, MBTiles
- **Vector Charts**: S-57/S-63 ENC, CM93
- **Online Charts**: Google Earth KML, WMS servers
- **Custom Formats**: Via plugins

Charts must be uploaded or mounted to the `/config/charts/` directory.

## Integration with Signal K

OpenCPN integrates seamlessly with Signal K Server:
1. In OpenCPN: Options → Connections → Add Connection
2. Select: Network / TCP
3. Protocol: Signal K
4. Address: `localhost` (or Signal K container name)
5. Port: `3000`
6. Enable: Receive input on this port

## NMEA Data Sources

Configure NMEA connections:
- **Network**: Connect to Signal K, NMEA servers, or UDP broadcasts
- **Serial**: Mount serial devices with proper device access
- **File Playback**: Upload NMEA log files for track replay

## Advanced Features

### Hardware Acceleration

GPU acceleration is enabled by default for improved chart rendering performance. Requires:
- Intel or AMD GPU with DRI3 support
- Proper GPU drivers on host system
- Falls back to software rendering if unavailable

### Custom Resolution

The interface adapts to your browser window size automatically. For fixed resolution, configure via environment variables.

### Authentication

For secure remote access, authentication can be enabled by setting:
- `CUSTOM_USER`: Username
- `PASSWORD`: Password

### Plugins

OpenCPN supports numerous plugins for enhanced functionality:
- Weather routing
- Tide and current prediction
- Celestial navigation
- Radar overlay
- And many more

Install plugins through OpenCPN's plugin manager: Options → Plugins

## Use Cases

- **Coastal Cruising**: Navigate with detailed coastal charts and real-time position tracking
- **Passage Planning**: Plan routes with waypoints, tides, and currents
- **Route Optimization**: Use weather routing plugins for optimal passages
- **Real-Time Navigation**: Monitor position, course, and speed from instruments
- **Chart Management**: Organize and update your chart collection
- **Educational**: Learn navigation with simulated data playback
- **Remote Monitoring**: Check vessel position and tracks from anywhere

## Chart Sources

Charts are not included and must be obtained separately:
- **Free Sources**: NOAA ENC charts (US waters), OpenSeaMap, OpenCPN chart downloader plugin
- **Commercial**: Navionics, C-MAP, official hydrographic office charts
- **Custom**: Create your own charts from satellite imagery

## Performance Tips

- Enable GPU acceleration for smoother chart rendering
- Use quilted charts for large coverage areas
- Adjust chart detail levels based on zoom
- Pre-load chart areas before cruising
- Use track simplification for long passages

## Documentation

- **OpenCPN User Manual**: https://opencpn.org/wiki/dokuwiki/doku.php?id=opencpn:opencpn_user_manual
- **Chart Sources**: https://opencpn.org/OpenCPN/info/chartsource.html
- **Plugin Catalog**: https://opencpn.org/OpenCPN/info/pluginslist.html
- **Forum Support**: https://www.cruisersforum.com/forums/f134/

## Notes

- This is navigation software for recreational use; always maintain proper lookout and use official charts for safety-critical navigation
- Charts are not included and must be uploaded separately
- Initial chart loading may take time depending on chart size and quantity
- The web interface uses WebRTC technology - modern browsers (Chrome, Firefox, Edge) recommended
- For best performance on Raspberry Pi, consider using vector charts (S-57) over large raster charts

## Support

For OpenCPN-specific questions, refer to the official documentation and forums. For Docker image issues, visit the [GitHub repository](https://github.com/hatlabs/opencpn-docker).
