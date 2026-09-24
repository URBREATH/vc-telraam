# Telraam VC Map Plugin

**Provided by:** VC Map Project (virtualcitySYSTEMS)

## Description

The Telraam plugin adds traffic data from [Telraam](https://telraam.net/) to [VC Map](https://github.com/virtualcitySYSTEMS/map-ui). It lets users select a map area, load available road segments, and inspect current and historical measurements for each segment.

## Installation Prerequisites

- VC Map UI 6 or later.
- A reachable Telraam proxy API and network access from the VC Map application to the configured proxy.
- An active Telraam sensor network in the area being queried.

## Installation Instructions

Deployment instructions for installing the plugin in a VC Map environment were not included in the provided documentation. For local development, install dependencies with `npm install` and start the development server with `npm start`. See [Development](#development) for build commands.

## Built Image Registry

Not specified in the provided documentation.

## License

This project is licensed under the [MIT License](LICENSE.md).

Copyright 2024 tadolphi [tadolphi@vc.systems](mailto:tadolphi@vc.systems)

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## External technical resources

- [Telraam](https://telraam.net/)
- [VC Map project](https://github.com/virtualcitySYSTEMS/map-ui)

## User Guide References

No separate user guide or FAQ links were included in the provided documentation. Usage instructions are included under [Additional Information](#additional-information).

## Additional Information

### Features

- Draw an area as a polygon or bounding box.
- Request a traffic snapshot for the selected area through the Telraam proxy.
- Add returned road segments as a GeoJSON layer in the VC Map content tree.
- Style segments by hourly car volume.
- Inspect measurements for cars, heavy vehicles, bicycles, pedestrians, and the V85 speed percentile.
- View hourly time series for the last 1 to 180 days.
- Open the corresponding segment on Telraam and access Telraam’s terms of use and privacy information.

### Usage

1. Open the **Telraam** tool in VC Map.
2. Enter a unique name for the area.
3. Draw a polygon or bounding box on the map.
4. Wait for the number of available segments to be displayed.
5. Select **Add to map**.
6. Select a road segment to open its measurements and time series.

The resulting layer is added to the content tree below the Telraam group. Remove a layer using its content-tree action.

### Configuration

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| `telraamURL` | `string` | `https://examplemap.de/xxxx` | Base URL of the Telraam proxy API. |
| `chartType` | `string` | `line` | ApexCharts chart type for the time series, for example `line` or `bar`. |
| `requestDays` | `number` | `1` | Number of days requested when a segment’s feature information is opened. Supported range: 1–180 days. |
| `noisePrediction` | `boolean` | `false` | Reserved configuration option; not currently used by the plugin. |
| `trafficPrediction` | `boolean` | `false` | Reserved configuration option; not currently used by the plugin. |

Example configuration:

```json
{
  "name": "telraam",
  "telraamURL": "https://examplemap.de/xxxx",
  "chartType": "bar",
  "requestDays": 7,
  "noisePrediction": false,
  "trafficPrediction": false
}
```

The `telraamURL` value shown is the configured default example. Set it to the Telraam proxy endpoint for the target environment.

### Telraam proxy API

The configured base URL is used for two requests:

- `POST <telraamURL>/v1/reports/traffic_snapshot` loads traffic features for the selected map extent.
- `POST <telraamURL>/v1/reports/traffic` loads hourly measurements for an individual segment.

The proxy must forward compatible requests to Telraam and allow requests from the VC Map application.

### Development

Install dependencies and run the local development server:

```bash
npm install
npm start
```

Available checks and build commands:

```bash
npm run build
npm run type-check
npm test
npm run lint
```

Create a production bundle with:

```bash
npm run bundle
```

### Limitations

- Results depend on Telraam sensor coverage and data availability.
- The area query uses the current snapshot returned by the proxy.
- Historical measurements require a working proxy connection.
- The plugin does not persist raw Telraam data locally.
