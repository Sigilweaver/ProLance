# ProLance documentation site

Docusaurus site for ProLance. Local dev runs on port `25818`,
deployed to `https://sigilweaver.app/prolance/docs/`.

```bash
bun install
bun run dev      # http://localhost:25818
bun run build
bun run deploy   # bun run build:cloudflare && bunx wrangler deploy
```
