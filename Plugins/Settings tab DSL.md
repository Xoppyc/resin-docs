# Settings Tabs & Fluent DSL

Plugins can register their own dedicated settings tabs inside the **Resin Settings Modal** using `PluginSettingTab` and the fluent `Setting` builder component.

---

## 1. Creating a Settings Tab

Extend `PluginSettingTab` and implement the `display()` method:

```ts
import { PluginSettingTab, Setting, type AppAPI } from "resin";
import type MyPlugin from "./index";

export class MyPluginSettingTab extends PluginSettingTab {
  plugin: MyPlugin;

  constructor(app: AppAPI, plugin: MyPlugin) {
    super(app, plugin);
    this.plugin = plugin;
    this.name = "My Plugin";
    this.icon = "GearIcon"; // Phosphor icon
    this.priority = 50; // Ordering priority (lower = higher up)
  }

  display(): void {
    const { containerEl } = this;
    containerEl.innerHTML = ""; // Clear existing contents

    containerEl.createEl("h2", { text: "General Settings" });

    // 1. Toggle Setting
    new Setting(containerEl)
      .setName("Enable Timestamp")
      .setDesc("Append current timestamp to new notes")
      .addToggle((toggle) =>
        toggle
          .setValue(this.plugin.settings.showTimestamp)
          .onChange(async (value) => {
            this.plugin.settings.showTimestamp = value;
            await this.plugin.saveData(this.plugin.settings);
          }),
      );

    // 2. Text Input Setting
    new Setting(containerEl)
      .setName("Default Folder")
      .setDesc("Folder where notes are created by default")
      .addText((text) =>
        text
          .setPlaceholder("Notes/Daily")
          .setValue(this.plugin.settings.defaultFolder)
          .onChange(async (value) => {
            this.plugin.settings.defaultFolder = value;
            await this.plugin.saveData(this.plugin.settings);
          }),
      );

    // 3. Slider Setting
    new Setting(containerEl)
      .setName("Auto-Save Interval")
      .setDesc("Interval in seconds for automatic background saves")
      .addSlider((slider) =>
        slider
          .setLimits(10, 300, 10)
          .setValue(this.plugin.settings.autoSaveInterval)
          .onChange(async (value) => {
            this.plugin.settings.autoSaveInterval = value;
            await this.plugin.saveData(this.plugin.settings);
          }),
      );

    // 4. Dropdown Select Setting
    new Setting(containerEl)
      .setName("Export Format")
      .setDesc("Preferred export document format")
      .addDropdown((dropdown) =>
        dropdown
          .addOption("pdf", "PDF Document")
          .addOption("html", "HTML Webpage")
          .addOption("docx", "Word Document")
          .setValue(this.plugin.settings.exportFormat)
          .onChange(async (value) => {
            this.plugin.settings.exportFormat = value;
            await this.plugin.saveData(this.plugin.settings);
          }),
      );
  }
}
```

---

## 2. Registering the Settings Tab in `Plugin`

In your plugin's `onLoad()`:

```ts
import { Plugin } from "resin";
import { MyPluginSettingTab } from "./MyPluginSettingTab";

export default class MyPlugin extends Plugin {
  override async onLoad(): Promise<void> {
    // Register settings tab
    this.app.ui.registerSettingTab(new MyPluginSettingTab(this.app, this));
  }
}
```

---

## 3. Next Steps

- Explore full code examples in **[[Example sidebar React|Recipes & Examples]]**.
