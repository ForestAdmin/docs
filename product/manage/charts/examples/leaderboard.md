![Leaderboard Chart example](../../assets/chart-leaderboard.png)

Leaderboard Charts display a list of records sorted by their value in descending order.

{% tabs %}
{% tab title="agent.addChart('companiesLive', async (context, resultBuilder) => {" %}
```javascript
// [...]

  return resultBuilder.leaderboard({
    Bonanza: 5835694,
    TalkSpace: 4179218,
    Tesco: 3959931,
    BitPesa: 3856685,
    Octiv: 3747458,
  });
});
```
{% endtab %}

{% tab title="@create_agent.add_chart('companiesLive') do |_context, result_builder|" %}
```ruby
result_builder.leaderboard({
    Bonanza: 5835694,
    TalkSpace: 4179218,
    Tesco: 3959931,
    BitPesa: 3856685,
    Octiv: 3747458,
  })
end
```
{% endtab %}
{% endtabs %}


