---
title: "Interface & branding"
description: "Customize your Forest Admin interface with white-label branding and custom domains"
---

The Interface tab provides white-label customization capabilities to align Forest Admin with your company's brand identity.

{% hint style="info" %}
This feature may not be available on all plans. Check the [pricing page](https://www.forestadmin.com/pricing) for more information, or [contact us](https://www.forestadmin.com/contact) to enable it for your organization.
{% endhint %}

## Accessing interface settings

1. Click your **project logo** in the top-left corner
2. Select **Project Settings**
3. Navigate to the **Interface** tab

## Available customization options

### Project logo

Set your own project logo which will appear in the top-left corner of your Forest Admin interface, replacing the default Forest Admin branding.

**Requirements:**
- Recommended format: PNG or SVG
- Recommended dimensions: 120x40 pixels for optimal display
- File size: Maximum 2MB

### Primary color

Customize the primary color applied throughout the application interface, including:
- Buttons
- Links
- Active states
- Navigation highlights

This allows you to match Forest Admin's color scheme to your company's brand guidelines.

### Custom domain

Instead of accessing your Forest Admin instance through the default `app.forestadmin.com/12345` URL, you can configure a custom domain.

**Examples:**
- `forestadmin.my-company.com`
- `admin.my-company.com`
- Any custom domain you own

#### Setting up a custom domain

1. Choose your desired subdomain or domain
2. Configure DNS settings with your domain provider:
   - Add a CNAME record pointing to the Forest Admin server
   - Specific DNS instructions are provided in the Interface tab
3. Enter your custom domain in the Interface settings
4. Wait for DNS propagation (usually 24-48 hours)
5. Access your Forest Admin instance via your custom domain

{% hint style="info" %}
SSL certificates are automatically provisioned for custom domains to ensure secure connections.
{% endhint %}

## Benefits of white-label customization

- **Brand Consistency** - Maintain your company's visual identity across all tools
- **Professional Appearance** - Present a polished interface to internal teams
- **Custom URLs** - Use memorable, company-specific domain names
- **Client Portals** - Create branded experiences for external users or partners

## Related pages

- [Teams](/product/control/teams) - Create different layouts for different user groups
- [General Settings](/product/project-settings/general) - Configure project name and basic settings
