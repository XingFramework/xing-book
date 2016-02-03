# Versioning

Xing follows a [http://semver.org/spec/v2.0.0.html](semantic versioning) scheme. Individual libraries prefixed with 'xing-' (that is all the Ruby gems, and most of the npm modules), as well as Relayer and A1Atscript each follow such a scheme. You should feel safe to update any of those dependencies of a Xing application over time without concern, so long as you do not update past your current major version.

## Our Commitment

In maintaining Xing, we're committing that any public interface to the various tools will continue as is for an entire major version. That means: if you reference a file path, use or subclass a class, or call a function or method provided by a Xing tool, those usages will remain valid.

We will want to extend the functionality of Xing over time. When we do this, it will always be by extending Xing's interfaces. New file paths, new classes, new methods and functions. We may add parameters to existing functions, but only optional parameters, and their omission will always result in the behavior from before they existed. Releases with these new functions will increment the appropriate tool's version number.

Like any piece of software, we expect that we will find bugs in Xing; probably many will be discovered outside of the Xing core team. We're commiting to fixing those, and when we do, it will be without change to the interface. However, fixing a bug necessarily means a change in behavior, and we are disclaiming responsibility to maintain bug-level compatibility within any series of any Xing tool. This is especially true of security fixes.

Ultimately, the interface of Xing has a design, and the released code is intended to fulfill that design - as the design changes, major and minor versions will change; as we discover that our intention wasn't realized, subversions will change.

## Synchronization of Versions

The Xing tools are all being set to version 1.0.0 on public release. (At time of writing, this isn't quite true: Xing is being announced at 1.0.0-beta, but a second synchronization will happen on the full release.) This is by way of declaring that these interfaces and their according behaviors are considered final and released. The commitment implied by semantic versioning is in effect, and you can take the assurances of that fact.

It is likely that major version updates (from 1.x to 2.x) will also need to be synchronized, because features of the "top level" libraries and tools will rely on the interfaces further down. Because those shifts will require a great deal of effort on client application code, we'll consider such synchronization very carefully before doing it.

## Deprecation

It is our intention that, when features and interfaces are superseded or otherwise deprecated, we will at least document this deprecation. It is our preference that code that uses deprecated features will make itself known to its developer (but never users of the software), for instance by printing a warning during testing or logging the issue during execution. These facilities will take their own time to develop, however, which will need to be set against a timely release schedule.

Likewise, it is our intention and preference that no interface be removed or its behavior changed materially at a major version release unless it has been previously publicly deprecated. A consequence of such a policy might be that an intermediate release be made with interfaces slated for removal being newly deprecated so that developers can test their code against them. Again, this may have impacts on a release schedule, so we'll consider such a plan carefully before each major version release.

## Beta

Beta releases of Xing tools are intended to make an interface public without completely commiting to it. They exist in order to provide a period for comment and feedback before we make a final release of the version. You should assume that the interface of a beta release is as the future final release will be, without the assurances that we'd make based on a final release.
