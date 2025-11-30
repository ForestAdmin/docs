## Enable polymorphic support

Forest admin can interact with polymorphic relations by enabling the `polymorphic` option on the datasource instance.

{{#php}}

```php
return static function () {
    $defaultDB = config('database.default');
    $forestAgent = app()->make(AgentFactory::class);

    $forestAgent->addDatasource(
        // set the second parameter to true to enable polymorphic relations
        new EloquentDatasource(config('database.connections.' . $defaultDB), true),
    );
};
```

{{/php}}

```ruby
def self.setup!
  database_configuration = Rails.configuration.database_configuration

  # set the second parameter to true to enable polymorphic relations
  datasource = ForestAdminDatasourceActiveRecord::Datasource.new(database_configuration[Rails.env], true)

  @create_agent = ForestAdminAgent::Builder::AgentFactory.instance.add_datasource(datasource)
  customize
  @create_agent.build
end

def self.customize; end
```

{{#python}}

{% tabs %}
{% tab title="my_project.settings.py" %}

```python
# ...
FOREST_CUSTOMIZE_FUNCTION = "my_app.forest_admin.customize_agent"
FOREST_AUTO_ADD_DJANGO_DATASOURCE = False
```

{% endtab %}{% tab title="my_app.forest_admin.py" %}

```python
from forestadmin.datasource_django.datasource import DjangoDatasource
from forestadmin.django_agent.agent import DjangoAgent


def customize_forest(agent: DjangoAgent):
    agent.add_datasource(DjangoDatasource(support_polymorphic_relations=True))

```

{% endtab %}
{% endtabs %}

Because Django usea a foreign key to `django_content_type` and Forest requires a string field for the type field, an enum field will automatically be created to replace the relationship to `django_content_type`. This enum field will have enum one entry per reverse relation declared with `django.contrib.contenttypes.fields.GenericRelation`.

{{/python}}

## Tips

The `id field` and the `type field`{{#python}}(along with the foreign key to `django_content_type`){{/python}} of the relationship are still shown. You can use them to create segment, [hide them from the agent](../agent-customization/fields/import-rename-remove.md#renaming-and-removing-fields-and-relations) or through the front-end configuration.

## Limitations

### Collections

#### Remove collection

When using the [remove collection customization](../datasources/getting-started/partial-imports.md), it's important to note a limitation regarding collections that are potential targets of polymorphic relationships.
If you attempt to delete a collection that is referenced in a polymorphic relationship, an exception will be thrown with the following message:

```
Cannot remove {CollectionName} because it's a potential target of polymorphic relation {InverseRelationName}.
```

This limitation exists to prevent corruption of data relationships that depend on this collection in a polymorphic context.
Before attempting deletion, ensure that the collection you want to delete is not used as a target in such a relationship before attempting the deletion.

#### Rename Collection

The [rename collection customization](../datasources/getting-started/naming-conflicts.md) also imposes a restriction on collections involved in polymorphic relationships. If you attempt to rename a collection that is the target of a polymorphic relationship, an exception will be thrown with the following message:

```
Cannot rename collection {CurrentName} because it's a target of a polymorphic relation '{ForeignCollectionName}.{InverseRelationName}'.
```

This limitation ensures the integrity of polymorphic relationships in your data model.
Before renaming a collection, verify that it is not involved in such polymorphic relationships to avoid errors.

### Fields

#### Remove field

When using the [remove field customization](../agent-customization/fields/import-rename-remove.md#renaming-and-removing-fields-and-relations), it's important to note a limitation concerning fields involved in the polymorphic relationships (`foreign_key_type` and `foreign_key_id`).
If you attempt to delete a field that is used in a polymorphic relationship, an exception will be thrown with the following message:

```
Cannot remove field {CollectionName}.{FieldName}, because it's implied in a polymorphic relation '{CollectionName}.{PolymorphicRelationName}'.
```

This limitation is in place to ensure the proper functioning of the polymorphic relationship.
Before deleting a field, ensure that the field you want to delete is not used as a foreign key or type field in any polymorphic relationship.

#### Rename field

When using the [rename field customization](../agent-customization/fields/import-rename-remove.md#renaming-and-removing-fields-and-relations), it's important to be aware of a limitation concerning fields involved in polymorphic relationships (`foreign_key_type` and `foreign_key_id`).
If you attempt to rename a field that is used in a polymorphic relationship, an exception will be thrown with the following message:

```
Cannot rename '{CollectionName}.{FieldName}', because it's implied in a polymorphic relation '{CollectionName}.{PolymorphicRelationName}'.
```

This limitation is in place to ensure the proper functioning of polymorphic relationship.
Before renaming a field, ensure that the field you want to rename is not used as a as foreign key or type field of a polymorphic relationship.

#### Computed field

When using [computed fields](../agent-customization/fields/computed.md), a list of `dependency` fields is required. Theses fields can travels '\*toOne' relationships using ':'.
However, due to the unpredictable target collection of polymorphic relationship, they cannot be used as dependencies for computed fields; an error will be thrown with the following message:

``` 
Dependencies over a polymorphic relations({CollectionName}.{PolymorphicRelationName}) are forbidden.
```

Another limitation exists with computed fields when they are used as `reference field`.
Due to the unpredictable target collection of the polymorphic relationships, computed fields cannot be evaluated over polymorphic relations. If used as a reference field, it will not be displayed on the front-end.

When computed fields are not evaluated over polymorphic relationships, a `debug` log is generated with the following message:

```
Cannot compute computed fields over polymorphic relation {CollectionName}.{PolymorphicRelationName}.
```

### Search (extended)

When searching within a collection that contains a polymorphic relationship, [extended searches](../agent-customization/search.md#search-modes) will not search through the polymorphic relationships.
This behavior is designed to prevent excessive resource consumption and delays in SQL queries.
A `debug` log will appear to alert you about this behavior with the following message:

```
We're not searching through {CollectionName}.{PolymorphicRelationName} because it's a polymorphic relation. You can override the default search behavior with 'replace_search'.
```

And a link leading to the [replace search documentation](../agent-customization/search.md#customization).
If you understand the implications and still want to search through polymorphic relations, you can follow the [replace search documentation](../agent-customization/search.md#customization) to override the default search behavior.
