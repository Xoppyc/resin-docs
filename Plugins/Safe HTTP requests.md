# Safe HTTP Requests

Resin provides a sandboxed HTTP client (`this.app.request` or `requestUrl`) routed through a Rust gateway to prevent SSRF and protect vault security.

## The Request API

```ts
import { Plugin } from "resin-api";

export default class WeatherPlugin extends Plugin {
  async fetchForecast(city: string) {
    try {
      const response = await this.app.request({
        url: `https://api.weather.com/v1/forecast?city=${encodeURIComponent(city)}`,
        method: "GET",
        headers: { Accept: "application/json" },
        timeoutMs: 10000,
      });

      if (!response.ok) return null;
      return response.json();
    } catch (err) {
      console.error("Network request failed:", err);
      return null;
    }
  }
}
```

## Request Options

| Option | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `url` | `string` | _(required)_ | Target URL (`https://` or `http://`). |
| `method` | `string` | `'GET'` | HTTP method. |
| `headers` | `Record<string, string>` | `{}` | Custom request headers. |
| `body` | `string` | `undefined` | Payload for POST/PUT. |
| `timeoutMs` | `number` | `15000` | Abort timeout in ms. |
| `allowLocal`| `boolean` | `false` | Allows access to localhost/LAN (e.g. local LLM servers). |

## Security Guardrails

The native Rust gateway enforces security invariants:
- **SSRF protection**: Blocks loopback and private subnets (`127.0.0.1`, `10.0.0.0/8`, `192.168.0.0/16`) by default unless `allowLocal: true`.
- **Payload caps**: Responses are capped at 25MB to prevent memory exhaustion.
- **Protocol validation**: Only HTTP and HTTPS are permitted.

