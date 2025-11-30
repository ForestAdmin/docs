---
title: 'Introduction'
description: 'Example section for showcasing API endpoints'
---

{% hint style="info" %}
  If you're not looking to build API reference documentation, you can delete
  this section by removing the api-reference folder.
{% endhint %}

## Welcome

There are two ways to build API documentation: [OpenAPI](https://docs.gitbook.com) and [MDX components](https://docs.gitbook.com). For the starter kit, we are using the following OpenAPI specification.

* [Plant Store Endpoints](#) - View the OpenAPI specification file

## Authentication

All API endpoints are authenticated using Bearer tokens and picked up from the specification file.

```json
"security": [
  {
    "bearerAuth": []
  }
]
```
