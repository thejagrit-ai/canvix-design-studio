<div align="center">

<img src="public/canvix.svg" width="72" alt="Canvix logo" />

# Canvix

**Design anything. Beautifully.**

A browser-based graphic design tool for social posts, presentations, posters, logos and more.

Created and maintained by [Jagrit Sharma](https://jagrit.tech).

</div>

---

## What is Canvix?

Canvix lets you create social media posts, presentations, posters, logos and
thumbnails with a drag-and-drop editor, a template library and one-click export.
Designs are stored in PostgreSQL and exports are generated in the browser.

## Features

- **Editor** — shapes, lines, graphics, stickers, images, tables, frames and embed
  cards; move, resize, rotate, group, lock and reorder layers with smart guides
- **Typography** — 63 Google Fonts, font pairings, text effects and curved text
- **Templates** — Instagram posts, stories, pitch decks, posters, logos and thumbnails
- **Brand kit** — saved colors, palettes, fonts and logos
- **Rulers, guides, crop and version history**
- **Animations** — page and element animations with preview
- **Export** — PNG, JPG, PDF, SVG and video, current page or all pages
- **Real-time collaboration** — live presence, cursors and comment pins
- **AI tools** *(needs an AI provider)* — Magic Layers, Magic Write, AI Assistant,
  image generation and editing, Magic Resize, translation and photo search
- **AI agents (MCP)** — a Model Context Protocol endpoint so AI assistants can
  create and edit designs
- **Responsive** — works on desktop, tablet and mobile with touch gestures

See [CHANGELOG.md](CHANGELOG.md) for the full release history.

## Quick start

**Prerequisites:** [Node.js 20+](https://nodejs.org) and a [PostgreSQL](https://www.postgresql.org) database.

```bash
npm install

# configure the database
cp .env.example .env     # then set DATABASE_URL to your Postgres connection string
npm run db:push          # create the tables

npm run dev              # → http://localhost:3000
```

To load the built-in templates into the database, run `npm run db:seed` (requires
[Bun](https://bun.sh)) or `npx tsx prisma/seed.ts`.

> The `dev` script runs Next.js with webpack (`next dev --webpack`). Turbopack needs
> Next's native SWC binary, which some Windows machines block through Smart App
> Control / Application Control.

## Environment variables

All variables are server-side only and documented in [.env.example](.env.example).

| Variable | Required | Purpose |
|---|---|---|
| `DATABASE_URL` | yes | PostgreSQL connection string |
| `ZAI_BASE_URL`, `ZAI_API_KEY` | no | AI provider; enables the AI tools |
| `CANVIX_MCP_TOKEN` | no | Enables the MCP endpoint at `POST /api/mcp` |

Everything except the AI tools works without an AI provider: the editor,
templates, exports, collaboration, comments, background removal (runs in the
browser) and color palettes.

## AI agents (MCP)

Set `CANVIX_MCP_TOKEN` and restart the server. Then point any MCP client at the
endpoint:

```json
{
  "mcpServers": {
    "canvix": {
      "url": "https://your-domain.example.com/api/mcp",
      "headers": { "Authorization": "Bearer <CANVIX_MCP_TOKEN>" }
    }
  }
}
```

The server exposes 20 tools for browsing templates, creating and editing designs,
animating pages, commenting and exporting (SVG / PNG / JSON). Agent edits appear
live in any open editor session.

## Tech stack

| Layer | Choice |
|---|---|
| Framework | Next.js 16 (App Router), React 19, TypeScript |
| Styling | Tailwind CSS 4, shadcn/ui |
| Canvas | Konva.js via react-konva |
| State | Zustand |
| Database | Prisma ORM, PostgreSQL (jsonb) |
| Realtime | Postgres event log + Server-Sent Events |

## Project structure

```
src/
├── app/            # Next.js App Router: page shell + REST API routes
├── components/
│   ├── landing/    # marketing page
│   ├── dashboard/  # home screen: presets, templates, recent designs
│   └── editor/     # the design editor (canvas, panels, dialogs)
├── lib/            # AI provider, collaboration, animations, exports, MCP server
├── store/          # Zustand stores
└── hooks/
prisma/             # database schema + seed
```

## License

See [LICENSE](LICENSE).

Canvix is not affiliated with, endorsed by, or connected to Canva.
