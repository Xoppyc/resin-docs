# Isolated Persistent Storage

Resin provides isolated JSON storage for every plugin. Data is persisted automatically to `<vault>/.resin/plugins/<pluginId>/data.json`.

---

## 1. Defining Settings Interfaces and Defaults

Define a TypeScript interface for your plugin's configuration and provide default fallback values:

```ts
interface AutoBackupSettings {
  intervalMinutes: number;
  backupFolder: string;
  notifyOnBackup: boolean;
}

const DEFAULT_SETTINGS: AutoBackupSettings = {
  intervalMinutes: 15,
  backupFolder: "Backups",
  notifyOnBackup: true,
};
```

---

## 2. Loading Settings on Startup

Call `this.loadData()` in `onLoad()` to retrieve saved data. Always merge with defaults so newly introduced setting keys receive default values when existing users upgrade:

```ts
import { Plugin } from "resin";

export default class AutoBackupPlugin extends Plugin {
  settings: AutoBackupSettings = DEFAULT_SETTINGS;

  override async onLoad(): Promise<void> {
    const savedData = await this.loadData<Partial<AutoBackupSettings>>();
    this.settings = Object.assign({}, DEFAULT_SETTINGS, savedData);

    console.log("[AutoBackup] Loaded settings:", this.settings);
  }
}
```

If no `data.json` exists on disk yet, `loadData()` returns `null`, and `Object.assign` cleanly falls back to `DEFAULT_SETTINGS`.

---

## 3. Saving Changes

Whenever settings change, update the local instance and call `this.saveData()`:

```ts
async updateSettings(updates: Partial<AutoBackupSettings>): Promise<void> {
  this.settings = { ...this.settings, ...updates };
  await this.saveData(this.settings);
}
```

`saveData()` serializes the object to formatted JSON and writes it safely to `<vault>/.resin/plugins/<pluginId>/data.json`.

---

## 4. Building Settings UI

To provide user-facing controls for these settings in the app settings modal, use the fluent [[Settings tab DSL]].
