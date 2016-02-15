## TODO List

#### General

* ***Instrument/datetime lookup page - to pull up a chart for arbitrary instrument/datetime combinations***
* ***Adjust for time zone differences between browser, server, and broker***
* Rename `accounts` to `profiles`
* Prevent bars from being skipped when no ticks arrive during their time span
* Create page with multiple panels each with a different instrument on a live chart
* Unit testing

#### Error handling and reporting

* Better handling and recovery from uncaught exceptions
* `ENOTFOUND` and other connection errors from `oanda.js` datasource module
* Precise error reporting from JSONOC syntax errors

#### Backtesting

* Apply `chart_data_backing` to load chart data faster
* Add UI controls for choosing instruments and date range to backtest prior to running
* Add triangular buttons to prepend/append previous/next bars on loaded chart
* Use smooth gradient scale for pnl background color on trade table
* When selecting a trade on left panel, fade opacity of markings of any trades on chart other than one selected
* In `/backtest` rewrite `show_trade_on_chart()` to rely on `Input`s defined in collection's jsnc

#### Collections and indicator base

* Allow a collection to load other collections inline (composition)
* Allow a collection to extend another using `Extends("base_collection_name")`, allow overriding of inds and vars
* Allow streams to support JSONOC-based types
* Validate functions/properties defined on indicator definitions (e.g. `synch` and not `sync`)
* Find universal solution for propagating unique command and events without using list of previous UUIDs
* Revisit `deferred` branch to correctly build collections indepedent of how sources are ordered on JSONOC config
* Report metrics on collection execution:
  - duration and throughput (bars/sec)
  - optionally record millisecond start/stop times for each indicator `update()` call
* Optimize: track indicator defs with same inputs & params and consolidate them down to same instance

#### Charting

* Create and implement `chart_data_backing` object to manage chart's data
  - Track data associated with all indicator markings on chart grouped by component
  - provide API for obtaining slices of the data sets
* Allow selecting bars on chart
* Allow indicators to define labels to show values
* Validate that all indicators used within chart contain required vis_* functions on init
* Clip indicator markings that plot outside of corresponding component
* Preserve control values and misc settings in browser's web storage (session storage)
* Split time cursor to highlight corresp. times on chart across different time frames (use canvas for cursor?)
* Create `tf:Trade` indicator to change time frame of trade events
* Create canvas implementation of chart
* Use `vis_implement` property on indicators to apply rendering functions of another indicator

#### Dataprovider

* Make clients extend EventEmitter2 to emit error events
* Create `fxcm` datasource to interact with FXCM broker
* Create `csv` datasource to read/write data to/from local CSV files
* Rename `fetch` action to `read`, `transmit_data` to `cmd` or `write`

#### JSONOC and JSONOC Support

* Use JSONOC for defining Charts in place of plain JSON
* In schema, find easy way to define methods on constructor's prototype
* Define `_get_children` method on Base prototype that will gather a list of all arguments recursively
* Extend stream types to support JSONOC objects that extend "Bar"

#### Indicator implementations

* Fix ZigZag indicator and create indicator to detect divergences
* Create "visual" indicator for sound alerts (`vis:SoundAlert`), takes `bool` input
* Add `stream:TickThrottle` to handle and consolidate spiking tick volume
* Create `EntryOrderSim` indicator to easily simulate entry orders
* Create indicator to proxy trade commands to real broker and receive actual trade events via dataprovider
  - Apply 'live_only' option so indicator can only be used on live chart and not for backtesting

#### ES6 [with [support status](https://kangax.github.io/compat-table/es6/)] / ESLint / Idiomatic

* Use `Promise`s where it makes better sense than `async` [supported]
* Use arrow function notation for small, one-liner functions `(x => x + 1)` [supported]
* Use `let` and `const` in place of `var` where applicable [supported]
* Refactor functions to use tail calls where possible [unsupported]
* Use destructuring assignments where possible [unsupported]
* Use `_.create()` in place of `new` in non-indicator (internal) code
* Use [Rest and Spread](https://github.com/lukehoban/es6features#default--rest--spread) for function calls [node unsupported]
* Use template strings [supported]:
  - Component titles
  - Label text
* Use Map/Set/WeakMap/WeakSet where applicable [supported]:
  - Maintaining active clients/connections in dataprovider and datasource modules
* Use `Proxy` to create objects [chrome/node support soon]:
  - stream's `simple()` and `substream()` methods
  
#### UI Style

* Move `get_viewport()` function definitions into `uitools`
* Clean up CSS, finish defining light/dark themes

#### Trivial

* Use `morgan` for better logging
* Replace `grunt-bower-requirejs` with something simpler
* Debug on iOS/Andriod
* Update lodash:
  - replace `_.object()` with `_.fromPairs()`
  - replace `_.pairs()` with `_.toPairs()`
  - replace `_.unique` with `_.uniq`
  - replace `_.all` with `_.every`
