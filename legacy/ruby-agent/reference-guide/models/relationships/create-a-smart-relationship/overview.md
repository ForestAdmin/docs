---
title: Create a Smart relationship
---
### What is a Smart Relationship?

Sometimes, you want to create a virtual relationship between two set of data that does not exist in your database. A concrete example could be creating a relationship between two collections available in two different databases. Creating a Smart Relationship allows you to customize with code how your collections are linked together.

### Create a BelongsTo Smart Relationship

On the Live Demo example, we have an **order** which `belongsTo` a **customer** which `belongsTo` a **delivery address**. We’ve created here a BelongsTo Smart Relationship that acts like a shortcut between the **order** and the **delivery address**.

A BelongsTo Smart Relationship is created like a [Smart Field](../../../smart-fields/#what-is-a-smart-field) with the `reference` option to indicate on which collection the Smart Relationship points to. You will also need to code the logic of the search query.

### Rails

```ruby
class Forest::Order
  include ForestLiana::Collection

  collection :Order

  search_delivery_address = lambda do |query, search|

    query.joins(customer: :address).or(Order.joins(customer: :address).where("addresses.country ILIKE ?", "%#{search}%"))

  end

  belongs_to :delivery_address, reference: 'Address.id', search: search_delivery_address do
    object.customer.address
  end
end
```

### Create a HasMany Smart Relationship

On the Live Demo example, we have a **product** `hasMany` **orders** and an **order** `belongsTo` **customer**. We’ve created a Smart Relationship that acts like a shortcut: **product** `hasMany` **customers**.

A HasMany Smart Relationship is created like a [Smart Field](https://docs.forestadmin.com/documentation/reference-guide/fields/create-and-manage-smart-fields) with the `reference` option to indicates on which collection the Smart Relationship points to.

### Rails

```ruby
class Forest::Product
  include ForestLiana::Collection

  collection :Product

  has_many :buyers, type: ['String'], reference: 'Customer.id'
end
```

### Rails
Upon browsing, an API call is triggered when accessing the data of the HasMany relationships in order to fetch them asynchronously. In the following example, the API call is a GET on `/Product/:product_id/buyers`.

We’ve built the right SQL query using [Active Record](http://guides.rubyonrails.org/active_record_basics.html) to **count** and **find all** customers who bought the current product.

Then, you should handle pagination in order to avoid performance issue. The API call has a querystring available which gives you all the necessary parameters you need to enable pagination.

Finally, you don’t have to serialize the data yourself. The Forest Admin agent already knows how to serialize your collection (`Customer` in this example). You can access to the serializer through the `serialize_models()` function.

```ruby
Rails.application.routes.draw do
  # MUST be declared before the mount ForestLiana::Engine.
  namespace :forest do
    get '/Product/:product_id/buyers' => 'orders#buyers'
  end

  mount ForestLiana::Engine => '/forest'
end
```

```ruby
class Forest::ProductsController < ForestLiana::ApplicationController
  def buyers
    limit = params['page']['size'].to_i
    offset = (params['page']['number'].to_i - 1) * limit

    product = Product.find(params['product_id'])
    customers = Customer.where(order_id: product.orders.ids)

    render json: serialize_models(customers.limit(limit).offset(offset), meta: {count: customers.count})
  end
end
```

