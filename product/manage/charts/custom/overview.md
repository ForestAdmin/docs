# Create a smart chart

By using the finite list of predefined Charts (Single, Distribution, Time-based, etc.), you are constrained by how that data is displayed.

With **Smart Charts**, you can code exactly which data you want and how you want it displayed!

{% hint style="info" %}
You need to be on a [Forest Admin Pro plan](https://www.forestadmin.com/pricing) to have access to this feature.
{% endhint %}

### Creating a smart chart

To create a Chart and access the Smart Chart Editor, click on the **Edit Smart Chart** button:

![](../../assets/smart-chart-create.png)

Next, use the _Template_, _Component,_ and _Style_ tabs to create your custom Chart. At any point, you can render your chart by clicking on the **Run code** button.

![](../../assets/smart-chart-code.png)

{% hint style="warning" %}
Don't forget to click on **Create Chart** (or **Save** if the Chart is already created) once you're done!
{% endhint %}

{% hint style="info" %}
If you are creating a Smart Chart in the context of a specific record (in the record Analytics tab), the **`record`** object is directly accessible (either through `this.args.record` in the component or `@record` in the template).
{% endhint %}
