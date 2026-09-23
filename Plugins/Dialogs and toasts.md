# Modals, Banners & Toasts

Resin provides three distinct notification and modal layers:

1. **Toasts (`app.toast`)**: Bottom notifications for transient feedback.
2. **Banners (`app.banners`)**: Top progress banners for long-running jobs.
3. **Dialogs (`app.dialogs`)**: Layered accessible modals, confirmations, and user prompts.

---

## 1. Toast Notifications (`app.toast`)

```ts
// Quick toast shortcuts
this.app.toast.success("Note exported successfully!");
this.app.toast.info("Plugin enabled.");
this.app.toast.warning("Note has uncommitted changes.");
this.app.toast.error("Failed to parse YAML frontmatter.");

// Async Promise Toast (Loading -> Success/Error)
await this.app.toast.promise(syncVaultWithServer(), {
  loading: "Uploading vault changes...",
  success: (result) => `Synced ${result.count} files!`,
  error: (err) => `Sync failed: ${err.message}`,
});
```

---

## 2. Top Notification Banners (`app.banners`)

Banners stick to the top of the workspace for persistent status or multi-step operations:

```ts
const banner = this.app.banners.show({
  title: "Vault Indexing in Progress",
  description: "Scanning 12,000 notes and wikilinks...",
  type: "info",
  progress: 45, // 0 to 100
  actions: [
    {
      label: "Cancel",
      onClick: () => banner.dismiss(),
    },
  ],
});

// Dynamically update progress
banner.update({ progress: 90, description: "Almost done..." });

// Dismiss when finished
banner.dismiss();
```

---

## 3. Modal Dialogs & Prompts (`app.dialogs`)

### Standard Alerts and Prompts

```ts
// 1. Simple Alert
await this.app.dialogs.alert("Operation completed successfully!", "Success");

// 2. Confirmation Modal
const confirmed = await this.app.dialogs.confirm(
  "Are you sure you want to reset all plugin settings to defaults?",
  "Reset Settings",
  {
    confirmLabel: "Reset",
    cancelLabel: "Cancel",
    isDanger: true,
  },
);

if (confirmed) {
  // perform reset
}

// 3. User Input Prompt
const folderName = await this.app.dialogs.prompt(
  "Enter new folder name:",
  "Untitled Folder",
  "New Folder",
);

if (folderName) {
  await this.app.vault.createFolder(folderName);
}
```

---

## 4. Next Steps

- Register ribbon icons and actions in **[[Ribbon and commands|Ribbon & Command Palette]]**.
- Manage persistent plugin data in **[[Persistent storage|Isolated Persistent Storage]]**.
