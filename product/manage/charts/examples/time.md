![Time-based Chart example](../../assets/chart-time.png)

Time-based charts are very similar to distribution charts, the only differences being that:

- An additional parameter tells the frontend if the dates should be displayed by `Day`, `Week`, `Month`, or `Year`,
  {{#nodejs}}
- The returned data is an array of objects with `date` as a `Date` and `value` as a `Number`.
  {{/nodejs}}


<details>
<summary><strong>agent.addChart('transactionVolume', async (context, resultBuilder) => {</strong></summary>

```javascript
  // [...]

  return resultBuilder.timeBased('Month', [
    { date: new Date('2017-02-01'), value: 636 },
    {
      date: new Date('2017-03-01'),
      value: 740,
    },
    {
      date: new Date('2017-04-01'),
      value: 648,
    },
    {
      date: new Date('2017-05-01'),
      value: 726,
    },
    // [...]
  ]);
});
```

</details>

<details>
<summary><strong>@create_agent.add_chart('transactionVolume') do |_context, result_builder|</strong></summary>

```ruby
  result_builder.time_based(
    'Month',
    [
        { date: '2017-02-01', value: 636 },
        { date: '2017-03-01', value: 740 },
        { date: '2017-04-01', value: 648 },
        { date: '2017-05-01', value: 726 }
    ]
  )
end
```

</details>

<details>
<summary><strong>agent.addChart('newUsersByProject', async (context, resultBuilder) => {</strong></summary>

```javascript
  // [...]

  return resultBuilder.multipleTimeBased(
    'Month',
    [
      new Date('2017-02-28'),
      new Date('2017-03-28'),
      new Date('2017-04-28'),
      new Date('2017-05-28'),
    ],
    [
      { label: 'Project 1', values: [636, 740, 648, 726] },
      { label: 'Project 2', values: [100, 200, 300, 400] },
    ],
  );
});
```

</details>


{{/nodejs,python}}
