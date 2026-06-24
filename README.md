# Forest Admin Documentation

This repository contains the source for the official Forest Admin documentation.

📖 **Read the docs at [docs.forest.app](https://docs.forest.app)**

## About Forest Admin

Forest Admin is an internal tool platform that gives your teams a ready-to-use admin panel on top of your databases and APIs. Connect your data sources, and get a powerful back-office to view, search, and edit your data, run business operations, and build custom workflows — all while keeping fine-grained control over permissions.

This documentation covers everything you need to install, configure, customize, and operate your Forest Admin project.

## Local development

This documentation is built with [Mintlify](https://mintlify.com).

**Prerequisites:** Node.js 18 or later.

1. Install the Mintlify CLI:

   ```bash
   npm i -g mint
   ```

2. From the repository root (where `docs.json` lives), start the dev server:

   ```bash
   mint dev
   ```

   The site is served at [http://localhost:3000](http://localhost:3000) with hot reload on `.mdx` edits. Use `mint dev --port <port>` if 3000 is taken.

3. Keep the CLI up to date:

   ```bash
   npm i -g mint@latest
   ```

To catch broken internal links before opening a PR:

```bash
mint broken-links
```

## Resources

- Documentation — [docs.forest.app](https://docs.forest.app)
- Website — [forest.app](https://www.forest.app)
