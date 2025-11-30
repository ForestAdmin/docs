![Percentage Chart example](../../assets/chart-percentage.png)

Percentage Charts are very similar to value charts.
The range of valid values to be returned is from 0 to 100.

```javascript
agent.addChart('averageVolumeIncrease', async (context, resultBuilder) => {
  // [...]

  return resultBuilder.percentage(11);
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\Context\AgentCustomizationContext;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Chart\ResultBuilder;

$forestAgent->addChart(
    'averageVolumeIncrease',
    fn(AgentCustomizationContext $context, ResultBuilder $resultBuilder) => $resultBuilder->percentage(11)
);
```

```ruby
@create_agent.add_chart('averageVolumeIncrease') do |_context, result_builder|
  result_builder.percentage(11)
end
```

```python
from forestadmin.datasource_toolkit.context.agent_context import AgentCustomizationContext
from forestadmin.datasource_toolkit.decorators.chart.result_builder import ResultBuilder
from forestadmin.datasource_toolkit.interfaces.chart import Chart

async def average_volume_increase_chart_fn(
    context: AgentCustomizationContext, result_builder: ResultBuilder
) -> Chart:
    return result_builder.percentage(11)

agent.add_chart("averageVolumeIncrease", average_volume_increase_chart_fn)
```
