# Persistent Storage

Resin provides isolated persistent storage for every plugin. Data is saved to `<vault>/.resin/plugins/<pluginId>/data.json`.

## Define Settings Interface

```ts
interface BackupSettings {
  intervalMinutes: number;
  backupFolder: string;
}

const DEFAULT_SETTINGS: BackupSettings = {
  intervalMinutes: 15,
  backupFolder: "Backups",
};
```

## Load Settings on Startup

Call `this.loadData()` inside `onLoad()`. Merge with `DEFAULT_SETTINGS` so new configuration options have defaults:

```ts
import { Plugin } from "resin-api";

export default class BackupPlugin extends Plugin {
  settings: BackupSettings = DEFAULT_SETTINGS;

  override async onLoad(): Promise<void> {
    const saved = await this.loadData<Partial<BackupSettings>>();
    this.settings = Object.assign({}, DEFAULT_SETTINGS, saved);
  }

  async updateSettings(updates: Partial<BackupSettings>): Promise<void> {
    this.settings = { ...this.settings, ...updates };
    await this.saveData(this.settings);
  }
}
```

If `data.json` does not exist yet, `loadData()` returns `null` and defaults are preserved.

To provide user-facing controls, see [[Settings tab DSL]].

