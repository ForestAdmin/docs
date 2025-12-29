## Cross-origin resource sharing (CORS)

This is the most common issue you can encounter while setting up the Forest Admin Agent.
Opening the Developer Console of your browser will help to detect this kind of issue. As Forest Admin provides a complete middleware, please make sure:

- That the Forest Admin Agent is mounted before any other middleware (Especially in a NestJS context, as the Nest App Factory allows configuring CORS on creation, and it can interfere with the `@forestadmin/agent` package).
- That your server is up and running. This can be easily checked by using the `/forest` endpoint.

## Invalid permissions

When adding users to your project, creating new collections or other permission based component. You might experience `Forbidden (403)` errors, although everything is properly configured in your settings. And only a server restart fixes your issue.

This is most commonly due to how Forest Admin keeps your agent up to date with the permission cache. On the latest versions of our agents we use `ServerSentEvent (SSE)`. This allows our server to notify your agent as soon as a change is made to ensure the latest permissions possible.

Even though everything should be configured properly to work without action on your side, we have reports that some reverse proxy will ignore, instruction headers to disable buffering on our `SSE` endpoint.

To ensure that the issue you are facing is caused by `SSE` buffering, try to restart your agent. If you are no longer facing a permission error from the agent this is indeed the cause.

To fix the issue long term, you can either:

- Disable buffering on your reverse proxy. Here is an example on `nginx` proxy configuration on how to do so:

```
fastcgi_buffering off;
proxy_buffering off;
```

- Disable the `SSE` mechanism and fallback to old TTL cache by setting `instantCacheRefresh: false` in your agent init options.

## Help us get better!

Finally, when your local server is started, you should be automatically redirected to a satisfaction form. Rate us so we can improve, then go to your newly created admin panel

#### ❓ don't you see an answer to your problem?

Describe it on our [Developer Community Forum](https://community.forestadmin.com) and we will answer quickly.
