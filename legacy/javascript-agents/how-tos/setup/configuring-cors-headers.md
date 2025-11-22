---
title: Configuring CORS headers
---
Depending on how you've setup your app, you may encounter a [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) error. It will look like this in your browser console:

![](/images/legacy/javascript-agents/cors.png)

In this case, you need to configure the right CORS headers to **allow the domain** `app.forestadmin.com` to trigger an API call on your Application URL, which is a different domain name (e.g. localhost:3000 on development).

We use the [Rack CORS](https://github.com/cyu/rack-cors) Gem for this purpose.

