![Objective Chart example](../../assets/chart-objective.png)

Objective Charts are very similar to Value Charts, the only difference being that two numbers should be provided to the `resultBuilder`: the value and the objective.

```javascript
agent.addChart('companiesLive', async (context, resultBuilder) => {
  // [...]

  return resultBuilder.objective(235, 300);
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\Context\AgentCustomizationContext;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Chart\ResultBuilder;

$forestAgent->addChart(
    'companiesLive',
    fn(AgentCustomizationContext $context, ResultBuilder $resultBuilder) => $resultBuilder->objective(235, 300)
);
```

```ruby
@create_agent.add_chart('companiesLive') do |_context, result_builder|
  result_builder.objective(235, 300)
end
```

```python
from forestadmin.datasource_toolkit.context.agent_context import AgentCustomizationContext
from forestadmin.datasource_toolkit.decorators.chart.result_builder import ResultBuilder
from forestadmin.datasource_toolkit.interfaces.chart import Chart

async def companies_live_chart_fn(
    context: AgentCustomizationContext, result_builder: ResultBuilder
) -> Chart:
    return result_builder.objective(235, 300)

agent.add_chart("companiesLive", companies_live_chart_fn)
```
