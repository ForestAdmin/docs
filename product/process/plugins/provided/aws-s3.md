The S3 plugin makes it easy to upload files to Amazon S3 from your admin panel.

## Installation

```sh
npm install @forestadmin/plugin-aws-s3
```

## Configuration

It is configurable so that you may choose:

- the permission level (public, private,...),
- where files should be stored,
- if replaced files should either be kept or deleted.

```javascript
import { createAgent } from '@forestadmin/agent';
import { createFileField } from '@forestadmin/plugin-aws-s3';

createAgent().customizeCollection('accounts', collection =>
  collection.use(createFileField, {
    /**
     * Name of the field that you want to use as a file-picker on the frontend.
     * This field should already exist in your database.
     */
    fieldname: 'avatar',

    /**
     * Where should the file be stored on S3?
     * Defaults to '<collection_name>/<field_name>/`.
     * If the objectKeyFromRecord option is not set, the output of that function will
     * also be used as the object key in S3.
     */
    storeAt: (recordId, originalFilename) =>
      `accounts/${recordId}/${originalFilename}`,

    /** Either if old files should be deleted when updating or deleting a record. */
    deleteFiles: false,

    /**
     * 'url' (the default) will cause urls to be transmitted to the frontend. Your
     * final users will download the file from S3.
     *
     * 'proxy' will cause files to be routed by the agent. Use this option only if
     * you are dealing with small files and are behind an entreprise proxy which
     * forbids direct access to S3.
     */
    readMode: 'url',

    /**
     * Which Access Control Level to use on the uploaded objects.
     * Default is "private" (urls will be signed so that the files can be reached
     * from the frontend).
     *
     * Valid values are "authenticated-read", "aws-exec-read",
     * "bucket-owner-full-control", "bucket-owner-read", "private", "public-read",
     * "public-read-write".
     *
     * @see https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/globals.html#objectcannedacl
     */
    acl: 'private',

    /** AWS configuration */
    aws: {
      /** AWS access key, defaults to process.env.AWS_ACCESS_KEY_ID. */
      accessKeyId: 'AKIA.........',

      /** AWS secret, defaults to process.env.AWS_ACCESS_KEY_SECRET. */
      secretAccessKey: '123.......',

      /** AWS region, defaults to process.env.AWS_DEFAULT_REGION. */
      region: 'eu-west-1',

      /** AWS bucket, defaults to process.env.AWS_S3_BUCKET */
      bucket: 'my-bucket',
    },

    /**
     * Allow to customize the key of the object stored in S3, without interfering
     * with what is stored in the database.
     *
     * Default to null - The key object will be the same as the one stored in the
     * database.
     *
     * @example
     * objectKeyFromRecord: {
     *   extraDependencies: ['firstname', 'lastname'],
     *   mappingFunction: (record, context) => {
     *     return `avatars/${record.firstname}-${record.lastname}.png`;
     *   }
     * };
     */
    objectKeyFromRecord: null,
  }),
);
```
