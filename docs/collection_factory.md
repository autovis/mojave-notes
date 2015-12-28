## Collection Factory

The `collection_factory` module is responsible for instantiating new `Collection` type objects, while handling most of the overhead involved during initialization of the collection.  It also infers how a collection's inputs should obtain their data based on config and environment settings.

Specifically it does the following before creating the collection:
- Loads and parses the JSONOC config file associated with collection
- Preloads all AMD modules belonging to all indicators referenced in collection
- Determines data sources to be used for all inputs defined in collection, and interprets how the data should be obtained from each, and ties them to the event chain of the indicators.

The purpose is to make creating/initializing a collection easier by allowing you to simply pass in a `config` object that describes how you want the collection to obtain its data, and in general how it should behave within the context where it will run.

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

- `source` - The name of the data source module to use to obtain the data. (required)
- `instrument` - Defines the instrument for which data should be obtained for modules that connect to brokers or other APIs.
- `vars` - An object defining a set a variables and their values to be passed in to the collection, which are then referenced from the collection using the `Var()` constructor in place of literal values.
- `range` - Use a range of datetimes to obtain data between a start and end datetime.  Expects an array defined as `[<start>, <end>]`. or `[<start>]` where all values are of JSDate type, and `<end>` defaults to now if no value is provided.  If an object is provided, then described array is expected as each value, and corresponding timesteps on which to apply range as the key.
- `count` - If an integer, defines the number of last bars to get for all inputs, or if an object defines the count value for each individual input based on key.
- `windowsize` - Set the number of bars to populate for each timestep, and auto-calculate how many bars of each timestep are needed  (for creating static charts).
- `combine` - If set to true, then the bars belonging to timesteps with higher unit values will "swallow" the bars of those with lower values where datetime values overlap, so that the same time isn't loaded more than once across different timesteps. (default: `true`)
- `preload` - If true, it will determine for each timestep the minimum number of bars that need to be obtained before the overall resulting data can be considered valid, and it loads those bars first in addition to what it will load based on the other config options.  (default: `false`)
- `streams` - Pass in an object of streams with keys of inputs to match together with streams. (`combine` will be ignored)

##### Config examples

Typical setup - A backtesting script wants to pass in range of data through the collection spanning over several months, and gather stats from the results.  This gets data from the `oanda` module, and initializes variables that are referenced in the Collection using Var() to define which timeframes to use for the lower and upper timesteps of the collection.  Only `m5` timesteps is 
```
{
  source: "oanda",
  instrument: "eurusd",
  vars: {
    ltf: "m5",  // var to define lower timeframe used
    htf: "H1"   // higher timeframe
  },
  config: {
    range: {
      m5: ['2015-06-01', '2015-12-01']
    }
  },
  preload: true
}
```

Bypass everything by instead directly passing in the input streams to use (calling code does all stream initialization work, streams are simply tied into the appropriate event chains):
```
{
  source: "oanda",
  instrument: "eurusd",
  streams: {
    tick: <Stream object>,
    m5: <Stream object>
  }
}
```

To dictate how many last bars of data to get on a per timestep basis.
```
{
  source: "oanda",
  instrument: "eurusd",
  count: {
    m5: 100,
    H1: 50
  },
  preload: true
}
```
