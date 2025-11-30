---
title: "Quickstart"
description: "Get your admin panel running in 5 minutes"
---

## How Forest Admin Works

Forest Admin works through an **agent**—a lightweight backend application that sits between your data and the Forest Admin UI.

**What the agent does:**
- Connects to your databases and APIs
- Handles authentication and permissions
- Executes your business logic (Smart Actions, Smart Fields)
- Translates Forest Admin UI requests into database queries

{% hint style="info" %}
**Important:** The agent is the only component that accesses your data. Forest Admin UI never connects directly to your database.
{% endhint %}

## Deployment Options

To install Forest Admin, you need to decide **where to run the agent**:

|  | **Cloud** | **Self-Hosted** | **On-Premise** |
|---|---|---|---|
| **Architecture** | ![Cloud](/assets/architecture-cloud.svg) | ![Self-Hosted](/assets/architecture-self-hosted.svg) | ![On-Premise](/assets/architecture-on-premise.svg) |
| **What** | We host the agent | You host the agent | You host everything |
| **Best for** | Quick POC, startups | Private networks, compliance | Air-gapped, maximum security |
| **Requirements** | Internet-accessible DB | Node.js 18+ or Ruby 3+ | Enterprise license |

## Which deployment is right for you?

Answer these questions to find your best option:


<details>
<summary>Is your database accessible from the internet?</summary>

**Yes** → Consider **Cloud** (easiest setup)

    **No** → Go to next question ↓

</details>


<details>
<summary>Do you need air-gapped deployment (zero external connections)?</summary>

**Yes** → You need **On-Premise**

    **No** → Go to next question ↓

</details>


<details>
<summary>Do you have strict compliance requirements (HIPAA, SOC2, data residency)?</summary>

**Yes** → Choose **Self-Hosted** or **On-Premise**

    **No** → Go to next question ↓

</details>


<details>
<summary>Do you want to manage infrastructure yourself?</summary>

**No, I want it managed** → Choose **Cloud**

    **Yes, I can manage it** → Choose **Self-Hosted**

</details>


{% hint style="info" %}
**Need help choosing?**
- [Contact sales](mailto:sales@forestadmin.com) to discuss your specific requirements
{% endhint %}

## Choose your deployment method


  * [Cloud](/get-started/quickstart-cloud.md) - Perfect for quick evaluation and standard use cases

  * [Self-hosted](/get-started/quickstart-self-hosted.md) - Ideal for private networks and compliance needs

  * [On-premise](/get-started/quickstart-on-premise.md) - Full control and air-gapped deployment


{% hint style="info" %}
Need guidance? Our team is here to help you choose the right deployment option for your needs. [Contact us](mailto:sales@forestadmin.com)
{% endhint %}

---

**Need help?** [Join our community](https://community.forestadmin.com) or [contact sales](mailto:sales@forestadmin.com)
