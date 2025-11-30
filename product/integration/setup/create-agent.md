<details>
<summary><strong>const { createAgent } = require('@forestadmin/agent');</strong></summary>

```javascript
const agent = createAgent({
  // Mandatory options (those will be provided during onboarding)
  authSecret: process.env.FOREST_AUTH_SECRET,
  envSecret: process.env.FOREST_ENV_SECRET,
  isProduction: process.env.NODE_ENV === 'production',

  // Optional variables
  customizeErrorMessage: ...,
  forestServerUrl: ...,
  logger: ...,
  loggerLevel: ...,
  permissionsCacheDurationInSeconds: ...,
  prefix: ...,
  schemaPath: ...,
  typingsMaxDepth: ...,
  typingsPath: ...,
  maxBodySize: ...,
});
```

</details>

<details>
<summary><strong>ForestAdminRails.configure do |config|</strong></summary>

```ruby
  # Mandatory options (those will be provided during onboarding)
  config.auth_secret = 'YOUR_AUTH_SECRET'
  config.env_secret = 'YOUR_ENV_SECRET'
  config.is_production = false

  # Optional variables
  config.customize_error_message = ...
  config.logger = ...
  config.logger_level = ...
  config.permissions_cache_duration_in_seconds = ...
  config.prefix = ...
  config.schema_path = ...
end
```

</details>

{{/ruby}}

## Mandatory variables

All mandatory variables are provided as environment variables during onboarding.

Your agent cannot be started without them, and no default values are provided.

### {{#nodejs}}`authSecret`{{/nodejs}}{{#ruby}}`auth_secret`{{/ruby}} (string, no default)

This variable contains a random secret token which is used to sign authentication tokens used in request between your users and your agent.

It is generated during onboarding, but never leaves your browser, and is not saved on our side.

Never share it to anybody, as that would allow attackers to impersonate your users on your agent!

### {{#nodejs}}`envSecret`{{/nodejs}}{{#ruby}}`env_secret`{{/ruby}} (string, no default)

This variable contains a random secret token which is used to authenticate requests between your agent and our servers.

