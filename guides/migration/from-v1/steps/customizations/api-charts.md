API charts in the legacy agents were declared using {{#nodejs}}express{{/nodejs}} routes.

In the new agent, you will need to use either the {{#nodejs,php}}`agent.addChart`{{/nodejs,php}}{{#python,ruby}}`agent.add_chart`{{/python,ruby}} or the {{#nodejs,php}}`collection.addChart`{{/nodejs,php}}{{#python,ruby}}`collection.add_chart`{{/python,ruby}} function, depending on if the chart is to be displayed on a record of a collection or a dashboard.

{% hint style="info" %}
You can find the full documentation of chart customization [here](../../../../agent-customization/charts/README.md).
{% endhint %}

# Code cheatsheet

{{#nodejs}}
| Legacy agent | New agent |
| ------------------------ | ------------------------------------------------------------------------- |
| router.post | agent.addChart<br>collection.addChart(...) |
| Liana.StatSerializer | return resultBuilder.value(...)<br>return resultBuilder.distribution(...) |
| request.query?.record_id | context.recordId |
{{/nodejs}}
{{#php}}
| Legacy agent | New agent |
| ---------------- | ------------------------------------------------------------------------- |
| route in web.php | $agent->addChart<br>collection.addChart(...) |
| render Chart | return $resultBuilder->value(...)<br>return $resultBuilder->distribution(...) |
| request object | $context->recordId |
{{/php}}
{{#python}}
| Legacy agent | New agent |
| ---------------- | ------------------------------------------------------------------------- |
| route in urls.py | agent.add_chart<br>collection.add_chart(...) |
| render Chart | return result_builder.value(...)<br>return result_builder.distribution(...) |
| request object | context.record_id |
{{/python}}
{{#ruby}}
| Legacy agent | New agent |
| ---------------- | ------------------------------------------------------------------------- |
| route in routes.rb | agent.add_chart<br>collection.add_chart(...) |
| render ForestLiana::Model::Stat | return result_builder.value(...)<br>return result_builder.distribution(...) |
| request object | context.record_id |
{{/ruby}}

# How to migrate

Migrating should be straightforward: the only differences are that:

- dashboard charts are now declared using the {{#nodejs,php}}`agent.addChart`{{/nodejs,php}}{{#python,ruby}}`agent.add_chart`{{/python,ruby}} function.
- collection charts are now declared using the {{#nodejs,php}}`collection.addChart`{{/nodejs,php}}{{#python,ruby}}`collection.add_chart`{{/python,ruby}} function, and access the record id using {{#nodejs,php}}`context.recordId`{{/nodejs,php}}{{#python,ruby}}`context.record_id`{{/python,ruby}} instead of `request.query?.record_id`.
- Both types should use the `resultBuilder` helper to return the chart data.

{% tabs %} {% tab title="Before" %}

```javascript
router.post('/stats/mrr', (req, res) => {
  // Load data
  const from = moment.utc('2018-03-01').unix();
  const to = moment.utc('2018-03-31').unix();
  const charges = await stripe.charges.list({ created: { gte: from, lte: to } });

  // Compute chart
  const mrr = charges.reduce((acc, charge) => acc + charge.amount, 0);
  const json = new Liana.StatSerializer({value: mrr}).perform();

  res.send(json);
});
```

{{#php}}

- Define a new route in `web.php`:
- Setup an action into a controller

```php
<?php

namespace App\Http\Controllers;

use ForestAdmin\LaravelForestAdmin\Facades\ChartApi;
use Stripe\StripeClient;

class ChartsController extends Controller
{
    public function mrr()
    {
        $mrr = 0;
        $stripe = new StripeClient('sk_AABBCCDD11223344');
        $charges = $stripe->charges->all(['limit' => 3]);
        foreach ($charges as $charge) {
            $mrr += $charge->amount;
        }
        return ChartApi::renderValue($mrr);
    }
}
```

{{/php}}

{{#python}}

- Define a new route in `urls.py`:
- Setup an action into a controller

```python
from django_forest.utils.views.base import BaseView


class MrrChartView(BaseView):
    def post(self, request):
        mrr = 0
        stripe = StripeClient("sk_AABBCCDD11223344")
        for charge in stripe.charges.all({"limit": 3}):
            mrr += charge.amount
        res = {
            "data": {
                "attributes": {
                    "value": {"countCurrent": mrr}
                },
                "type": "stats",
                "id": uuid.uuid4(),
            }
        }
        return JsonResponse(status=200, data=res)


```

{{/python}}

{{#ruby}}

- Define a new route in `routes.rb`:
- Setup an action into a controller

```ruby
def mrr
    mrr = 0

    Stripe.api_key = 'sk_AABBCCDD11223344'
    Stripe::Charge.list.list({ limit: 3 }).each do |charge|
        mrr += charge.amount
    end

    stat = ForestLiana::Model::Stat.new({ value: mrr })
    render json: serialize_model(stat)
  end
```

{{/ruby}}

{% endtab %} {% tab title="After" %}

```javascript
agent.addChart('monthlyRecuringRevenue', async (context, resultBuilder) => {
  // Load data
  const from = moment.utc('2018-03-01').unix();
  const to = moment.utc('2018-03-31').unix();
  const charges = await stripe.charges.list({ created: { gte: from, lte: to } });

  // Compute chart
  const mrr = charges.reduce((acc, charge) => acc + charge.amount, 0);

  return resultBuilder.value(mrr);
});
```

```php
<?php

use ForestAdmin\AgentPHP\Agent\Builder\AgentFactory;
use ForestAdmin\AgentPHP\DatasourceEloquent\EloquentDatasource;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Context\AgentCustomizationContext;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Chart\ResultBuilder;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Aggregation;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\ConditionTreeFactory;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Filters\Filter;

return static function () {
	$forestAgent = app()->make(AgentFactory::class);
    $forestAgent->addDatasource(
        new EloquentDatasource(...),
    )
    ->addChart(
    'monthlyRecuringRevenue',
    function (AgentCustomizationContext $context, ResultBuilder $resultBuilder) {
        $mrr = 0;
        $stripe = new StripeClient('sk_AABBCCDD11223344');
        $charges = $stripe->charges->all(['limit' => 3]);
        foreach ($charges as $charge) {
            $mrr += $charge->amount;
        }

        return $resultBuilder->value($mrr)
    }
);

```

```python
from forestadmin.datasource_toolkit.context.agent_context import AgentCustomizationContext
from forestadmin.datasource_toolkit.decorators.chart.result_builder import ResultBuilder

def mrr_chart(context: AgentCustomizationContext, result_builder: ResultBuilder):
    mrr = 0
    stripe = StripeClient("sk_AABBCCDD11223344")
    for charge in stripe.charges.all({"limit": 3}):
        mrr += charge.amount
    return result_builder.value(mrr)

agent.add_chart("monthlyRecuringRevenue", mrr_chart)
```

```ruby
module ForestAdminRails
  class CreateAgent
    def self.customize
      @create_agent.add_chart('monthlyRecuringRevenue') do |_context, result_builder|
        mrr = 0
        Stripe.api_key = 'sk_AABBCCDD11223344'
        Stripe::Charge.list.list({ limit: 3 }).each do |charge|
          mrr += charge.amount
        end

        result_builder.value(mrr)
      end
    end
  end
end
```

{% endtab %} {% endtabs %}
