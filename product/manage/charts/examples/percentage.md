![Percentage Chart example](../../assets/chart-percentage.png)

Percentage Charts are very similar to value charts.
The range of valid values to be returned is from 0 to 100.

{% tabs %}
{% tab title="agent.addChart('averageVolumeIncrease', async (context, resultBuilder) => {" %}
```javascript
// [...]

  return resultBuilder.percentage(11);
});
```
{% endtab %}

{% tab title="@create_agent.add_chart('averageVolumeIncrease') do |_context, result_builder|" %}
```ruby
result_builder.percentage(11)
end
```
{% endtab %}
{% endtabs %}


