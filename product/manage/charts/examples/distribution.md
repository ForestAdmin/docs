![Distribution Chart example](../../assets/chart-distribution.png)

Distribution Charts display a pie and should return a plain object.

<details>
<summary><strong>agent.addChart('booksByAuthorCountry', async (context, resultBuilder) => {</strong></summary>

```javascript
  // [...]

  return resultBuilder.distribution({
    validated: 100,
    rejected: 100,
    to_validate: 100,
  });
});
```

</details>

<details>
<summary><strong>@create_agent.add_chart('booksByAuthorCountry') do |_context, result_builder|</strong></summary>

```ruby
  result_builder.distribution({
    validated: 100,
    rejected: 100,
    to_validate: 100
  })
end
```

</details>


