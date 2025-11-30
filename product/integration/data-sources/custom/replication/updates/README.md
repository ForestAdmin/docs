In the [minimal example](../../replication/README.md#minimal-example), we've seen that data was loaded once, never to be updated again.

This is unlikely to be the case in a real-world scenario, as you may want your Forest Admin agent to display up-to-date data.

Three ways to achieve this are available, they can be used independently or in combination:

- [Refetch all records periodically](./scheduled-rebuild.md)
- [Change polling (using Forest Admin events)](./change-polling.md)
- [Change pushing (using target API events)](./push.md)
