## TODO List

#### General / Trivial

* ***Instrument/datetime lookup page - to pull up a chart for arbitrary instrument/datetime combinations***
* ***Adjust for time zone differences between browser, server, and broker***
* Rename `accounts` to `profiles`
* Prevent bars from being skipped when no ticks arrive during their time span
* Create page with multiple panels each with a different instrument on a live chart
* Debug on iOS/Andriod
* Use `morgan` for better logging

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

#### Collections and indicator base

* Allow a collection to load other collections inline
* Allow streams to support JSONOC-based types
* Validate functions/properties defined on indicator definitions (e.g. `synch` and not `sync`)
* Find universal solution for propagating unique command and events without using list of previous UUIDs
* Revisit `deferred` branch to correctly build collections indepedent of how sources are ordered on JSONOC config 

#### Charting

* Create and implement `chart_data_backing` object to manage chart's data
* Allow selecting bars on chart
* Allow indicators to define labels to show values
* Validate that all indicators used within chart contain required vis_* functions on init
* Clip indicator markings that plot outside of corresponding component
* Preserve control values and misc settings in browser's web storage (session storage)
* Split time cursor to highlight corresp. times on chart across different time frames (use canvas for cursor?)
* Create `tf:Trade` indicator to change time frame of trade events 

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

#### Style

* Move `get_viewport()` function definitions into `uitools`
* Clean up CSS, finish defining light/dark themes
