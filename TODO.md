## TODO List

#### General

* Instrument/datetime lookup page - to pull up a chart for arbitrary instrument/datetime combinations
* Adjust for time zone differences between browser, server, and broker
* Use `morgan` for better logging
* Rename `accounts` to `profiles`
* Prevent bars from being skipped when no ticks arrive during their time span
* Debug on iOS/Andriod
* Create page with multiple panels each with a different instrument on a live chart

#### Backtesting

* Apply `chart_data_backing` to load chart data faster
* Add UI controls for choosing instruments and date range to backtest prior to running
* Add triangular buttons to prepend/append previous/next bars on loaded chart
* Use smooth gradient scale for pnl background color on trade table
* When selecting a trade on left panel, fade opacity of markings of any trades on chart other than one selected

#### Collections and indicator base

* Rewrite collection definitions to use object constructors and allow metadata options
* Allow a collection to load other collections inline
* Allow indicators to have generic types (e.g. `^a`)
* Validate functions/properties defined on indicator definitions (e.g. `synch` and not `sync`)
* Support `$` directives on indicator sources and parameters, propagate changes on dependencies

#### Charting

* Create and implement `chart_data_backing` object to manage chart's data
* Allow selecting bars on chart
* Allow indicators to define labels to show values
* Validate that all indicators used within chart contain required vis_* functions on init
* Clip indicator markings that go outside of corresponding component
* Preserve control values and misc settings in browser's web storage (session storage)
* Split time cursor to highlight corresp. times on chart across different time frames
* Create tf:Trade indicator to change time frame of trade events

#### Dataprovider

* Create `fxcm` datasource to interact with FXCM broker
* Create `csv` datasource to read/write data to/from local CSV files

#### Indicator implementations

* Fix ZigZag indicator
* Create "visual" indicator for sound alerts (`vis:SoundAlert`), takes `bool` input
* Add `stream:TickThrottle` to handle and consolidate spiking tick volume
* Create `EntryOrderSim` indicator to easily simulate entry orders
* Create indicator to proxy trade commands to real broker and receive actual trade events via dataprovider
  - Apply 'live_only' option so indicator can only be used on live chart and not for backtesting

#### Style

* Move `get_viewport()` function definitions into `uitools`
* Clean up CSS, finish defining light/dark themes
