## Collection Factory

The `collection_factory` module is responsible for instantiating new `Collection` objects, and handling most of the overhead involved in initializing the collection and attaching data sources to its inputs.  

Specifically it does the following before creating the collection:
- Loads and parses the JSONOC config file associated with collection
- Preloads all AMD modules belonging to all indicators referenced in collection 
- Determines data sources to be used for all inputs defined in collection, and interprets how the data should be obtained from each

Essentially this modules makes using a collection much more user-friendly by allowing you to simply pass in a `config` object that describes how you want the collection to obtain its data, and in general how it should operate in the context where it will be running.

#### Methods

- `.create(path, config, callback)`
  Create a new collection
    - `path` - Path to the name of collection
    - `config` - Configuration for how to create collection (see Config section below)
    - `callback` - The callback called when collection is created: `callback(err, collection)`

- `.is_collection(obj)`
  Returns `true` if `obj` is an instance of a collection

- `.get_dataprovider()`
  Returns dataprovider module being used

- `.set_dataprovider(dataprovider)`
  Sets the dataprovider to be used

#### Config

A `config` object is passed to the `.create()` method in order to define how the collection will behave.   

- `source` - The name of the data source module to use to obtain the data (required)
- `instrument` - Defines the instrument for which data should be obtained for modules that connect to brokers or other APIs
- `vars` - An object defining a set a variables and their values to be passed in and used within the collection
- `count` - If an integer, defines the number of bars to get for all inputs, or if an object defines the count for each individual input based on key
- `windowsize` - Set the number of bars to populate for each timestep, and auto-calculate how many bars of each timestep are needed (used for creating static charts)
- `inputs` - Pass in an object of streams with keys of inputs to match with

##### Config examples

Get data from the `oanda` module, using the `instrument` parameter, and initializing variables that are referenced in the Collection using Var()

Basic setup:
```
{
  source: "oanda",
  instrument: "eurusd",
  vars: {
    ltf: "m5",  // var to define lower timeframe used
    htf: "H1"   // higher timeframe
  }
}
```

For displaying a static chart that is 100 bars wide:
```
{
  windowsize: 100
}
```

Using predefined input streams:
```
{
  source: "oanda",
  
  ...
  
  inputs: {
    tick: <Stream object>,
    m5: <Stream object>
  }
}
```
