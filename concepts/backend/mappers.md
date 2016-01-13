# Mappers

The job of a mapper is to take an incoming request (e.g. the body of a PUT or POST) and transform the data therein to changes in the application (usually records in the database.)

Generally, you'll have (at least) one mapper per REST resource. Those mappers will be descendants of ```Xing::Mappers::Base```.

### Subclasses must define:

Method | Purpose
 --- | ----
 aliases       | for self.record
 record_class | if the mapper maps to an AR object

### Subclasses should usually define:

Method | Purpose
--- | ---
assign_values  | move values from JSON into the mapped AR record

### Subclasses may also want to define:

Method | Purpose
--: | ---
locator_attribute_name | if the locator something other than :id, like :url_slug
find_existing_record | for locating the underlying AR record
build_new_record     | for for instantiating a new underlying AR record
map_nested_models |
build_errors         | if simply copying AR errors is insufficient
save                 | if they need to save more than 1 AR record

When updating records, pass the locator (e.g. DB id, url_slug, or other unique resource extracted from the resource path) as the second argument.

## Example

```ruby
class ProjectMapper < Xing::Mappers::Base

  # These aliases aren't strictly necessary.  By default 
  # any Xing mapper has a class variable with getter and setter
  # called 'record', which is assumed to be the ActiveRecord object
  # that the mapper is mapping to.  But since this one maps Projects,
  # we alias so that we can use the more readable #project and #project=
  # methods.
  alias project record
  alias project record=

  def record_class
    # this should be the actual ActiveRecord class we are mapping to
    Project  
  end

  def assign_values(data_hash)
  
    # the assign_values method in the superclass assumes that
    # attribute names in the data hash match attribute names
    # in the activerecord class, so often you don't need to do anything
    super
  end
 
  def map_nested_models
    # if the project JSON resource embedded other nested resources,
    # logic for extracting their data (to get put into related AR records,
    # or wherever) would go here.
  end

end
```

