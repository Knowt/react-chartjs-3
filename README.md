[![version](https://badge.fury.io/js/%40iftek%2Freact-chartjs-3.svg)](https://www.npmjs.com/package/@iftek/react-chartjs-3)
[![downloads](https://img.shields.io/npm/dt/@iftek/react-chartjs-3.svg?style=flat-square)](https://npm-stat.com/charts.html?package=@iftek/react-chartjs-3&from=2021-03-24)
[![license](https://img.shields.io/github/license/mashape/apistatus.svg?style=flat-square)](http://opensource.org/licenses/MIT)

# @iftek/react-chartjs-3

React wrapper for [Chart.js 3](http://www.chartjs.org/docs/#getting-started)

- Fork from [react-chartjs-2](https://github.com/reactchartjs/react-chartjs-2)
- Merge [#599 3.x Migration](https://github.com/reactchartjs/react-chartjs-2/pull/599) from react-chartjs-2
- Bump dependency [Chart.js](https://github.com/chartjs/Chart.js) version to 3.0.2
- Implement v3 function [getElementsAtEventForMode](https://github.com/chartjs/Chart.js/blob/master/docs/developers/api.md#getelementsateventformodee-mode-options-usefinalposition)  

Open for PRs and contributions!
## Demo & Examples

Live demo: [reactchartjs.github.io/react-chartjs-2](https://reactchartjs.github.io/react-chartjs-2/)

To build the examples locally, run:

```bash
npm install
npm start
```

Then open [`localhost:8000`](http://localhost:8000) in a browser.

## Demo & Examples via React Storybook

We have to build the package, then you can run storybook.

```bash
npm run build
npm run storybook
```

Then open [`localhost:6006`](http://localhost:6006) in a browser.


## Installation via NPM

```bash
npm install --save @iftek/react-chartjs-3 chart.js
```

## Installation via YARN

```bash
yarn add @iftek/react-chartjs-3 chart.js
```


## Usage

Check example/src/components/* for usage.

```js
import { Doughnut } from '@iftek/react-chartjs-3';

<Doughnut data={...} />
```

### Properties

* data: (PropTypes.object | PropTypes.func).isRequired,
* width: PropTypes.number,
* height: PropTypes.number,
* id: PropTypes.string,
* legend: PropTypes.object,
* options: PropTypes.object,
* redraw: PropTypes.bool,
* getElementsAtEventForMode: PropTypes.func

### Custom size
In order for Chart.js to obey the custom size you need to set `maintainAspectRatio` to false, example:

```js
<Bar
  data={data}
  width={100}
  height={50}
  options={{ maintainAspectRatio: false }}
/>
```

### Chart.js instance
Chart.js instance can be accessed by placing a ref to the element as:

```js
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.chartReference = React.createRef();
  }

  componentDidMount() {
    console.log(this.chartReference); // returns a Chart.js instance reference
  }

  render() {
    return (<Doughnut ref={this.chartReference} data={data} options={options} />)
  }
}
```

### Getting context for data generation
Canvas node and hence context, that can be used to create CanvasGradient background,
is passed as argument to data if given as function:

This approach is useful when you want to keep your components pure.

```js
render() {
  const data = (canvas) => {
    const ctx = canvas.getContext("2d")
    const gradient = ctx.createLinearGradient(0,0,100,0);
    ...
    return {
      ...
      backgroundColor: gradient
      ...
    }
  }

  return (<Line data={data} />)
}
```

### Chart.js Defaults
Chart.js defaults can be set by importing the `defaults` object:

```javascript
import { defaults } from '@iftek/react-chartjs-3';

// Disable animating charts by default.
defaults.global.animation = false;
```

If you want to bulk set properties, try using the [lodash.merge](https://lodash.com/docs/#merge) function. This function will do a deep recursive merge preserving previously set values that you don't want to update.

```js
import { defaults } from '@iftek/react-chartjs-3';
import merge from 'lodash.merge';
// or
// import { merge } from 'lodash';

merge(defaults, {
  global: {
    animation: false,
    line: {
      borderColor: '#F85F73',
     },
  },
});
```

### Chart.js object

You can access the internal Chart.js object to register plugins or extend charts like this:

```JavaScript
import { Chart } from '@iftek/react-chartjs-3';

componentWillMount() {
  Chart.pluginService.register({
    afterDraw: function (chart, easing) {
      // Plugin code.
    }
  });
}
```

### Scatter Charts

If you're using Chart.js 2.6 and below, add the `showLines: false` property to your chart options. This was later [added](https://github.com/chartjs/Chart.js/commit/7fa60523599a56255cde78a49e848166bd233c6e) in the default config, so users of later versions would not need to do this extra step.

### Events

#### getElementsAtEventForMode (function)

A function to be called when mouse clicked on chart elements, will return all element at that point as an array. [Check](https://github.com/chartjs/Chart.js/blob/master/docs/developers/api.md#getelementsateventformodee-mode-options-usefinalposition)

```js
{
  getElementsAtEventForMode: (elems) => {},
  // `elems` is an array of chartElements
}
```
#### getDatasetAtEvent (function)

Looks for the element under the event point, then returns all elements from that dataset. This is used internally for 'dataset' mode highlighting [Check](https://github.com/chartjs/Chart.js/blob/master/docs/docs/developers/api.md#getdatasetatevente)

```js
{
  getDatasetAtEvent: (dataset) => {}
  // `dataset` is an array of chartElements
}
```

### Working with Multiple Datasets

You will find that any event which causes the chart to re-render, such as hover tooltips, etc., will cause the first dataset to be copied over to other datasets, causing your lines and bars to merge together. This is because to track changes in the dataset series, the library needs a `key` to be specified - if none is found, it can't tell the difference between the datasets while updating. To get around this issue, you can take these two approaches:

1. Add a `label` property on each dataset. By default, this library uses the `label` property as the key to distinguish datasets.
2. Specify a different property to be used as a key by passing a `datasetKeyProvider` prop to your chart component, which would return a unique string value for each dataset.

## Development (`src`, `lib` and the build process)

**NOTE:** The source code for the component is in `src`. A transpiled CommonJS version (generated with Babel) is available in `lib` for use with node.js, browserify and webpack. A UMD bundle is also built to `dist`, which can be included without the need for any build system.

To build, watch and serve the examples (which will also watch the component source), run `npm start`. If you just want to watch changes to `src` and rebuild `lib`, run `npm run watch` (this is useful if you are working with `npm link`).


## License

[MIT Licensed](/LICENSE.md)
Copyright (c) 2021 Iftek

