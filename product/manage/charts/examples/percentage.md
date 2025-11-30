![Percentage Chart example](../../assets/chart-percentage.png)

Percentage Charts are very similar to value charts.
The range of valid values to be returned is from 0 to 100.

<details>
<summary><strong>agent.addChart('averageVolumeIncrease', async (context, resultBuilder) => {</strong></summary>

```javascript
  // [...]

  return resultBuilder.percentage(11);
});
```

</details>

<details>
<summary><strong>@create_agent.add_chart('averageVolumeIncrease') do |_context, result_builder|</strong></summary>

```ruby
  result_builder.percentage(11)
end
```

</details>