Unlike the {{#nodejs}}`authSecret`{{/nodejs}}{{#ruby}}`auth_secret`{{/ruby}}, it is stored in our database, so it can be **privately** shared with Forest Admin employees.

Never share it publicly, as it would allow attackers to impersonate your agent with our servers. That would not cause any data leak, but opens the possibility for attackers to cause denial of service.

### {{#nodejs}}`isProduction`{{/nodejs}}{{#ruby}}`is_production`{{/ruby}} (boolean, no default)

In development mode the agent has a few extra behaviors (when using {{#nodejs}}`isProduction: false`{{/nodejs}}){{#ruby}}`is_production = false`{{/ruby}})

- At startup, the agent will print the URL of all mounted charts
- At startup, the agent will update the `.forestadmin-schema.json`{{#nodejs}} and [typings](./autocompletion-and-typings.md) files{{/nodejs}}.
- When exceptions are thrown, a report will be printed to stdout.

## Optional variables

### {{#nodejs}}`customizeErrorMessage`{{/nodejs}}{{#ruby}}`customize_error_message`{{/ruby}} (function, defaults to {{#nodejs}}null{{/nodejs}}{{#ruby}}nil{{/ruby}})

When unexpected errors are raised in the agent code during a request, the error will be logged (using {{#nodejs,ruby}}`options.logger`{{/nodejs,ruby}}), but in the admin-panel, the final user will get a default message 'Unexpected error'.

This is done as to:

- Prevent error message from leaking internal information about the agent (credentials, ...).
- Prevent technical/cryptic error messages to show in the frontend.

This behavior can be customized.

```javascript
createAgent({
  // ...
  customizeErrorMessage: error => {
    if (error instanceof SequelizeConectionRefusedError) {
      return (
        'Failed to connect to the database, ' +
        'contact John at 06 12 34 56 78 and tell him to reboot the server'
      );
    }

    return (
      'Unexpected error, ' +
      'contact Jane at 06 87 65 43 21 and tell her to get it fixed.'
    );
  },
});
```


### {{#nodejs}}`forestServerUrl`{{/nodejs}}{{#ruby}}`forest_server_url`{{/ruby}} (string, defaults to 'https://api.forestadmin.com') <!-- markdown-link-check-disable-line -->

This variable should be used only for customers using [the self-hosted version of Forest Admin](https://www.forestadmin.com/self-hosted).

It allows to specify the URL at which Forest Admin servers can be reached.

```javascript
createAgent({
  // ...
  forestServerUrl: 'https://api.forestadmin.com',
});
```


### `logger` (function) and {{#nodejs,ruby}}`loggerLevel`{{/nodejs,ruby}} (string, defaults to {{#nodejs,ruby}}'Info'{{/nodejs,ruby}})

{{#nodejs}}Forest Admin encourages customers to use [In-app installations](./README.md#standalone-vs-in-app-installation).{{/nodejs}}

You may want to have control of the logger which is used by Forest Admin.

This configuration key allows to format and route logs to a logging service, instead of printing them in stdout.

<details>
<summary><strong>createAgent({</strong></summary>

```javascript
  // ...
  loggerLevel: 'Info', // Valid values are 'Debug', 'Info', 'Warn' and 'Error'
  logger: (logLevel, message) => {
    console.error(logLevel, message);
  },
});
```

</details>

<details>
<summary><strong>ForestAdminRails.configure do |config|</strong></summary>

```ruby
  # ...
  config.logger_level = 'Info' # Valid values are 'Debug', 'Info', 'Warn' and 'Error'
  config.logger = -> (log_level, message) { puts "#{log_level}: #{message}" }
end

```

</details>


### {{#nodejs}}`permissionsCacheDurationInSeconds`{{/nodejs}}{{#ruby}}`permissions_cache_duration_in_seconds`{{/ruby}} (number, defaults to 15 minutes)

Forest Admin administrators can [restrict operations which final users can perform](https://docs.forestadmin.com/user-guide/project-settings/teams-and-users).

Those permissions are enforced both in the frontend, and in your agent.

This configuration variable allows to customize how often the agent should ask the server to provide the permissions table.

<details>
<summary><strong>createAgent({</strong></summary>

```javascript
  // ...
  permissionsCacheDurationInSeconds: 15 * 60,
});
```

</details>

<details>
<summary><strong>ForestAdminRails.configure do |config|</strong></summary>

```ruby
  # ...
  config.permission_expiration = 15 * 60
end
```

</details>

### `prefix` (string, default to empty string)

This variable adds a prefix to the url at which **routes are locally mounted** on your application.
It is mostly used for customers which wish to mount multiple agent instances on the same Node.js process (for setups using multiple Forest Admin projects).

Note that this variable has **no influence** on the base URL that will be used by your users to reach the agent: it is determined only by the **application URL** provided during onboarding and deployment.

![Application URL during onboarding](../../assets/onboarding-application-url.png)

This is done so that customers using reverse proxies can implement their routing table as they see fit.

<!-- markdown-link-check-disable -->

| Desired Local URLs                        | Desired Public URLs                          | How to configure your agent                                                   |
| ----------------------------------------- | -------------------------------------------- | ----------------------------------------------------------------------------- |
| http://localhost:3000/forest              | https://api.company.com/forest               | prefix = ''<br>agentUrl = 'https://api.company.com'                           |
| http://localhost:3000/forest              | https://www.company.com/api/forest           | prefix = ''<br>agentUrl = 'https://www.company.com/api'                       |
| http://localhost:3000/prefix/forest       | https://api.company.com/prefix/forest        | prefix = 'prefix'<br>agentUrl = 'https://api.company.com/prefix'              |
| http://localhost:3000/local-prefix/forest | https://api.company.com/public-prefix/forest | prefix = 'local-prefix'<br>agentUrl = 'https://api.company.com/public-prefix' |

<!-- markdown-link-check-enable -->

<details>
<summary><strong>createAgent({</strong></summary>

```javascript
  // ...
  prefix: 'api',
});
```

</details>

<details>
<summary><strong>ForestAdminRails.configure do |config|</strong></summary>

```ruby
  # ...
  config.prefix = 'api'
end
```

</details>

### {{#nodejs}}`schemaPath`{{/nodejs}}{{#ruby}}`schema_path`{{/ruby}} (string, defaults to '.forestadmin-schema.json')

This variable allows to choose where the `.forestadmin-schema.json` file should be written in development, and read from in production.


This allows to:

- Improve git repository organisation
- Work around read only folders (for instance, if developing using a read only docker volume).
- Have flexibility when using custom builds in production (code minification, ...)

<details>
<summary><strong>createAgent({</strong></summary>

```javascript
  // ...
  schemaPath: '/volumes/fa-agent-configuration/schema.json',
});
```

</details>

<details>
<summary><strong>ForestAdminRails.configure do |config|</strong></summary>

```ruby
  # ...
  config.schema_path = '/volumes/fa-agent-configuration/schema.json'
end
```

</details>

{{#nodejs}}

### `typingsPath` (string, defaults to null), `typingsMaxDepth` (number, defaults to 3)

This variable allows to choose where the [typing file](./autocompletion-and-typings.md) should be written in development.

```typescript
import { createAgent } from '@forestadmin/agent';
import { Schema } from './typings';

createAgent<Schema>({
  // ...
  typingsPath: './typings.ts',
  typingsMaxDepth: 5,
});
```

### `maxBodySize` (string default to '50mb')

This variable allows to increase the request body limit size.

To be used when you want to handle big files on smart action and so on.

{{/nodejs}}
