![Time-based Chart example](../../assets/chart-time.png)

Time-based charts are very similar to distribution charts, the only differences being that:

- An additional parameter tells the frontend if the dates should be displayed by `Day`, `Week`, `Month`, or `Year`,
  {{#nodejs}}
- The returned data is an array of objects with `date` as a `Date` and `value` as a `Number`.
  {{/nodejs}}
  {{#php}}
- The keys of the returned object must be ISO-8601 compliant dates.
  {{/php}}
  {{#python}}
- The keys of the returned object can be ISO-8601 compliant string dates, python `datetime.date` object, or python `datetime.datetime` object.
  {{/python}}

```javascript
agent.addChart('transactionVolume', async (context, resultBuilder) => {
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

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\Context\AgentCustomizationContext;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Chart\ResultBuilder;

$forestAgent->addChart(
    'transactionVolume',
    fn(AgentCustomizationContext $context, ResultBuilder $resultBuilder) => $resultBuilder->timeBased(
        'Month',
        [
            '2017-02-01' => 636,
            '2017-03-01' => 740,
            '2017-04-01' => 648,
            '2017-05-01' => 726,
        ]
    )
);
```

```ruby
@create_agent.add_chart('transactionVolume') do |_context, result_builder|
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

```python
from datetime import date, datetime
from forestadmin.datasource_toolkit.context.agent_context import AgentCustomizationContext
from forestadmin.datasource_toolkit.decorators.chart.result_builder import ResultBuilder
from forestadmin.datasource_toolkit.interfaces.chart import Chart

async def transaction_volume_chart_fn(
    context: AgentCustomizationContext, result_builder: ResultBuilder
) -> Chart:
    return result_builder.timeBased(
        "Month",
        {
            '2017-02-01': 636,
            date(2017, 3, 1): 740,
            datetime(2017, 4, 1, 0, 0): 648,
            '2017-05-01': 726,
    })

agent.add_chart("transactionVolume", transaction_volume_chart_fn)
```

{{#nodejs,python}}
If you want to add several lines to the same chart, you can use the {{#nodejs}}`multipleTimeBased`{{/nodejs}}{{#python}}`multiple_time_based`{{/python}} method:

```javascript
agent.addChart('newUsersByProject', async (context, resultBuilder) => {
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

```python
from datetime import date, datetime
from forestadmin.datasource_toolkit.context.agent_context import AgentCustomizationContext
from forestadmin.datasource_toolkit.decorators.chart.result_builder import ResultBuilder
from forestadmin.datasource_toolkit.interfaces.chart import Chart


async def new_users_by_project(
    context: AgentCustomizationContext, result_builder: ResultBuilder
) -> Chart:
      # [...]

      return result_builder.multipleTimeBased(
          "Month"
          [
              date(2017, 2, 28),
              datetime(2017, 3, 28),
              '2017-04-28',
              '2017-05-28',
          ],
          [
              {"label": 'Project 1', "values": [636, 740, 648, 726]},
              {"label": 'Project 2', "values": [100, 200, 300, 400]},
          ],
      )


agent.add_chart('newUsersByProject', new_users_by_project)
```

{{/nodejs,python}}
