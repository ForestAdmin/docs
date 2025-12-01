![Fields in Hex and  modes](../../assets/binary-mode.png)

In Forest Admin, binary fields are not handled specially: they are included in the payloads that transit between your agent and the UI like any other field.
To achieve that they are either encoded using the [data-URI scheme](https://en.wikipedia.org/wiki/Data_URI_scheme) or in [hexadecimal representation](https://en.wikipedia.org/wiki/Hexadecimal).

Binary fields in databases are usually either used to store compact data (like a hash or an identifier) or large data (like an image).

To be able to handle both cases, Forest Admin has two distinct modes available.

# Summary

|                 | `hex` mode                                                | `datauri` mode                                         |
| --------------- | --------------------------------------------------------- | ------------------------------------------------------ |
| Best suited for | Compact data (identifiers, hashes, ...)                   | Large data (files)                                     |
| Description     | The binary data is encoded in hexadecimal representation. | The binary data is encoded using the data-URI scheme.  |
| Example         | `0xdeadbeef`                                              | `data:image/png;base64,...`                            |
| UI widget       | Textual representation                                    | File picker / viewer                                   |
| Default mode    | Field is used as either a primary or foreign key          | Field is _not_ used as either a primary or foreign key |

# Switching between modes

Note that as both modes result in a textual representation of the binary data, changing the mode will not affect the widget used in the UI.

You will need to update the widget manually using the [UI customization](https://docs.forestadmin.com/user-guide/collections/customize-your-fields#edit-settings) feature.

## Using the hexadecimal mode

The hexadecimal mode is suitable for all data that you would not save in a file.

It is the default mode for all binary fields that are used as either a primary or foreign key.

If you want to use the hexadecimal mode for another field, use the {{#nodejs}}`replaceFieldBinaryMode`{{/nodejs}}{{#ruby}}`replace_field_binary_mode`{{/ruby}} method:

{% tabs %}
{% tab title="agent.customizeCollection('people', collection =>" %}
```javascript
collection.replaceFieldBinaryMode('avatar', 'hex'),
);
```
{% endtab %}

{% tab title="@create_agent.customize_collection('people') do |collection|" %}
```ruby
collection.replace_field_binary_mode('avatar', 'hex')
end
```
{% endtab %}
{% endtabs %}


## Using the data-URI mode

The data-URI mode is suitable for all data that you would save in a file (images, PDFs, ...).

When using that mode, you will be able to use both the [File Viewer](https://docs.forestadmin.com/user-guide/collections/customize-your-fields/display-widgets#file-viewer) and the [File Picker](https://docs.forestadmin.com/user-guide/collections/customize-your-fields/edit-widgets#file-picker) widgets in the UI to respectively preview and upload files.

If the automatic detection based on the field type is not working for you, you can force the `datauri` mode using the {{#nodejs}}`replaceFieldBinaryMode`{{/nodejs}}{{#ruby}}`replace_field_binary_mode`{{/ruby}} method as so:

{% tabs %}
{% tab title="agent.customizeCollection('people', collection =>" %}
```javascript
collection.replaceFieldBinaryMode('avatar', 'datauri'),
);
```
{% endtab %}

{% tab title="@create_agent.customize_collection('User') do |collection|" %}
```ruby
collection.replace_field_binary_mode('avatar', 'datauri')
end
```
{% endtab %}
{% endtabs %}


