## TODO List

#### General

* Create page with multiple panels each with a different instrument on a live chart
* The Great Renaming:
  - Indicator rendering funcs: `vis_` => `plot_`, Indicator names: `vis:*` => `plot:*`
  - `chart_setup` => `chart_template`, `setup` => `template`
  - `Collection` => `StreamGraph`, `indicator_collection` => `stream_graph`, `collection`/`coll` => `sgraph`
  - `vis:Price` => `vis:Candle`
  - `config/accounts` => `config/profiles`
  - Indicators: `fn:*` => `num:*`
  - `selection` => `dataset`, `inputs` => `features`, `tags` => `targets`
* Prevent bars from being skipped when no ticks arrive during their time span
* Replace `bower` with `npm` and ES6 modules on client
* Unit testing

#### Error handling and reporting

* Better handling and recovery from uncaught exceptions
* `ENOTFOUND` and other connection errors from `oanda.js` datasource module
* Precise error reporting for JSONOC errors originating from constructors

#### Backtesting

* Apply `chart_data_backing` to manage chart data beyond what is visible
* Allow bar-by-bar forward and backward replay of a backtesting session using timeline
* Add UI controls for choosing instruments and date range to backtest prior to running
* Add triangular buttons to prepend/append previous/next bars on loaded chart
* When selecting a trade on left panel, fade opacity of markings of any trades on chart other than one selected
* In `/backtest` rewrite `show_trade_on_chart()` to rely on `Input`s defined in collection's jsnc

#### Collections and indicator base

* Allow a collection to load other collections inline (composition)
* Allow a collection to extend another using `Extends("base_collection_name")`, allow overriding of inds and vars
* Add `Fork` jsnc that will auto-split a branch of indicators into parallel copies fed by different input streams
* Validate functions/properties defined on indicator definitions (e.g. `synch` and not `sync`)
* Find universal solution for propagating unique command and events without using list of previous UUIDs
* Report metrics on collection execution:
  - duration and throughput (bars/sec)
  - optionally record millisecond start/stop times for each indicator `update()` call (profiling)
* Optimize: track indicator defs with same inputs & params and consolidate them down to use a single instance
* Collection inputs can be merged and collated by date
* Define "delegates" as an async source type used in place of indicators (as new SrcType: Delegate)
* Add optional `on_bar_open()` and `on_bar_close()` functions to indicators
* Change constructor of indicator_instance to use parameter format: ([srcs], indname, param1, param2, ...)

#### Charting

* Create and implement `chart_data_backing` object to manage chart's data
  - Track data associated with all indicator markings on chart grouped by component
  - provide API for obtaining slices of the data sets
* Allow indicators to define labels and their position on data array to show values
* Validate that all indicators used within chart contain required vis_* functions on init
* Clip indicator markings that plot outside of corresponding component
* Preserve misc settings in browser's web storage (session storage)
* Split time cursor to highlight corresp. times on chart across different time frames (use canvas for cursor?)
* Create `tf:Trade` indicator to change time frame of trade events
* Create `canvas` version of chart, implement scrolling
* Use `vis_implement` property on indicators to apply rendering functions of another indicator
* Add `set_maxsize()` method 
* Add `throttle` option to indicators to delay redraws on slow rendering
* Indicator to render speed gradients behind tick charts
* Keyboard navigation with WASD:
  - `wasd`: pan up/down/left/right
  - shift + `a`|`d`: expand bars wider/narrower respectively
  - shift + `w`|`s`: zoom in/out respectively
  - `esc` to reset nav state to default

#### Dataprovider

* Create `fxcm` datasource to interact with FXCM broker
* Create `csv` datasource to read/write data to/from local CSV files
* Rename `fetch` action to `read`, `transmit_data` to `cmd` or `write`
* Bug: "Unexpected end of input" error when stream return no data (i.e. when forcing weekend day on chart)

#### JSONOC and JSONOC Support

* Use JSONOC for defining Charts in place of plain JSON
* In schema, find easy way to define methods on constructor's prototype
* Define `_get_children` method on Base prototype that will gather a list of all arguments recursively
* Fix polymorphism (import children of a constructor that are within same namespace)
* Create category of constructors to be used in place of indicator params where allowed (i.e. `Match()`)
* Create `mode-jsonoc.js` for ace editor to interpret JSONOC code and do syntax coloring

#### Indicator implementations

* Create indicator to detect divergences using ZigZag
* Create "visual" indicator for sound alerts (`vis:SoundAlert`), takes `bool` input
* Add `stream:TickThrottle` to handle and consolidate spiking tick volume
* Create `EntryOrderSim` indicator to easily simulate entry orders
* Create indicator to proxy trade commands to real broker and receive actual trade events via dataprovider
  - Apply 'live_only' option so indicator can only be used on live chart and not for backtesting
* `reg:Sin` for sinusoidal regressions

#### Machine learning

* `bool:SVM` and `dir:SVM` for creating SVMs and applying training datasets
* Complete dataset view/edit page with scatterplot matrix vis and chart view
* Store dataset configurations in postgres; edit base condition and feature sets using ace

#### ES6 [with [support status](https://kangax.github.io/compat-table/es6/)] / ESLint / Idiomatic

* Use `Promise`s where it makes better sense than `async` [supported]
* Use `let` and `const` in place of `var` where applicable [supported]
* Refactor functions to use tail calls where possible [unsupported]
* Use destructuring assignments where possible [supported]
* Use `_.create()` in place of `new` in non-indicator (internal) code
* Use [Rest and Spread](https://github.com/lukehoban/es6features#default--rest--spread) for function calls [supported]
* Use template strings [supported]:
  - Component titles
  - Label text
* Use Map/Set/WeakMap/WeakSet where applicable [supported]:
  - Maintaining active clients/connections in dataprovider and datasource modules
  - Use Set for `tsteps` property in indicator update event object
* Use `Proxy` to validate and interpret object access [supported]:
  - validating indicator `context` and `params`
  - use proxy in `params` to resolve JSONOC constructors
  - stream's `simple()` and `substream()` methods
  
#### UI Style

* Move `get_viewport()` function definitions into `uitools`
* Clean up CSS, finish defining light/dark themes

#### Trivial

* Use `morgan` for better logging
* Replace `grunt-bower-requirejs` with something simpler
* Debug on iOS/Andriod
