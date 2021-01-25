# ApexCharts Card by @RomRider <!-- omit in toc -->

![Header](docs/Header.png)

This is higly customizable graph card for [Home-Assistant](https://www.home-assistant.io)'s Lovelace UI.<br/>

It is based on [ApexCharts.js](https://apexcharts.js) and offers most of the features of the library.

It is also inspired by the great [`mini-graph-card`](https://github.com/kalkih/mini-graph-card) by @kalkih


However, some things might be broken :grin:

## Table of Content <!-- omit in toc -->

- [Installation](#installation)
  - [HACS (recommended)](#hacs-recommended)
  - [Manual install](#manual-install)
  - [CLI install](#cli-install)
  - [Add resource reference](#add-resource-reference)
- [Using the card](#using-the-card)
  - [Main Options](#main-options)
  - [`series` Options](#series-options)
  - [`show` Options](#show-options)
  - [`header` Options](#header-options)
  - [`group_by` Options](#group_by-options)
  - [`func` Options](#func-options)
  - [Apex Charts Options Example](#apex-charts-options-example)
  - [Layouts](#layouts)
  - [Examples](#examples)

## Installation

### HACS (recommended)

This card is available in [HACS](https://hacs.xyz/) (Home Assistant Community Store).
<small>_HACS is a third party community store and is not included in Home Assistant out of the box._</small>

### Manual install

1. Download and copy `apexcharts-card.js` from the [latest release](https://github.com/RomRider/apexcharts-card/releases/latest) into your `config/www` directory.

2. Add the resource reference as decribed below.

### CLI install

1. Move into your `config/www` directory.

2. Grab `apexcharts-card.js`:

```
$ wget https://github.com/RomRider/apexcharts-card/releases/download/v0.0.1/apexcharts-card.js
```

3. Add the resource reference as decribed below.

### Add resource reference

If you configure Lovelace via YAML, add a reference to `apexcharts-card.js` inside your `configuration.yaml`:

```yaml
resources:
  - url: /local/apexcharts-card.js?v=0.0.1
    type: module
```

Else, if you prefer the graphical editor, use the menu to add the resource:

1. Make sure, advanced mode is enabled in your user profile (click on your user name to get there)
2. Navigate to Configuration -> Lovelace Dashboards -> Resources Tab. Hit orange (+) icon
3. Enter URL `/local/apexcharts-card.js` and select type "JavaScript Module".
4. Restart Home Assistant.

## Using the card

### Main Options

:warning: Since this card is in its debuts, you should expect breaking changes moving forward. :warning:

The card stricly validates all the options available (but not for the `apex_config` object). If there is an error in your configuration, it will tell you where and display a red error card.


[x] **means required**.

| Name | Type | Default | Since | Description |
| ---- | :--: | :-----: | :---: | ----------- |
| <ul><li>[x] `type`</li></ul> | string | | NEXT_VERSION | `custom:apexcharts-card` |
| <ul><li>[x] `series`</li></ul> | object | | NEXT_VERSION | See [series](#series-options) |
| `hours_to_show` | number | `24` | NEXT_VERSION | The span of the graph in hours (Use `0.25` for 15min for eg.) |
| `show` | object | | NEXT_VERSION | See [show](#show-options) |
| `cache` | boolean | `true` | NEXT_VERSION | Use in-browser data caching to reduce the load on Home Assistant's server |
| `stacked` | boolean | `false` | NEXT_VERSION | Enable if you want the data to be stacked on the graph |
| `layout` | string | | NEXT_VERSION | See [layouts](#layouts) |
| `header` | string | | NEXT_VERSION | See [header](#header-options) |
| `apex_config`| object | | NEXT_VERSION | Apexcharts API 1:1 mapping. You call see all the options [here](https://apexcharts.com/docs/installation/) --> `Options (Reference)` in the Menu. See [Apex Charts](#apex-charts-options-example) |



### `series` Options

| Name | Type | Default | Since | Description |
| ---- | :--: | :-----: | :---: | ----------- |
| <ul><li>[x] `entity`</li></ul> | string | | NEXT_VERSION | The `entity_id` of the sensor to display |
| `name` | string | | NEXT_VERSION | Override the name of the entity |
| `type` | string | `line` | NEXT_VERSION | `line`, `area` or `bar` are supported for now |
| `curve` | string | `smooth` | NEXT_VERSION | `smooth` (nice curve),  `straight` (direct line between points) or `stepline` (flat line until next point then straight up or down) |
| `extend_to_end` | boolean | `true` | NEXT_VERSION | If the last data is older than the end time displayed on the graph, setting to true will extend the value until the end of the timeline. Only works for `line` and `area` types. |
| `unit` | string | | NEXT_VERSION | Override the unit of the sensor |
| `group_by` | object | | NEXT_VERSION | See [group_by](#group_by-options) |


### `show` Options

| Name | Type | Default | Since | Description |
| ---- | :--: | :-----: | :---: | ----------- |
| `loading` | boolean | `true` | NEXT_VERSION | Displays a spinning icon while the data is loading/updating |

### `header` Options

| Name | Type | Default | Since | Description |
| ---- | :--: | :-----: | :---: | ----------- |
| `show` | boolean | `true` | NEXT_VERSION | Show or hide the header |
| `floating` | boolean | `false` | NEXT_VERSION | Makes the header float above the graph. Positionning will be supported later |

### `group_by` Options

| Name | Type | Default | Since | Description |
| ---- | :--: | :-----: | :---: | ----------- |
| `func` | string | `raw` | NEXT_VERSION | See [func](#func-options) |
| `duration` | string | `1h` | NEXT_VERSION | If `func` is **not** `raw` only. It build buckets of states over `duration` period of time. Doesn't work for months. Eg of valid values: `2h`, `1d`, `10s`, `25min`, `1h30`, ... |
| `fill` | string | `last` | NEXT_VERSION | If `func` is **not** `raw` only. If there is any missing value in the states history, `last` will replace it will the last non-empty state, `zero` will fill missing values with `0`, `null` will fill missing values with `null`

### `func` Options

| Name | Since | Description |
| ---- | :---: | ----------- |
| `raw` | NEXT_VERSION | Displays all the state history as known by Home Assistant |
| `avg` | NEXT_VERSION | Will return the average of all the states in each bucket |
| `min` | NEXT_VERSION | Will return the smallest state of each bucket |
| `max` | NEXT_VERSION | Will return the biggest state of each bucket |
| `last` | NEXT_VERSION | Will return the last state of each bucket |
| `first` | NEXT_VERSION | Will return the first state of each bucket |
| `sum` | NEXT_VERSION | Will return the sum of all the states in each bucket |
| `median` | NEXT_VERSION | Will return the median of all the states in each bucket |
| `delta` | NEXT_VERSION | Will return the delta between the biggest and smallest state in each bucket |

### Apex Charts Options Example

This is how you could change some options from ApexCharts as described on the [`Options (Reference)` menu entry](https://apexcharts.com/docs/installation/).

Some options might now work in the context of this card.

```yaml
type: custom:apexcharts-card
entities:
  - ...
apex_config:
  dataLabels:
    enabled: true
    dropShadow:
      enabled: true
```

### Layouts

For now, only `minimal` is supported: It will remove the grid, the axis and display the legend at the top. But you can use the `apex_config` to do whatever you want.

For code junkies, you'll find the default options I use in [`src/apex-layouts.ts`](blob/master/src/apex-layouts.ts)

### Examples

TBD.