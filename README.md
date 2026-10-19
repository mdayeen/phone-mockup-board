# phone-mockup-board

An Agent Skill that builds a **clickable comparison board of phone mockups**. Several phone frames sit side by side on one HTML page, and each one is a small working app:

- Every phone scrolls inside its own frame.
- Chips, tiles and tabs actually filter and switch.
- Tapping any item opens it in a detail phone, and the board scrolls there.
- A stepper rescales values live (servings, quantity, nights…).
- A colour-shade picker recolours every phone at once, which helps while the client hasn't chosen the brand colour.
- Each phone has a caption, and the footer marks what is sample content.

It's made for design reviews: open the page, record a Loom or screen walkthrough, and send it to the client. The client doesn't need Figma, a login or an install.

## What's in the repo

```
phone-mockup-board/
├── .claude-plugin/
│   ├── plugin.json                 # Claude Code plugin manifest
│   └── marketplace.json            # lets people install with one command
├── skills/phone-mockup-board/
│   ├── SKILL.md                    # instructions the agent follows
│   ├── assets/board-template.html  # working starter board (edit the CONFIG object)
│   ├── references/screen-patterns.md
│   └── LICENSE
├── LICENSE                         # MIT
└── README.md
```

The output is one self-contained `index.html` plus an `img/` folder. It has no build step, so double-click it to open.

## Install

### Claude Code (plugin, recommended)

```bash
claude plugin marketplace add mdayeen/phone-mockup-board
claude plugin install phone-mockup-board@mdayeen
```

Inside a session, `/plugin marketplace add mdayeen/phone-mockup-board` works too. The skill then runs automatically when you ask for phone mockups, or as `/phone-mockup-board:phone-mockup-board`.

### Claude Code (plain skill)

Copy `skills/phone-mockup-board/` to `~/.claude/skills/phone-mockup-board/` for all projects, or to `.claude/skills/phone-mockup-board/` for one project.

### Claude.ai / Claude Desktop

Zip the `skills/phone-mockup-board/` folder and upload it under Settings → Capabilities → Skills.

### Other agents that read SKILL.md

The skill uses the open Agent Skills format: a folder with a `SKILL.md` that has YAML frontmatter. Put `skills/phone-mockup-board/` wherever your tool loads skills from.

## Use

Describe the project, attach a sketch or reference screenshots if you have them, and ask. For example:

> Make a phone mockup board for **Green Basket**, a grocery delivery app. Primary colour is green but not final, so give me 3 shades. Row 1: three home screen options (search → deals → categories), different layouts, not just recolours. Row 2: product page + cart. No login or loyalty screens, they're out of scope. Light theme, rounded corners.

Things that improve results:

- **Attach images.** A pen sketch or 3–6 reference screenshots beat any description.
- **Name what's out of scope**, so the board doesn't promise features the client hasn't paid for.
- **Say how many options you want and how they should differ.**
- **Say what the stepper should change** (servings, quantity, guests).

## Customising the template

Open `skills/phone-mockup-board/assets/board-template.html` and edit the `CONFIG` object (brand, shades, tabs, categories, items). Then rewrite the render functions `homeA`, `homeB`, `homeC`, `detail` and `list` to fit your flow. Each screen is a plain function that returns an HTML string. One delegated click handler covers every phone through `data-*` attributes:

| Attribute | What it does |
| --- | --- |
| `data-open="id"` | opens an item in the detail phone |
| `data-set="key" data-val="…"` | updates a filter and re-renders that phone |
| `data-tab` | switches the detail tabs |
| `data-qty` | moves the stepper |
| `data-go="screenId"` | scrolls the board to that phone |
| `data-back` | scrolls back to the first home phone |

## What it runs and connects to

- **The skill itself** is plain instructions plus an HTML template. It has no hooks, MCP servers, scripts, package installs or API calls, and it collects, stores or sends no data.
- **The pages it generates** are static HTML files on your machine (or published wherever you choose). By default they load the Figtree font from Google Fonts (`fonts.googleapis.com`, `fonts.gstatic.com`). Any outbound links in a board, such as a WhatsApp share link or a YouTube link, are ones you or the agent put there, and they open only when the viewer clicks them.
- **Shade choice.** A generated board remembers the viewer's colour choice in the browser's `localStorage` on that device only.

## Contributing

Contributions are welcome! `main` is protected, so all changes land through pull requests.
Please read [CONTRIBUTING.md](CONTRIBUTING.md) and our [Code of Conduct](CODE_OF_CONDUCT.md) before opening a PR.

## Security

Please do not report vulnerabilities in public issues. See [SECURITY.md](SECURITY.md).

## License

MIT © 2026 Mohammed Ayeenuddin. See [LICENSE](LICENSE).
