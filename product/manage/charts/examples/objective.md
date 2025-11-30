![Objective Chart example](../../assets/chart-objective.png)

Objective Charts are very similar to Value Charts, the only difference being that two numbers should be provided to the `resultBuilder`: the value and the objective.

<details>
<summary><strong>agent.addChart('companiesLive', async (context, resultBuilder) => {</strong></summary>

```javascript
  // [...]

  return resultBuilder.objective(235, 300);
});
```

</details>

<details>
<summary><strong>@create_agent.add_chart('companiesLive') do |_context, result_builder|</strong></summary>

```ruby
  result_builder.objective(235, 300)
end
```

</details>


