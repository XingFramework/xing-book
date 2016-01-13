# Serializers

Xing serializers are responsible for producing JSON resources for transmission to the frontend.  

```Xing::Serializers::Base``` is the base class for all Xing serializers.  In addition, a few subclasses are useful:

* ```Xing::Serializers::List``` provides for serialization of an ordered list or array of data. By analogy to a classic Rails application, this would be an #index action in a controller.
* ```Xing::Serializers::PagedList``` provides for a list that is both ordered and paginated, with forward and back links to retrieve previous and next pages of items.
* ```Xing::Serializers::RootResources``` should be subclassed to provide your application's ```/resources``` list, the first endpoint visited by the client.

In general, subclasses of ```Xing::Serializers::Base``` should:

 * Define a links method that returns a hash of hypermedia links to related resources

 * Specify the attributes (via the ```ActiveModel::Serializers.attributes``` class method) that will be copied into the data: block of the generated resource.

 * Define methods for any attributes that do not exist as plain attributes in the ```ActiveModel``` being serialized.  Note that this may (and often will) include calling other serializers on related resources or other data, in order to generate embedded resources.

Xing serializers descend from ```ActiveModel::Serializer``` and are typically instantiated with an instance of an ```ActiveModel``` model in the usual way.

## Example

```ruby
class Projects < Xing::Serializers::Base
  attributes :name, :deadline

  def links
    {}
  end
end
```
