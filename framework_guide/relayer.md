# Relayer

*(WIP)*

This is currently a quick note on a realization I had about how Relayer frames something actually kind of complicated: embedded resources.

The basic idea of a resource is that it's a whole, individual, self-contained thing. It has a URL that identifies it (at `/links/self`), and updates can be sent to that link. Other resources are available via named links (e.g.  `/links/next_page`).

Relayer makes all this easy: Resource classes are described with relationships spelled out to other resources, and by following those links (and awaiting the returned Promise) you can access the resource.

One feature of Relayer (and Xing) is the ability to _embed_ resources. That is, within one resource, we can include a copy of another one. This can be handy when one complex resource needs to include the data from other resources.

It can be tempting, therefore, to include partial versions of a resource that only include the fields needed by the main resource.

A key point to remember here: updates (i.e. `PUT`) should include complete representations of the new state of the resource. As a result, we come to the situation where we may retreive a partial embedded resource and wish to update it. A correctly implemented backend will consider the update to delete the fields that were omitted from the original representation. (We can't avoid this: how else should we remove those fields in cases where we would want to do so?)

To further understand this phenomenon, consider what you're trying to do: just change the values of the names fields, and leave "everything else" untouched.  The correct way to do this would be to issue an HTTP `PATCH` request - typically we'd produce the body of such a request by building a diff from the original; but we _don't have_ the complete original to produce a diff from.

All that said, it's possible that in the future Relayer might built "blind" diffs and let the server sort out the difference.
