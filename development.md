---
title: 'Development'
description: 'Preview changes locally to update your docs'
---

{% hint style="info" %}
  **Prerequisites**:
  - Node.js version 19 or higher
  - A docs repository with a `docs.json` file
{% endhint %}

Follow these steps to install and run GitBook on your operating system.

<Steps>
<Step title="Install the GitBook CLI">

```bash
npm i -g @gitbook/cli
```
</Step>

<Step title="Preview locally">

Navigate to your docs directory where your `docs.json` file is located, and run the following command:

```bash
gitbook serve
```

A local preview of your documentation will be available at `http://localhost:3000`.

</Step>
</Steps>

## Custom ports

By default, GitBook uses port 3000. You can customize the port GitBook runs on by using the `--port` flag. For example, to run GitBook on port 3333, use this command:

```bash
gitbook serve --port 3333
```

If you attempt to run GitBook on a port that's already in use, it will use the next available port:

```md
Port 3000 is already in use. Trying 3001 instead.
```

## GitBook versions

Please note that each CLI release is associated with a specific version of GitBook. If your local preview does not align with the production version, please update the CLI:

```bash
npm mint update
```

## Validating links

The CLI can assist with validating links in your documentation. To identify any broken links, use the following command:

```bash
mint broken-links
```

## Deployment

If the deployment is successful, you should see the following:

<Frame>
  <img src="/images/checks-passed.png" alt="Screenshot of a deployment confirmation message that says All checks have passed." style={{ borderRadius: '0.5rem' }} />
</Frame>

## Code formatting

We suggest using extensions on your IDE to recognize and format MDX. If you're a VSCode user, consider the [MDX VSCode extension](https://marketplace.visualstudio.com/items?itemName=unifiedjs.vscode-mdx) for syntax highlighting, and [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) for code formatting.

## Troubleshooting


<details>
<summary>Details</summary>

This may be due to an outdated version of node. Try the following:
    1. Remove the currently-installed version of the CLI: `npm remove -g mint`
    2. Upgrade to Node v19 or higher.
    3. Reinstall the CLI: `npm i -g @gitbook/cli`

</details>


<details>
<summary>Issue: Encountering an unknown error</summary>

Solution: Go to the root of your device and delete the `~/.gitbook` folder. Then run `gitbook serve` again.

</details>


Curious about what changed in the latest CLI version? Check out the [CLI changelog](https://docs.gitbook.com/product-tour/git-sync?activeTab=versions).
