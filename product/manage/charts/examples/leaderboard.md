![Leaderboard Chart example](../../assets/chart-leaderboard.png)

Leaderboard Charts display a list of records sorted by their value in descending order.

<details>
<summary><strong>agent.addChart('companiesLive', async (context, resultBuilder) => {</strong></summary>

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

</details>

<details>
<summary><strong>@create_agent.add_chart('companiesLive') do |_context, result_builder|</strong></summary>

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

</details>


