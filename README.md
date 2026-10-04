# The Kitchen

A home for the games, worlds, and tools made by **TechInTheKitchen**. The Kitchen is a static project gallery adapted from [Obsidian GitHub Web Hosting](https://github.com/TechInTheKitchen/Obsidian-GitHub-Web-Hosting), with a charcoal-and-brass theme, a paper-style light theme, and a frying-pan site icon.

Each project gets a clickable image card with a short description. Search and category filters make the collection easy to browse as it grows. The layout adapts to desktop and mobile screens, and the visitor's theme preference is remembered in their browser.

No npm packages, external fonts, account, or backend service are required. Node.js is used only to build the project index and run the local preview.

## Preview locally

1. Install Node.js if it is not already available.
2. Double-click **[Open Local Site.cmd](Open%20Local%20Site.cmd)** in the project root.
3. The launcher rebuilds the project index and opens the site in your browser. Keep the server window open while testing; close it to stop.

The preview binds to `127.0.0.1` and uses the first available port from `8765` to `8775`. Project cards still open their configured hosted URLs.

To start without opening a browser, run this command from the project folder:

```sh
node tools/local-server.cjs --no-open
```

Opening `index.html` directly from Explorer will not load the project index correctly. Use the local launcher instead.

## Add or edit a project

Every entry lives in its own folder:

```text
projects/
  my-project/
    project.json
    description.txt
    image.webp
```

1. Duplicate an existing folder under `projects` and give the copy a unique name.
2. Edit `project.json` with the project's title, destination, and display settings.
3. Add your image to that folder and update the `image` filename.
4. Write a short plain-text summary in `description.txt`.
5. Run **[Update Project Index.cmd](tools/Update%20Project%20Index.cmd)**, then refresh the preview. Restarting the local launcher also rebuilds the index.

Example `project.json`:

```json
{
  "title": "My Project",
  "category": "Tabletop games",
  "url": "https://example.com/my-project/",
  "image": "image.webp",
  "imageFill": true,
  "color": "#365663",
  "label": "A new adventure",
  "action": "Explore the project",
  "order": 5,
  "hidden": false
}
```

| Field | Purpose |
| --- | --- |
| `title` | Required. Project name beneath the image. |
| `category` | Required. Card category and automatic filter button. Use the same spelling to group projects. |
| `url` | Required. Complete destination URL beginning with `https://` or `http://`. |
| `image` | Required. Image path inside this entry's folder. SVG, PNG, WebP, and JPG are supported. |
| `imageFill` | `true` fills the inner frame; `false` or omission displays a centered icon. Use a JSON boolean, not a quoted string. |
| `color` | Optional six-digit hex color for the banner glow, such as `#365663`. |
| `label` | Optional text at the bottom-right of the image panel. Defaults to the category. |
| `action` | Optional button text. Defaults to “Explore project.” |
| `order` | Optional number; lower values appear first. Entries without it use 999. Ties sort by title. |
| `credit` | Optional plain-text credit beneath the button text. |
| `hidden` | Set to `true` to leave an entry out of the published index. |

### Image display

With `"imageFill": true`, the image fills the innermost outlined box using a centered crop. It is not stretched. The colored outer margin, outline, file number, and caption stay visible, with shading behind the overlaid text to aid readability. Wide images work best; keep essential artwork away from the edges because cropping changes with the screen width.

With `"imageFill": false`, the image remains a small, centered, uncropped icon against the colored background.

### Index and drafts

The builder collects entries into `assets/projects.json`. Treat the entry folders as the source of truth; rebuild the index instead of editing the generated file. Missing required fields, unsupported URL protocols, invalid toggle values, missing images, and empty descriptions stop the build with an error.

Folders beginning with `_` or `.` and folders without `project.json` are ignored. Hidden and ignored entries are excluded from the gallery, but their files are **not private** if you upload them to a public repository or website. Keep unpublished material outside the deployment folder.

To build from a terminal:

```sh
node tools/build-index.cjs
```

## Customize the site

| File | What to change |
| --- | --- |
| `index.html` | Page title, introduction, header branding, and footer. |
| `assets/css/palette.css` | Dark and light theme colors. |
| `assets/css/gallery.css` | Card layout, image framing, typography, and responsive rules. |
| `assets/images/site-icon.svg` | Frying-pan header icon and browser favicon. |
| `assets/js/app.js` | Card rendering, search, filters, and theme switching. |
| `projects/*/project.json` | Individual card settings and destination links. |
| `projects/*/description.txt` | Individual project summaries. |

HTML, CSS, and JavaScript changes only need a browser refresh. Entry changes need an index rebuild followed by a refresh. If you replace the site icon and your browser retains the old one, update its version query in both `index.html` references.

## Publish to GitHub Pages

1. Run `tools/Update Project Index.cmd` and resolve any reported errors.
2. Test the gallery locally, including project links and images.
3. Commit the site's files to your hosting repository. Include `index.html`, `assets`, `projects`, and `.nojekyll`, along with the documentation and applicable license notices.
4. Configure GitHub Pages to publish the repository root on your chosen branch.

All gallery asset paths are relative, so the site can be hosted at a domain root or a repository subpath. The published site does not need Node.js or a running local server.

For later updates, rebuild and commit `assets/projects.json` alongside the changed entry files and images. The gallery links to the individual project sites; it does not bundle or deploy those sites.

## Troubleshooting

- **The launcher cannot find Node.js:** install it, then reopen the launcher.
- **An edited card has not changed:** rebuild the index and refresh. Check that you edited the entry folder rather than the generated index.
- **An image will not load:** check its filename, extension, and capitalization against `image`. Hosted paths may be case-sensitive even when Windows paths are not.
- **An image is cropped:** this is expected in fill mode. Use a wider image or set `imageFill` to `false`.
- **An entry is missing:** check `hidden`, the folder name, and the index builder's output.

## License and credits

The Kitchen's website code and documentation use the **[MIT License](LICENSE)**, matching Obsidian GitHub Web Hosting. The license retains **Copyright (c) 2026 TechInTheKitchen**.

The palette, header styling, and local server were adapted from Obsidian GitHub Web Hosting. The frying-pan site icon was created for The Kitchen.

Project artwork, game material, and third-party assets retain their own licenses; the site's MIT license does not replace those terms. Included project notices are available in the [Entrenched](projects/entrenched/LICENSE.md), [The Great Climb](projects/the-great-climb/LICENSE.md), and [cloak\\dagger\\system](projects/cloak-dagger-system/LICENSE.txt) entry folders. The original cloak\\dagger\\system game is by Ruminastro. Keep any additional image credits and source-specific notices with the assets when replacing or adding images.
