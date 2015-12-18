# Caveat Lector

Be aware that this section is much rougher, and contains a great deal more unvarnished opinion. If you're just learning Xing, you can (and probably should) skip this appendix.

# Hypermedia

Since Roy Fielding's seminal disertation, the concept of REST has been repeatedly misunderstood and misrepresented, to the degree that its adherents have found it necessary to use completely different terminology. Like early Christians, they use words that are hard for their enemies to pronounce as a way to identify each other.

Rather than waste lifetimes trying to descibe "what REST is not," we prefer to explain what we believe REST to be.

First of all, REST (that is: Represestational State Transfer) is an *architectural* approach, structured around principles of creative constraint. No framework (including Xing) can claim to encompass all of REST - the best we can do is endevour to support RESTful principles, and try to starve non-RESTful approaches.

That said, REST is the architecture that HTTP is built on - there's a reason that the names at the top of the HTTP RFCs include "R. Feilding" - so repecting its principles pays dividends in terms of performance and infrastructural costs.

Fielding's dissertation is dense, so trying to summarize it here would be folly. Read Fielding's thesis and gain ten stones of strength.

What we can do is try to explain how we've tried to apply the RESTful style to Xing.

# The Transport

Xing assumes that you'll be using HTTP (at this writing, in fact, there's no other supported transport.)  HTTP is a really impressive transport layer, but it's important not to mistake it for anything other than what it is. REST and HTTP are not synomous, and it should be possible to refigure the components of Xing against different transports. To be completely candid, though, this has never been a goal of Xing as a framework.

# As the Engine

This is the modern touchstone for the RESTful architectural style. Relayer attempts to support this - where ever you are in a Xing application, you should be able to get to where you need to be by walking links between resources. By default, all resources are available immediately from the single root URL - by no means is a Xing application required (or even encourage) to make all its resource links available for the well known URL, but in early days we've found that it's very convenient.

# Is Not

A few things REST/hypemedia is not

* Pretty URLs: while the user interface value of the browser address bar is arguable (and major browser vendors are erasing its value as time goes on...) the URLs that Xing primarily concerns itself with (i.e. the URLs the frontend and backend exchange) are invisible to the user, and therefore irrelevant to the user interface or experience. They should, from the point of view of the frontend, be opaque identifiers and nothing more.

# An Architectural Style

Key to REST is that it's an approach to building distributed applications. It provides a series of constraints which guide the development of software such that it can fulfill a number of goals. Xing attempts to provide a framework that supports this style, partly by making a number of the decisions that REST leaves up to the designer. For instance, Xing specializes JSON to provide a hypermedia media type, and it simplifies the use of HTTP verbs.


