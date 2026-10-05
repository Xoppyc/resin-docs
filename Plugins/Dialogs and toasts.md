# Dialogs and Toasts

Resin provides three UI feedback mechanisms:
1. **Toasts (`app.toast`)**: Floating notifications for non-blocking status.
2. **Banners (`app.banners`)**: Pinned header bars for progress tracking.
3. **Dialogs (`app.dialogs`)**: Modals for confirmations and user input.

![[dialogs-toasts-ui.png|Toast notification and confirmation dialog in Resin]]

## Toast Notifications

```ts
// Status shortcuts
this.app.toast.success("Note exported successfully!");
this.app.toast.info("Plugin loaded.");
this.app.toast.warning("File has unsaved changes.");
this.app.toast.error("Failed to connect to server.");

// Async Promise Toast
await this.app.toast.promise(syncVaultWithServer(), {
  loading: "Uploading changes...",
  success: (res) => `Synced ${res.count} notes!`,
  error: (err) => `Sync failed: ${err.message}`,
});
```

## Top Banners

```ts
const banner = this.app.banners.show({
  title: "Indexing Vault",
  description: "Scanning notes and links...",
  type: "info",
  progress: 45,
  actions: [
    {
      label: "Cancel",
      onClick: () => banner.dismiss(),
    },
  ],
});

banner.update({ progress: 90, description: "Finalizing..." });
banner.dismiss();
```

## Modal Dialogs

```ts
// Simple Alert
await this.app.dialogs.alert("Export complete!", "Success");

// Confirmation
const confirmed = await this.app.dialogs.confirm(
  "Reset all plugin settings to defaults?",
  "Reset Settings",
  {
    confirmLabel: "Reset",
    cancelLabel: "Cancel",
    isDanger: true,
  },
);

// Text Prompt
const folderName = await this.app.dialogs.prompt(
  "Enter new folder name:",
  "Untitled Folder",
  "New Folder",
);
```

