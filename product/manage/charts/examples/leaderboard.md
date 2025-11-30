![Leaderboard Chart example](../../assets/chart-leaderboard.png)

Leaderboard Charts display a list of records sorted by their value in descending order.

```javascript
agent.addChart('companiesLive', async (context, resultBuilder) => {
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

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\Context\AgentCustomizationContext;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Chart\ResultBuilder;

$forestAgent->addChart(
    'companiesLive',
    fn(AgentCustomizationContext $context, ResultBuilder $resultBuilder) => $resultBuilder->leaderboard(
        [
            'Bonanza'   => 5835694,
            'TalkSpace' => 4179218,
            'Tesco'     => 3959931,
            'BitPesa'   => 3856685,
            'Octiv'     => 3747458,
        ]
    )
);
```

```ruby
@create_agent.add_chart('companiesLive') do |_context, result_builder|
  result_builder.leaderboard({
    Bonanza: 5835694,
    TalkSpace: 4179218,
    Tesco: 3959931,
    BitPesa: 3856685,
    Octiv: 3747458,
  })
end
```

```python
from forestadmin.datasource_toolkit.context.agent_context import AgentCustomizationContext
from forestadmin.datasource_toolkit.decorators.chart.result_builder import ResultBuilder
from forestadmin.datasource_toolkit.interfaces.chart import Chart

async def companies_live_chart_fn(
    context: AgentCustomizationContext, result_builder: ResultBuilder
) -> Chart:
    return result_builder.leaderboard({
        "Bonanza": 5835694,
        "TalkSpace": 4179218,
        "Tesco": 3959931,
        "BitPesa": 3856685,
        "Octiv": 3747458,
    })

agent.add_chart("companiesLive", companies_live_chart_fn)
```
