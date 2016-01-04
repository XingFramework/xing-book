# Paged Lists

It's a common use case to need to transmit a long list of items from the backend to the frontend. In cases where the length of the list precludes transmitting the whole list verbatim, it's useful to chop the list up into "pages" and make those pages available to access by the frontend.

This is a common enough use case that Xing includes specialized serializers to support it. Here's how to use them.

# The Overall Setup

Normally, when you're building a resource, you build a controller with the normal Rails actions of`show`, `update`, `create`, `destroy` and `index.` In a certain kind of way, the index action is actually a different resource. After all, you access the other actions by making requests to a URL like `/fleas/1425326`, and `#index` is used to respond to queries for `/fleas`.

What we're going to do as we switch over to a paginated list is to have two controllers. One will be the indiviual item controller (e.g. `FleasController`) and one will be the list controller. The item controller is exactly the same as a usual controller as discussed in the general Xing workflow, except that we remove the `#index` action. The list controller will have just `#index` and `#show`.

You'll need to make sure that the 'kaminari' gem is added to the `backend/Gemfile` as well.

# Routes

Let's keep going with the "fleas" example. First, we'll add routes for the flea resources:

```
  resources :fleas, :except => [:index]
  resources :flea_pages, :only => [:index, :show], :param => :page
```


# Controllers

We'll need two controllers. The first is the `FleasController`, which is exactly like a normal Xing controller, with the exception that the `#index` action is omitted.

The `FleaPagesController` is a little different from usual. Let's take a look:

```
class FleaPagesController < ApplicationController
  def index
    render json: FleaIndexSerializer.new(page_relation(1))
  end

  def show
    render json: FleaPageSerializer.new(page_relation(params[:page]))
  end

  private

  def page_relation(page_num)
    Fleas.order(:created_at).page(page_num)
  end
end
```

But what are those two extra serializers? Well...

# Serializers

There are two extra serializers to write to complete the paged list. They're very simple:

```
class FleaIndexSerializer < Xing::Serializers::PagedIndex
  def self_link
    routes.fleas_path
  end

  def template_link
    routes.flea_page_path_rfc6570
  end
end

class FleaPageSerializer < Xing::Serializers::PagedList
  def item_serializer_class
    FleaSerializer
  end

  # This whole method is optional
  # In our example, we'll pass these option to the FleaSerializer to keep the list short
  def item_serializer_options
    { only: [:circus_trick, :weight] }
  end

  def template_link
    routes.flea_page_path_rfc6570
  end
end
```

# Mappers

At the moment, paged lists are considered read only. You can set up a POST action on e.g. the `:fleas` route in order to create new items, as well as handling update and delete on the item controller. A streamlined way to handle re-ordering of items in a paged list is a feature we'd like to support, but there isn't yet a Xing tool to do that.

The good news is: having written the FleaMapper already, we're done!

# The Frontend handling

To come...
