# Mappers

The job of a mapper is to take an incoming request (e.g. the body of a PUT) and transform the data therein to changes in the application (usually records in the database.)

Generally, you'll have (at least) one mapper per REST resource. Those mappers will be descendants of Xing::Mappers::Base. It's best practice for there to be one mapper class per model used - it's much easier to compose several mappers into a single Rails controller than it is to manage multiple database tables in a single mapper.

## In use

Within the controller for a particular resource, actions that modify the database (i.e. `#update`, `#create`) create the appropriate mapper, give it the JSON of the request body and a parameter value (e.g. `:id`), and then call `mapper.save`.

A very standard example:
```ruby
class CheeseController
  def update
    mapper = CheeseMapper.new(body, params[:id])
    mapper.save
    # and so on ...
  end
end
```

Each mapper extends and overrides `Xing::Mappers::Base`. Like other Xing backend classes, mappers are designed to be overriden in both large and small ways. Most useful for this task is understanding the flow that happens in the `.new; .save` process.

## Mapper Extension Guide

First of all, a mapper goes through four distinct phases when it does a `save`:

0. Load - the ActiveRecord object needed is loaded from the database or created fresh
0. Update - values from the request JSON are transfered to the the object
0. Validation - ActiveRecord validations are checked, and whole-request validations are performed
0. Save - if the request validates, the ActiveRecord object is saved.

By cleanly separating these phases, we make it possible to compose Xing Mappers in order to cleanly separate HTTP resources from the database tables that record them.

One key idea to mappers is the idea of "nesting" mappers: a top level mapper can contruct other mappers, build JSON documents for them, and otherwise use other mappers in order to break even the most complicated requests into reasonably sized pieces.

### A Simple Mapper

Many mappers simply copy values from the incoming JSON record to fields in the database. If this is sufficient for a particular resource, you can do it like this:

```ruby
class CheeseMapper < Xing::Mappers::Base
  def record_class
    Cheese
  end

  def assign_values(data)
    record.update_attributes(data.slice(:kind, :age, :color, :smell))
  end
end
```


### Complete reference for the save cycle

The example above covers many use cases of mappers, but not all. Almost every Xing application will include more complicated resources. Rather than try to categorize every such use case, we've included a guide to how the Mapper superclass works, so that you can determine what methods make sense to override.

First, the mapper is initialized with:
source_hash: the parsed JSON from the request
locator: the value used to look up the record (nil, in the case of a create)

In the below, indents indicate a nested call. Reading top to bottom completes the save process
* `save`
  * `perform_mapping`
    * data = `unwrap_data(source_hash)`
      * `source_hash["data"]`
    * links = `unwrap_links(source_hash)`
      * `source_hash["links"]`
    * errors = {}

    * `assign_values(data)`
      * `record`
        * `find_existing_record` if locator is present
          * @record gets `record_class`.find(@locator)
            * _record_class_ has to be implemented by subclass
        * `build_new_record` otherwise
          * @record gets `record_class`.new
      * `update_record`
        * _update_record_ has to be implemented by a subclass
    * `map_nested_models`
      * _map_nested_models_ can be left as is, but doesn't do anything by default
    * `build_errors`
      * `add_ar_errors(record)`
        * Converts activerecord validations into Xing style error document fields
  * `save_nested_models` and `record.save` if no errors
    * _save_nested_models_ can be left as is, but doesn't do anything by default


### Subclasses must define:

Method | Purpose
 --- | ----
 record_class  | if the mapper maps to an AR object
assign_values  | move values from JSON into the mapped AR record

### Subclasses should usually define:

Method | Purpose
--- | ---
aliases        | for self.record

### Subclasses may also want to define:

Method | Purpose
--: | ---
locator_attribute_name | if the locator something other than :id, like :url_slug
find_existing_record | for locating the underlying AR record
build_new_record     | for for instantiating a new underlying AR record
map_nested_models |
save_nested_models |
build_errors         | if simply copying AR errors is insufficient

When updating records, pass the locator (e.g. DB id, url_slug, or other unique resource extracted from the resource path) as the second argument.
