---
title: "Quickstart"
description: "Start building awesome documentation in minutes"
---

## Get started in three steps

Get your documentation site running locally and make your first customization.

### Step 1: Set up your local environment


<details>
<summary>Clone your docs locally</summary>

During the onboarding process, you created a GitHub repository with your docs content if you didn't already have one. You can find a link to this repository in your [dashboard](https://app.gitbook.com).
    
    To clone the repository locally so that you can make and preview changes to your docs, follow the [Cloning a repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) guide in the GitHub docs.

</details>

  
<details>
<summary>Start the preview server</summary>

1. Install the GitBook CLI: `npm i -g @gitbook/cli`
    2. Navigate to your docs directory and run: `gitbook serve`
    3. Open `http://localhost:3000` to see your docs live!
    
    {% hint style="success" %}
Your preview updates automatically as you edit files.
{% endhint %}

</details>


### Step 2: Deploy your changes


<details>
<summary>Install our GitHub app</summary>

Install the GitBook GitHub app from your [dashboard](https://app.gitbook.com).
    
    Our GitHub app automatically deploys your changes to your docs site, so you don't need to manage deployments yourself.

</details>


<details>
<summary>Update your site name and colors</summary>

For a first change, let's update the name and colors of your docs site.

    1. Open `docs.json` in your editor.
    2. Change the `"name"` field to your project name.
    3. Update the `"colors"` to match your brand.
    4. Save and see your changes instantly at `http://localhost:3000`.

    {% hint style="success" %}
Try changing the primary color to see an immediate difference!
{% endhint %}

</details>


### Step 3: Go live


<details>
<summary>Publish your docs</summary>

1. Commit and push your changes.
  2. Your docs will update and be live in moments!

</details>


## Next steps

Now that you have your docs running, explore these key features:




* [Write Content](/essentials/markdown.md) - Learn MDX syntax and start writing your documentation.

* [Customize style](/essentials/settings.md) - Make your docs match your brand perfectly.

* [Add code examples](/essentials/code.md) - Include syntax-highlighted code blocks.

* [API documentation](/api-reference/introduction.md) - Auto-generate API docs from OpenAPI specs.




{% hint style="info" %}
  **Need help?** See our [full documentation](https://docs.gitbook.com) or join our [community](https://community.gitbook.com).
{% endhint %}
