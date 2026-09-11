# Telraam VC Map Plugin

The Telraam plugin adds traffic data from [Telraam](https://telraam.net/) to
[VC Map](https://github.com/virtualcitySYSTEMS/map-ui). It lets users select an
area on the map, load available Telraam road segments, and inspect current and
historical measurements for each segment.

## Features

- Draw an area as a polygon or bounding box.
- Request a traffic snapshot for the selected area through the Telraam proxy.
- Add the returned road segments as a GeoJSON layer in the VC Map content tree.
- Style segments by hourly car volume.
- Inspect measurements for cars, heavy vehicles, bicycles, pedestrians, and
  the V85 speed percentile.
- View hourly time series for the last 1 to 180 days.
- Open the corresponding segment on Telraam and access Telraam's terms of use
  and privacy information.

## Requirements

- VC Map UI 6 or later.
- A reachable Telraam proxy API.
- A network connection from the VC Map application to the configured proxy.
- An active Telraam sensor network in the area being queried.

The default proxy is:

```text
https://examplemap.de/xxxx
```

## Usage

1. Open the **Telraam** tool in VC Map.
2. Enter a unique name for the area.
3. Draw a polygon or bounding box on the map.
4. Wait for the number of available segments to be displayed.
5. Select **Add to map**.
6. Select a road segment to open its measurements and time series.

The resulting layer is added to the content tree below the Telraam group. A
layer can be removed using its content-tree action.

## Configuration

The plugin accepts the following options:

| Option              | Type      | Default                                           | Description                                                                  |
| ------------------- | --------- | ------------------------------------------------- | ---------------------------------------------------------------------------- |
| `telraamURL`        | `string`  | `https://examplemap.de/xxxx` | Base URL of the Telraam proxy API.                                           |
| `chartType`         | `string`  | `line`                                            | ApexCharts chart type used for the time series, for example `line` or `bar`. |
| `requestDays`       | `number`  | `1`                                               | Number of days requested when a segment's feature information is opened.     |
| `noisePrediction`   | `boolean` | `false`                                           | Reserved configuration option; not currently used by the plugin.             |
| `trafficPrediction` | `boolean` | `false`                                           | Reserved configuration option; not currently used by the plugin.             |

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

## Telraam proxy API

The configured base URL is used for two requests:

- `POST <telraamURL>/v1/reports/traffic_snapshot` loads traffic features for
  the selected map extent.
- `POST <telraamURL>/v1/reports/traffic` loads hourly measurements for an
  individual segment.

The proxy is responsible for forwarding compatible requests to Telraam and
must allow requests from the VC Map application.

## Development

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

## Limitations

- Results depend on Telraam sensor coverage and data availability.
- The area query uses the current snapshot returned by the proxy.
- Historical measurements require a working proxy connection.
- The plugin does not persist raw Telraam data locally.

## License

This project is licensed under the [MIT License](LICENSE.md).
