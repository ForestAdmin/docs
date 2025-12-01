API charts in the legacy agents were declared using {{#nodejs}}express{{/nodejs}} routes.

In the new agent, you will need to use either the {{#nodejs}}`agent.addChart`{{/nodejs}}{{#ruby}}`agent.add_chart`{{/ruby}} or the {{#nodejs}}`collection.addChart`{{/nodejs}}{{#ruby}}`collection.add_chart`{{/ruby}} function, depending on if the chart is to be displayed on a record of a collection or a dashboard.

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


{{#ruby}}
| Legacy agent | New agent |
| ---------------- | ------------------------------------------------------------------------- |
| route in routes.rb | agent.add_chart<br>collection.add_chart(...) |
| render ForestLiana::Model::Stat | return result_builder.value(...)<br>return result_builder.distribution(...) |
| request object | context.record_id |
{{/ruby}}

# How to migrate

Migrating should be straightforward: the only differences are that:

- dashboard charts are now declared using the {{#nodejs}}`agent.addChart`{{/nodejs}}{{#ruby}}`agent.add_chart`{{/ruby}} function.
- collection charts are now declared using the {{#nodejs}}`collection.addChart`{{/nodejs}}{{#ruby}}`collection.add_chart`{{/ruby}} function, and access the record id using {{#nodejs}}`context.recordId`{{/nodejs}}{{#ruby}}`context.record_id`{{/ruby}} instead of `request.query?.record_id`.
- Both types should use the `resultBuilder` helper to return the chart data.

{% tabs %} {% tab title="Before" %}

{% tabs %}
{% tab title="router.post('/stats/mrr', (req, res) => {" %}
```javascript
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
{% endtab %}

{% tab title="def mrr" %}
```ruby
mrr = 0

    Stripe.api_key = 'sk_AABBCCDD11223344'
    Stripe::Charge.list.list({ limit: 3 }).each do |charge|
        mrr += charge.amount
    end

    stat = ForestLiana::Model::Stat.new({ value: mrr })
    render json: serialize_model(stat)
  end
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="agent.addChart('monthlyRecuringRevenue', async (context, resultBuilder) => {" %}
```javascript
// Load data
  const from = moment.utc('2018-03-01').unix();
  const to = moment.utc('2018-03-31').unix();
  const charges = await stripe.charges.list({ created: { gte: from, lte: to } });

  // Compute chart
  const mrr = charges.reduce((acc, charge) => acc + charge.amount, 0);

  return resultBuilder.value(mrr);
});
```
{% endtab %}

{% tab title="module ForestAdminRails" %}
```ruby
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
{% endtab %}
{% endtabs %}

{% endtab %} {% endtabs %}
