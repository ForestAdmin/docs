Custom data sources are the answer to the need to import collections from either your in-house APIs, unsupported databases or SaaS providers.

Forest Admin is built so that it does not need to know the nature of the data source it is speaking to, as long as it exposes a given interface.

That interface is only there to abstract away differences between backends so that they can be used as Forest Admin collections. It was built to allow for the minimal feature set which allows Forest Admin to work.

# Getting started

When creating a custom data source, you can use either the "Query Translation" or "Replication" strategy:

{{#ruby}}

The Replica strategy is only available in [agent-nodejs](https://docs.forestadmin.com/developer-guide-agents-nodejs/data-sources/custom/replication).
If it's important to you, don't hesitate to ask it on our [community forum](https://community.forestadmin.com).

{{/ruby}}

| -                | Replication connector                                                            | Query translation connector                                                         |
| ---------------- | -------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| Recommended for  | APIs with limited capabilities.                                                  | Databases or APIs with advanced query capabilities.                                 |
| How does it work | Maintains a copy of all the data in the target API, and use it for read queries. | The data source translates all Forest Admin queries to the target API in real time. |
| Preconditions    | Target API is capable of exporting all data.                                     | Target API is capable of expressing filters, aggregating data, ...                  |
