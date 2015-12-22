## Collection Factory

The `collection_factory` module is responsible for instantiating new `Collection` objects, and handling the overhead involved in initializing the collection and attaching data sources to its inputs.  

Specifically it does the following before creating the collection:

- Loads and parses the JSONOC config file associated with collection
- Preloads all AMD modules belonging to all indicators referenced in collection 
- Determines data sources to be used for all inputs defined in collection, and interprets how the data should be obtained from each

The module allows you to supply a `config` object to describe how you want the collection to obtain its data, and in general how it should operate in the context where it will be running.

#### Methods

- `.create(path, config, callback)`
  Create a new collection
    - `path` - Path to the name of collection
    - `config` - Configuration for how to create collection (see Config section below)
    - `callback` - The callback called when collection is created: `callback(err, collection)`

- `.is_collection(obj)`
  Returns `true` if obj is a collection instance.
    - `obj` - Object to test whether it is a collection

- `.get_dataprovider()`

#### Config

The `config` parameter passed in the `.create()` method serve to provide parameters for how the collection should behave.   

##### Config examples

Get data from the `oanda` module, using the `instrument` parameter, and initializing variables that are referenced in the Collection using Var()

Basic setup
```
{
  source: "oanda",
  instrument: "eurusd",
  vars: {
    ltf: "m5",
    htf: "H1"
  }
}
```

For displaying a static chart
```
{
  windowsize: 100
}
```
