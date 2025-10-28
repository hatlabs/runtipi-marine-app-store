# InfluxDB

InfluxDB is a high-performance time series database designed for storing and analyzing time-stamped data. It's perfect for marine applications where you need to store and query sensor data from Signal K and other marine data sources.

## Features

- **Time Series Optimized**: Purpose-built for time-stamped data
- **High Performance**: Fast writes and queries for large datasets
- **SQL-like Query Language**: Familiar syntax for data analysis
- **Built-in Web UI**: Intuitive interface for data exploration
- **Retention Policies**: Automatic data lifecycle management
- **Continuous Queries**: Real-time data aggregation
- **Grafana Integration**: Seamless integration with Grafana for visualization

## Marine Use Cases

- **Sensor Data Storage**: Store data from GPS, depth sounders, wind sensors, etc.
- **Performance Analysis**: Track boat speed, fuel consumption, and efficiency
- **Weather Logging**: Record environmental conditions over time
- **System Monitoring**: Track battery levels, tank levels, and engine parameters
- **Historical Playback**: Analyze past voyages and conditions

## Default Configuration

- **Web Interface**: Port 8086 (HTTP)
- **API**: RESTful HTTP API on port 8086
- **Data Storage**: Persistent volumes for database files

After installation, access the InfluxDB UI at `http://your-device-ip:8086`.

## Integration with Signal K

InfluxDB works seamlessly with Signal K to store marine sensor data:
1. Install Signal K Server
2. Install InfluxDB (this app)
3. Enable the Signal K InfluxDB plugin
4. Configure the plugin to point to `http://localhost:8086`
5. Your marine data will automatically be logged to InfluxDB

## Documentation

- Official website: https://www.influxdata.com/influxdb/
- Documentation: https://docs.influxdata.com/influxdb/

## Note

This app uses InfluxDB v2 for compatibility with Signal K. The database will automatically initialize on first run. You can configure initial setup through the web interface at port 8086.
