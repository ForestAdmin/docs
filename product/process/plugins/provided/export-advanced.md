The advanced export plugin allows to implement custom data exports in various formats.

## Installation

```sh
npm install @forestadmin/plugin-export-advanced
```

## Configuration

The plugin can be used directly on the Agent, or a specific Collection.
Using it on the Agent will enable the export for all Collections.

All settings are optional. Not providing them will open a form in the UI to configure the export.

```js
import { addExportAdvanced } from '@forestadmin/plugin-export-advanced';

agent.customizeCollection('accounts', collection => {
  collection.use(addExportAdvanced, {
    /**
     * The name of the export button
     * default: Export <collection name> (advanced)
     */
    actionName: 'Export mailing list',

    /**
     * The name of the export file, should not include the extension
     */
    filename: 'mailing-list',

    /**
     * The format of the export file
     * Supported formats are: '.csv', '.xlsx', '.json'
     */
    format: '.xlsx',

    /**
     * The fields to export.
     * Relations can be exported using the following notation.
     */
    fields: [
      'firstname',
      'lastname',
      'email',
      'address:streetName',
      'address:city',
      'address:country',
    ],
  });
});
```
