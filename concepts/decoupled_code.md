# Decoupled Code

[Logical Reality Design](https://lrdesign.com), the company that started Xing, is made up of developers who believe deeply in decoupling logic code from the framework used to structure that code.

While frameworks like Rails and Angular provide great value in tooling, conventions, and structure for your project, many bright people have observed that code that is deeply coupled to the framework is cumbersome and slow to test.

So at all levels of Xing, we have striven to build business logic in lightweight classes and objects that neither subclass nor depend on heavy framework constructs. In the Rails backend, Xing projects (by convention) put very little code in the models or controllers, with most db <-> JSON translation logic existing instead in lightweight Serialzers and Mappers.   Likewise, in the Angular frontend, Xing uses plain JavaScript objects for controllers and components.  They can be tested in isolation, with all Angular dependencies completely mocked.

We believe this represents the best compromise between the advantages and disadvantages of using frameworks to construct web applications.