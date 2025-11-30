![Distribution Chart example](../../assets/chart-distribution.png)

Distribution Charts display a pie and should return a plain object.

```javascript
agent.addChart('booksByAuthorCountry', async (context, resultBuilder) => {
  // [...]

  return resultBuilder.distribution({
    validated: 100,
    rejected: 100,
    to_validate: 100,
  });
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\Context\AgentCustomizationContext;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Chart\ResultBuilder;

$forestAgent->addChart(
    'booksByAuthorCountry',
    fn(AgentCustomizationContext $context, ResultBuilder $resultBuilder) => $resultBuilder->distribution(
        [
            'validated'   => 100,
            'rejected'    => 100,
            'to_validate' => 100,
        ]
    )
);
```

```ruby
@create_agent.add_chart('booksByAuthorCountry') do |_context, result_builder|
  result_builder.distribution({
    validated: 100,
    rejected: 100,
    to_validate: 100
  })
end
```

```python
from forestadmin.datasource_toolkit.context.agent_context import AgentCustomizationContext
from forestadmin.datasource_toolkit.decorators.chart.result_builder import ResultBuilder
from forestadmin.datasource_toolkit.interfaces.chart import Chart

async def books_by_author_country_chart_fn(
    context: AgentCustomizationContext, result_builder: ResultBuilder
) -> Chart:
    return result_builder.distribution({
        "validated": 100,
        "rejected": 100,
        "to_validate": 100,
    })

agent.add_chart("booksByAuthorCountry", books_by_author_country_chart_fn)
```
