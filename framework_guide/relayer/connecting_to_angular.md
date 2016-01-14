# Angular and Relayer

The final step is to connect our top level resource definition to our well known URL, which is done in an A1AtScript config block:

```javascript
@Config('relayerProvider')
function setupResources(relayerProvider) {
  relayerProvider.createApi("resources", Resources, "http://www.example.com/")
}

// setup an a1atscript module
var AppModule = new Module('myApp', [RL, setupResources])
bootstrap(AppModule);
```

This will actually instantiate an Angular services called "resources" (first parameter) that will talk to the well known url "http://www.example.com/" and use the Resources class to interpret the response that comes back from the server.
