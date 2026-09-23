# MetadataCache & Link Graph

The `MetadataCache` subsystem (`app.metadataCache`) maintains real-time in-memory indexes of all YAML frontmatter, markdown headings, AST sections, block identifiers (`^block-id`), transclusion embeds (`![[]]`), `#tags`, backlinks, and wikilink connections across your vault notes.

---

## 1. Querying Note Metadata

Retrieve structured metadata and AST character positions for any note in 0ms:

```ts
const cache = this.app.metadataCache.getFileCache("Projects/Roadmap.md");

if (cache) {
  // 1. YAML Frontmatter properties & token position
  console.log(cache.frontmatter?.status); // 'in-progress'
  console.log(cache.frontmatterPosition); // { start: { line: 1, col: 0, offset: 0 }, end: ... }

  // 2. Headings with exact Pos ranges
  for (const h of cache.headings || []) {
    console.log(
      `H${h.level}: ${h.heading} (lines ${h.position.start.line}-${h.position.end.line})`,
    );
  }

  // 3. Extracted #tags
  for (const t of cache.tags || []) {
    console.log(`Tag: #${t.tag} at offset ${t.position.start.offset}`);
  }

  // 4. Outgoing Wikilinks and Transclusion Embeds
  for (const link of cache.links || []) {
    console.log(`Links to: ${link.link} (${link.original})`);
  }
  for (const embed of cache.embeds || []) {
    console.log(`Embeds note: ${embed.link}`);
  }

  // 5. AST Sections & Block IDs (^id)
  for (const sec of cache.sections || []) {
    console.log(
      `Section [${sec.type}] from line ${sec.position.start.line} to ${sec.position.end.line}`,
    );
    if (sec.id) console.log(`  Targetable Block ID: ^${sec.id}`);
  }

  // 6. Target block references directly
  if (cache.blocks && cache.blocks["my-key-takeaway"]) {
    console.log("Found block:", cache.blocks["my-key-takeaway"].position);
  }
}
```

---

## 2. Querying Tags & Backlinks

```ts
// 1. Get all unique tags across the vault (sorted)
const allTags = this.app.metadataCache.getTags(); // ['architecture', 'ideas', 'roadmap']

// 2. Find all notes tagged with a specific tag (O(1) lookup)
const todoNotes = this.app.metadataCache.getFilesWithTag("#roadmap");
// ['Projects/Roadmap.md', 'Notes/Meeting.md']

// 3. Find all notes that link TO this note (Backlinks)
const backlinks = this.app.metadataCache.getBacklinks("Projects/Roadmap");
// ['Dashboard.md', 'Notes/Weekly.md']
```

---

## 3. Force-Directed Link Graph Dataset

Get the complete 2D/3D graph dataset of nodes and link connections across the vault:

```ts
const graphData = this.app.metadataCache.getGraphData();

console.log(graphData.nodes);
// Array<{ id: string; name: string; type: 'note' | 'tag'; val: number }>

console.log(graphData.links);
// Array<{ source: string; target: string }>
```

---

## 4. Metadata Events

Subscribe to real-time metadata indexing events:

```ts
this.registerEvent(
  this.app.metadataCache.on("changed", (filePath, meta) => {
    console.log("Metadata updated for:", filePath);
  }),
);

this.registerEvent(
  this.app.metadataCache.on("ready", () => {
    console.log("Initial metadata cache indexing complete!");
  }),
);
```

---

## 5. Next Steps

- Understand how tabs and splits work in **[[Workspace and leaves|Workspace & Layout Tree]]**.
- Learn about the central **[[Events and lifecycle|Event Bus & Subscriptions]]**.
