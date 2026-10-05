# Recipe: Complete Production Plugin

This recipe brings all Resin extensibility concepts together into an end-to-end production plugin:

- Isolated persistent configuration
- Settings Tab with the fluent DSL
- Left Ribbon button
- Command Palette action with shortcut
- Custom Context Menu provider
- Persistent Status Bar indicator
- Custom React Leaf View

---

## Complete Plugin Code

```ts
import {
  Plugin,
  PluginSettingTab,
  Setting,
  View,
  type AppAPI,
  type WorkspaceLeaf,
  type MenuItem,
  useApp,
} from 'resin'
import React, { useState, useEffect } from 'react'

// 1. Settings Types & Defaults
interface NoteReminderSettings {
  defaultReminderMinutes: number
  soundEnabled: boolean
}

const DEFAULT_SETTINGS: NoteReminderSettings = {
  defaultReminderMinutes: 30,
  soundEnabled: true,
}

// 2. React UI Component
function ReminderDashboard() {
  const app = useApp()
  const [notes, setNotes] = useState(app.vault.getMarkdownFiles())

  return (
    <div style={{ padding: 24 }}>
      <h2>Reminders Dashboard</h2>
      <p>Tracking reminders across {notes.length} vault notes.</p>
    </div>
  )
}

// 3. Custom View
class ReminderView extends View {
  getViewType(): string {
    return 'reminder-view'
  }
  getDisplayText(): string {
    return 'Reminders'
  }
  override getIcon(): string {
    return 'BellIcon'
  }
  override async onLoad(): Promise<void> {
    this.renderReact(<ReminderDashboard />)
  }
}

// 4. Settings Tab
class ReminderSettingTab extends PluginSettingTab {
  plugin: NoteReminderPlugin

  constructor(app: AppAPI, plugin: NoteReminderPlugin) {
    super(app, plugin)
    this.plugin = plugin
    this.name = 'Note Reminders'
    this.icon = 'BellIcon'
    this.priority = 50
  }

  display(): void {
    const { containerEl } = this
    containerEl.innerHTML = ''
    containerEl.createEl('h2', { text: 'Reminder Settings' })

    new Setting(containerEl)
      .setName('Default Reminder Offset')
      .setDesc('Default lead time in minutes before reminder alerts')
      .addSlider((slider) =>
        slider
          .setLimits(5, 120, 5)
          .setValue(this.plugin.settings.defaultReminderMinutes)
          .onChange(async (val) => {
            this.plugin.settings.defaultReminderMinutes = val
            await this.plugin.saveData(this.plugin.settings)
          })
      )

    new Setting(containerEl)
      .setName('Enable Alert Sounds')
      .setDesc('Play an audio chime when a reminder is triggered')
      .addToggle((toggle) =>
        toggle
          .setValue(this.plugin.settings.soundEnabled)
          .onChange(async (val) => {
            this.plugin.settings.soundEnabled = val
            await this.plugin.saveData(this.plugin.settings)
          })
      )
  }
}

// 5. Main Plugin Class
export default class NoteReminderPlugin extends Plugin {
  id = 'community.note-reminders'
  name = 'Note Reminders'
  settings: NoteReminderSettings = DEFAULT_SETTINGS

  override async onLoad(): Promise<void> {
    // 1. Load Settings
    const loaded = await this.loadData<NoteReminderSettings>()
    this.settings = Object.assign({}, DEFAULT_SETTINGS, loaded)

    // 2. Register Settings Tab
    this.app.ui.registerSettingTab(new ReminderSettingTab(this.app, this))

    // 3. Register Custom Workspace View
    this.registerView('reminder-view', (leaf) => new ReminderView(leaf), {
      icon: 'BellIcon',
      title: 'Reminders',
    })

    // 4. Left Ribbon Action
    this.addRibbonIcon('BellIcon', 'Open Reminders', () => {
      this.app.workspace.revealView('reminder-view', 'left')
    })

    // 5. Command Palette Action
    this.addCommand({
      id: 'open-reminders',
      label: 'Open Reminders Dashboard',
      shortcut: 'mod+shift+r',
      handler: () => {
        this.app.workspace.revealView('reminder-view', 'main')
      },
    })

    // 6. Context Menu Contribution
    this.registerContextMenu('file-explorer:file', (ctx: { path: string }): MenuItem[] => [
      {
        label: 'Set Reminder for Note...',
        icon: 'BellIcon',
        onSelect: async () => {
          const minutes = await this.app.dialogs.prompt(
            'Remind me in how many minutes?',
            String(this.settings.defaultReminderMinutes),
            'Set Note Reminder'
          )
          if (minutes) {
            this.app.toast.success(`Reminder set for ${ctx.path} in ${minutes}m!`)
          }
        },
      },
    ])

    // 7. Status Bar Item
    this.app.statusBar.registerItem({
      id: 'reminders-indicator',
      alignment: 'right',
      priority: 20,
      icon: 'BellIcon',
      text: '0 due',
      onClick: () => {
        this.app.workspace.revealView('reminder-view', 'left')
      },
    })
  }
}
```
