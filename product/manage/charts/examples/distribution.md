![Distribution Chart example](../../assets/chart-distribution.png)

Distribution Charts display a pie and should return a plain object.

{% tabs %}
{% tab title="agent.addChart('booksByAuthorCountry', async (context, resultBuilder) => {" %}
```javascript
// [...]

  return resultBuilder.distribution({
    validated: 100,
    rejected: 100,
    to_validate: 100,
  });
});
```
{% endtab %}

{% tab title="@create_agent.add_chart('booksByAuthorCountry') do |_context, result_builder|" %}
```ruby
result_builder.distribution({
    validated: 100,
    rejected: 100,
    to_validate: 100
  })
end
```
{% endtab %}
{% endtabs %}


