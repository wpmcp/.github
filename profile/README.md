# WP MCP

WP MCP turns a WordPress site into an MCP server, built on the official WordPress Abilities API, so Claude, Cursor, or any other MCP client can build pages, edit content, and configure the site. Every write is snapshot-backed, so the agent physically cannot make an unrecoverable change.

## The safety model is the point

- **Snapshot before every write.** Mutating tools go through one safety layer that captures the affected data first, then mutates. There is no bypass path.
- **One-click rollback.** Restore any snapshot from the History screen, or roll back everything an agent did in a whole session.
- **Governance that can only narrow.** Six permission layers, each of which can restrict but never widen what an agent is allowed to do.
- **Audit log.** Every tool call and every governance decision is recorded.
- **Scoped identities.** Give each agent exactly the capabilities it needs, nothing more.
- **Auth.** OAuth 2.1 with PKCE, or WordPress application passwords.
- **Escape hatches stay shut.** Guarded WP-CLI and guarded PHP execution ship disabled by default and run in development environments only.

Snapshots are fine-grained, per-operation undo. They are not a replacement for a full-site backup plugin.

## What agents can do

200+ abilities, grouped by what they touch:

| Area | Coverage |
|---|---|
| Content | Posts, pages, custom post types, taxonomies, menus, users, options, media library, stock image imports |
| Gutenberg | Surgical block edits, full page composition, data-driven custom block building |
| Elementor | Widgets, containers, templates, theme builder, popups, dynamic tags, global styles, atomic v4 elements, custom widget building |
| Other builders | Bricks and Divi structural editing |
| WooCommerce | Products, orders, and store settings |
| SEO | Yoast, Rank Math, SEOPress, The SEO Framework, SureRank |
| Forms and data | Gravity Forms, WPForms, Contact Form 7, Formidable, Ninja Forms, Fluent Forms (read) |
| Custom fields | ACF and Meta Box, read and write |
| Events, donations, memberships | Modern Events Calendar, The Events Calendar, Give, Paid Memberships Pro (read) |
| Everything else | REST passthrough, still snapshotted |

Integration pages state plainly what each one does not cover. Read-only integrations are read-only because form and table entries are not snapshottable yet, not because the work was skipped.

## Repositories

| Repo | What it is |
|---|---|
| [wpmcp](https://github.com/wpmcp/wpmcp) | The plugin. GPL-2.0-or-later, WordPress 6.9+, PHP 8.1+. This is where issues and pull requests go. |
| [wpmcp-marketing](https://github.com/wpmcp/wpmcp-marketing) | Source for wpmcp-pro.com: the marketing site, integration pages, and guides. |
| [.github](https://github.com/wpmcp/.github) | This repo. Org profile and the org-wide issue, pull request, contributing, and security templates. |

## Start here

- Site and docs: [wpmcp-pro.com](https://wpmcp-pro.com/) and [the docs](https://wpmcp-pro.com/docs.html)
- Guides and integrations: [guides](https://wpmcp-pro.com/guides.html), [integrations](https://wpmcp-pro.com/integrations.html)
- Install the free build: [wpmcp.zip from the latest release](https://github.com/wpmcp/wpmcp/releases/latest/download/wpmcp.zip), then follow the connection wizard in the WP MCP admin menu
- Report a bug or request a feature: [wpmcp/wpmcp issues](https://github.com/wpmcp/wpmcp/issues)
- Paid tier: [pricing](https://wpmcp-pro.com/pricing.html). Pro adds unlimited snapshot history, deep Elementor editing, the widget and block builders, and cloud sync. It does not add safety. The safety core is free.

Built on the [WordPress Abilities API](https://developer.wordpress.org/apis/abilities-api/) and the [Model Context Protocol](https://modelcontextprotocol.io/).
