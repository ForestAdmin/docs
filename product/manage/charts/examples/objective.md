![Objective Chart example](../../assets/chart-objective.png)

Objective Charts are very similar to Value Charts, the only difference being that two numbers should be provided to the `resultBuilder`: the value and the objective.

{% tabs %}
{% tab title="agent.addChart('companiesLive', async (context, resultBuilder) => {" %}
```javascript
// [...]

  return resultBuilder.objective(235, 300);
});
```
{% endtab %}

{% tab title="@create_agent.add_chart('companiesLive') do |_context, result_builder|" %}
```ruby
result_builder.objective(235, 300)
end
```
{% endtab %}
{% endtabs %}


