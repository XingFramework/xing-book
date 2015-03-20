# Chapter 1

## Promises and testing

```javascript


      // JDL: this promise setup has been refactored to a smoother pattern for
      // setting up promises for tests and working with Promises in general
      demoQuestion.complete.then((question) => {
        return question.screenerOptions.map((item) => { //a then resolution returns it's value so...
          return item.complete;
        })
      }).then( (optionPromises) => {       // this is the mapped set of promises from 2 lines up

        return Promise.all(optionPromises) // if a promise is returned, *it's* the new thing in the resolution chain

      }).then( values => {                 // so this the Promise.all's
                                           //   resolution: all the resolved values of the optionPromises
        //
        screenerOptions = values;          // (for easy use in tests below)


      }).then((resolved) => { done(); },   // this makes sure that, no matter
              (rejected) => { done(); });  //   if this all works or not, we call done() at the end.

```

## A Little Commentary

Notice how this makes clear that we do a series of small steps with
the callbacks - there's a continual march of time down the page - as
opposed to if we nest the callbacks (or the promise resolves), where
the march of time is sorta-to-the-right and sorta-down.

Promises are tricky. We should include a diagram of the happy/sad swimlanes.
