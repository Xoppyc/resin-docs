# Safe HTTP Requests

In Resin, plugins interact with external APIs through a hardened HTTP client rather than raw browser `fetch()`.

---

## Why Plugins Use `app.request()`

Running arbitrary `fetch()` in desktop applications creates real security risks:

1. **Local Network Scanning (SSRF)**: A malicious plugin could probe your router settings at `192.168.1.1` or query private local services running on `localhost:8080`.
2. **Silent Vault Exfiltration**: Unrestricted network calls make it easy to upload note contents to unvetted tracking endpoints.
3. **Memory Leaks and Hangs**: A request without a hard timeout or size cap can lock up resources.

To solve this, Resin routes all external requests through a native Rust gateway that enforces security checks before any bytes leave your machine.

---

## The Request API

Inside your plugin, call `this.app.request()` (or import `requestUrl` from `resin`):

```ts
import { Plugin } from "resin";

export default class WeatherPlugin extends Plugin {
  id = "community.weather";
  name = "Weather Widget";

  async fetchForecast(city: string) {
    try {
      const response = await this.app.request({
        url: `https://api.weather.com/v1/forecast?city=${encodeURIComponent(city)}`,
        method: "GET",
        headers: {
          Accept: "application/json",
        },
        timeoutMs: 10000,
      });

      if (!response.ok) {
        console.error("Weather request failed with status", response.status);
        return null;
      }

      // Parse JSON directly with the helper
      const data = response.json();
      return data;
    } catch (err) {
      console.error("Network request failed:", err);
      return null;
    }
  }
}
```

---

## Request Options

| Option       | Type                                                        | Default      | Description                                                |
| :----------- | :---------------------------------------------------------- | :----------- | :--------------------------------------------------------- |
| `url`        | `string`                                                    | _(required)_ | Full target URL (must begin with `https://` or `http://`). |
| `method`     | `'GET' \| 'POST' \| 'PUT' \| 'DELETE' \| 'PATCH' \| 'HEAD'` | `'GET'`      | HTTP method.                                               |
| `headers`    | `Record<string, string>`                                    | `{}`         | Custom request headers.                                    |
| `body`       | `string`                                                    | `undefined`  | Request payload for POST, PUT, or PATCH.                   |
| `timeoutMs`  | `number`                                                    | `15000`      | Abort timeout in milliseconds (max 60000).                 |
| `allowLocal` | `boolean`                                                   | `false`      | Allows access to localhost during plugin development.      |

---

## Response Object

The returned promise resolves to an object with:

- **`status`**: HTTP status code (e.g. `200`, `404`).
- **`statusText`**: Status reason phrase (e.g. `'OK'`, `'Not Found'`).
- **`headers`**: Normalized lowercase map of response headers.
- **`text`**: Raw response body as a string.
- **`ok`**: Boolean flag indicating whether status is between 200 and 299.
- **`json<T>()`**: Convenience method to parse `text` into a typed JSON object.

---

## Security Safeguards in Rust

Every request must pass the following checks in `src-tauri/src/http.rs`:

- **SSRF Defense**: The gateway resolves the target host's IP address. If the IP points to a loopback (`127.0.0.1`, `::1`), private LAN (`10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`), link-local (`169.254.0.0/16`), or multicast address, the request is immediately rejected unless `allowLocal: true` is set.
- **Protocol Enforcement**: Only HTTP and HTTPS are permitted. File URLs (`file://`), JavaScript protocols, and raw sockets are blocked.
- **Payload Limits**: Response bodies are capped at 25MB to prevent memory exhaustion.
- **Plugin Auditing**: Resin automatically appends your plugin ID to the User-Agent header (`Resin/0.3.5 (+https://resin.md) Plugin/<pluginId>`), allowing servers and local logs to trace request provenance.

---

## Local Development Mode

If you are developing a plugin that talks to a local dev server (for example, a local Ollama instance on `http://localhost:11434`), pass `allowLocal: true`:

```ts
const response = await this.app.request({
  url: "http://localhost:11434/api/tags",
  method: "GET",
  allowLocal: true,
});
```

Community plugins submitted to the official directory must not use `allowLocal: true` unless their specific feature set requires local server communication.
