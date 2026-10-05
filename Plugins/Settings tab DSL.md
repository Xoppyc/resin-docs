# Settings Tabs

Plugins can register dedicated configuration tabs in **Settings** using `PluginSettingTab` and the fluent `Setting` builder.

![[plugin-settings-tab-ui.png|Plugin settings tab rendered with toggle and inputs]]

## Create a Settings Tab

Extend `PluginSettingTab` and implement `display()`:

```ts
import { PluginSettingTab, Setting, type AppAPI } from "resin-api";
import type MyPlugin from "./index";

export class MyPluginSettingTab extends PluginSettingTab {
  plugin: MyPlugin;

  constructor(app: AppAPI, plugin: MyPlugin) {
    super(app, plugin);
    this.plugin = plugin;
    this.name = "My Plugin";
    this.icon = "GearIcon";
  }

  display(): void {
    const { containerEl } = this;
    containerEl.innerHTML = "";

    // Toggle Setting
    new Setting(containerEl)
      .setName("Enable Timestamps")
      .setDesc("Append current date to new notes")
      .addToggle((toggle) =>
        toggle
          .setValue(this.plugin.settings.enableTimestamps)
          .onChange(async (value) => {
            this.plugin.settings.enableTimestamps = value;
            await this.plugin.saveData(this.plugin.settings);
          }),
      );

    // Text Input Setting
    new Setting(containerEl)
      .setName("Default Directory")
      .setDesc("Target folder for created notes")
      .addText((text) =>
        text
          .setPlaceholder("Notes/Daily")
          .setValue(this.plugin.settings.defaultDir)
          .onChange(async (value) => {
            this.plugin.settings.defaultDir = value;
            await this.plugin.saveData(this.plugin.settings);
          }),
      );
  }
}
```

## Register the Tab

Register the tab inside your plugin's `onLoad()`:

```ts
import { Plugin } from "resin-api";
import { MyPluginSettingTab } from "./MyPluginSettingTab";

export default class MyPlugin extends Plugin {
  override async onLoad(): Promise<void> {
    this.addSettingTab(new MyPluginSettingTab(this.app, this));
  }
}
```

