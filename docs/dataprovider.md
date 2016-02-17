## Dataprovider

The `dataprovider` module serves to provide data to a requesting client in a consistent manner, regardless of the source the client is requesting the data from.  The client issues a request for data from a particular `source` along with various other parameters, and `dataprovider` serves the request while handling all 

Types of data sources:

Datasource Properties:
- `use_interpreter` - (bool) `true` if the module should apply the corresponding interpreter indicator to the incoming data in order to convert each record's fields into their correct types.
- `single_stream` - (bool) `true` if the module only provides a single steam of data for the source specified, otherwise on of several stream may be offered depending on other parameters (instrument, timestep).  When `true`, all parameters provided by client will be forcefully associated to the data returned.

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
- `instrument` - Instrument (or array of instruments) that API modules should obtain data from, or instrument to associate with data from non-API modules (CSV, Databases).
- `timestep` - Timestep (or array of timesteps) to which the collection should be applied.
- `vars` - An object defining a set a variables and their values to be passed in to the collection, which are then referenced from the collection using the `Var()` constructor in place of literal values.
- `range` - Use a range of datetimes to obtain data between a start and end datetime.  Expects an array defined as `[<start>, <end>]`. or `[<start>]` where all values are of JSDate type, and `<end>` defaults to now if no value is provided.  If an object is provided, then described array is expected as each value, and corresponding timesteps on which to apply range as the key.
- `count` - If an integer, defines the number of last bars to get for all inputs, or if an object defines the count value for each individual input based on key.
- `windowsize` - Set the number of bars to populate for each timestep, and auto-calculate how many bars of each timestep are needed  (for creating static charts).
- `combine` - If set to true, then the bars belonging to timesteps with higher unit values will "swallow" the bars of those with lower values where datetime values overlap, so that the same time isn't loaded more than once across different timesteps. (default: `true`)
- `preload` - If true, it will determine for each timestep the minimum number of bars that need to be obtained before the overall resulting data can be considered valid, and it loads those bars first in addition to what it will load based on the other config options.  (default: `false`)
- `input_streams` - Pass in an object of streams with keys of inputs to match together with streams. (`combine` will be ignored)

##### Usage examples

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
  input_streams: {
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
