# Model-View-Controller (MVC)

## Learning Goals

- Explain the benefit of the model-view-controller pattern
- Know what each layer of MVC is responsible for

## Model-View-Controller Overview

In order to ensure that applications have a specific organizational structure,
the creators of Rails utilized the model-view-controller architecture. This
concept can be a little abstract if you've never used it before, so let's give
it a real world analogy.

In a restaurant kitchen we have three key components:

- A chef that makes the food
- A waiter that takes the order and brings out the food
- A table where the food is served to the customer

Using this as an analogy, we can assign the following roles to each of our
restaurant components:

- **Model**: The model is the _chef_. It manages the critical aspects of the
  application, such as communicating with the database via Active Record. One of
  my favorite tasks in a Rails application is working with the model files. This
  is where you can be very expressive with the custom algorithms that you want
  to utilize and you also have direct access to the specific database record.
  The logic of your application should mainly reside in the model files.

- **Controller**: The controller is the _waiter_. In the same way that the
  waiter takes the order from the customer to the chef and then brings the food
  to the table, the controller transmits data requests from the user to the
  model, and then delivers data that is rendered in the view to the user.

- **View**: The view is the _table_. A table shouldn't do anything besides sit
  there and hold the food when it is delivered. In like manner the views should
  not contain any programming logic, they should simply render what the
  controller sends them. In a Rails API application, our view layer will consist
  of the JSON data that is returned from our controllers.

## Routing, File Naming Conventions, and Data Flow

Rails was created with the concept of convention over configuration, and this
holds true for how the MVC structure was set up. View files correspond directly
to controller files, which speak directly with models. As an example, imagine
that you have a blog that has a database table called `articles`. You will have the
following set of files:

- A `app/models/article.rb` model file that will contain: validations, database
  relationships, callbacks, and any custom logic for articles.

- A `app/controllers/articles_controller.rb` file that will have methods to manage
  data flow for the article behavior, including the full set of CRUD features.
  The standard methods are: `index`, `create`, `show`, `update`, and `destroy`.

- A `app/views/articles` directory that will contain a corresponding view for
  each of the pages that the end user will access. **Note**: in a Rails API
  application, we won't have a dedicated folder for our views like we would in a
  typical Rails MVC application.

In order to benefit from all the work Rails does for us — and to avoid breaking
our app — we need to follow the indicated file structure and naming conventions.

## Request Flow

![MVC Request Flow](https://s3.amazonaws.com/flatiron-bucket/readme-lessons/mvc_flow_updated.png)

## Roles and Responsibilities

One reason that MVC architecture is so popular is because it enforces the idea
of **separation of concerns** in an application. Each layer has a clearly
defined role.

### Models

At the end of the day, the model file is a Ruby class. If it has a corresponding
database table, it will inherit from the `ActiveRecord::Base` class, which means
that it can access a number of methods that assist in working with the database.
However, you can treat it like a regular Ruby class, allowing you to create
methods, data attributes, and everything else that you would want to do in a
class file.

In a typical model file you will find your application's domain logic. To extend
the restaurant analogy, the chef (your **model**) performs a number of tasks to
create each meal that the waiter (**controller**) and especially the table
(**views**) don't know anything about. Some of this domain logic would include
items such as complex database queries, data relationships, and custom
algorithms.

It is important to remember to follow the single responsibility principle for
your model class files. If any of the methods that you place in the model file
perform tasks outside the scope of that specific model, they should probably be
moved to their own class.

### Controllers

As mentioned before, the controller is like the waiter in a restaurant. The
controllers connect the models, views, and routes.

In order for the controller to be able to connect everything together, each
route defined in the routes file must have a corresponding method in a
controller. When a request comes in, Rails uses the routes file to determine
which controller method to run. The controller method then accesses data using
the model, and uses that data to render the correct view (whether that's an
actual `.erb` file, or some JSON data).

Remembering our restaurant analogy, the easiest way to think of the controller
is that it manages data flow between the routes, model, and views, in the same
way that a waiter takes the order from a patron, relays the order details to the
chef, and brings the meal out to the table.

### Views

In a Rails application, the view layer should contain the least amount of logic
of any layer in the model-view-controller architecture. The role of the view is
to simply render whatever it is sent from the controller.

The kind of data our view layer is responsible for depends on what kind of
response our clients need. In a full-stack Rails app, that could mean generating
HTML as a view by using a `.erb` template:

```erb
<div>
  <%= "#{@post.title}: #{@post.summary}" %>
</div>
```

For a Rails API, we'll typically be responding with JSON data.

The data for our view layer will typically come from our models:

```rb
class ArticlesController < ApplicationController

  def index
    # access data from the Model
    articles = Article.all
    # respond with a View
    render json: articles
  end

end
```

Later on, we'll also see how to customize the structure of this JSON data even
further using a **serializer**. For now though, just think of the view as the
response that's being returned from the controller.

## Resources

- [Getting Started with Rails](https://guides.rubyonrails.org/getting_started.html)
