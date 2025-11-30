![The pagination widget in action](../assets/pagination.png)

## Disabling page count

Each time you load a Table View, 2 requests are made to your Agent:

- one to get the records to display according to the pagination size,
- one to get the total count of records.

Depending on the data source connector that you are using, the total count can be expensive to compute.
As it is used to display the total number of records and pages in the Table View, and this information is optional to use a Collection, you can choose to disable it.

<details>
<summary><strong>agent.customizeCollection('people', collection => {</strong></summary>

```javascript
  collection.disableCount();
});
```

</details>

<details>
<summary><strong>@create_agent.customize_collection('people') do |collection|</strong></summary>

```ruby
  collection.disable_count
end
```

</details>


