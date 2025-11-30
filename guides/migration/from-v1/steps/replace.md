When agents start, Forest Admin knows which project and environment they are linked to thanks to the `envSecret` configuration key.

By bringing the old configuration to your newly generated codebase, you will link this new codebase to your existing environment (eg. Production).

Notably:

- `authSecret` so that issued authentication tokens stay valid.
- `envSecret` to change the project and environment the agent is linked to.
- `prefix` and `port` so that the new agent is mounted on the same URL as the old one.

Your project will then be migrated to the new agent, and you can delete both:

- The old agent code from your source tree
- The temporary environment that you created.
