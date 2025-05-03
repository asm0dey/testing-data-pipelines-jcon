# Testing Data Pipelines

This repository contains the slides and code examples for the "Himalayan Peaks of Testing Data Pipelines" presentation by Ksenia Tomak and Pasha Finkelshteyn.

## Prerequisites
This project uses [slidev](https://sli.dev/) to create presentations using markdown. Make sure to have bun installed

## Bun installation

```bash
curl -fsSL https://bun.sh/install | bash
```

Now you can use the bun command line tool.

## Installation

```bash
# install dependencies
bun install
```

## Run the slides in Web Browser

```bash
# start Slidev in the current directory
bun run dev
```
Your presentation will be available at `localhost:3030`.

## Building
Use the `bun run build` command to build the slides to `dist`.

## Building for GitHub Pages

If you want to publish your slides to GitHub Pages, use the `bun run build-gha` command.

## Exporting Slides

```bash
# export to PDF
bun run export-pdf
# or to static HTML + assets
bun run export
```

## Credits

This slide setup uses [@slidev/cli](https://github.com/slidevjs/cli), [@slidev/theme-default](https://github.com/slidevjs/themes/tree/main/packages/theme-default) and [@slidev/theme-seriph](https://github.com/slidevjs/themes/tree/main/packages/theme-seriph) for better looks and features.

## License

License is [MIT](./LICENSE).

## Contributors

- Ksenia Tomak
- Pasha Finkelshteyn
