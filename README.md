<p>
  <img width="100%" src="https://assets.solidjs.com/banner?background=tiles&project=solid-ag-grid" alt="solid-ag-grid">
</p>

## AG Grid Solid Component

Solid AG Grid is a fully-featured and highly customizable JavaScript data grid.
It delivers [outstanding performance](https://www.ag-grid.com/example?utm_source=solid-ag-grid-readme&utm_medium=repository&utm_campaign=github#/performance/1), has no 3rd party dependencies and integrates smoothly with Solid as Solid Component. Here's how our grid looks like with multiple filters and grouping enabled:

![Image of AG Grid showing filtering and grouping enabled.](./github-grid-demo.jpg "AG Grid demo")

When using AG Grid with Solid, all of the grid's core rendering (headers, rows, cells etc) is rendered using Solid. AG Grid Solid shares the same 'business logic layer' as the other AG Grid versions (React, Angular, Vue, or just JavaScript). This means the features of AG Grid Solid are identical to the features in AG Grid's other framework flavours. However because the rendering is done 100% in Solid, the grid works as a native Solid Component.

AG Grid Solid is NOT a JavaScript component with a thin Solid wrapper. AG Grid is the Real Deal when it comes to a Data Grid Implementation for SolidJS.

## Features

Besides the standard set of features you'd expect from any grid:

- Column Interactions (resize, reorder, and pin columns)
- Pagination
- Sorting
- Row Selection

Here are some of the features that make AG Grid stand out:

- Grouping / Aggregation \*
- Accessibility support
- Custom Filtering
- In-place Cell Editing
- Records Lazy Loading \*
- Server-Side Records Operations \*
- Live Stream Updates
- Hierarchical Data Support & Tree View \*
- Customizable Appearance
- Customizable Cell Contents
- State Persistence
- Keyboard Navigation
- Data Export to CSV
- Data Export to Excel \*
- Excel-like Pivoting \*
- Row Reordering
- Copy / Paste
- Column Spanning
- Pinned Rows
- Full Width Rows
- Integrated Charting
- Sparklines

\* The features marked with an asterisk are available in the [enterprise version](https://www.ag-grid.com/license-pricing?utm_source=solid-ag-gridct-readme&utm_medium=repository&utm_campaign=github) only.

Check out [developers documentation](https://www.ag-grid.com/react-data-grid/solidjs/) for a complete list of features or visit [our official docs](https://www.ag-grid.com/features-overview?utm_source=solid-ag-grid-readme&utm_medium=repository&utm_campaign=github) for tutorials and feature demos.

You may also read the [Solid specific documentation](https://ag-grid.com/react-data-grid/solidjs/).

## Usage Overview

Use the setup instructions below or go through [a 5-minute-quickstart guide](https://www.ag-grid.com/react-grid?utm_source=ag-grid-react-readme&utm_medium=repository&utm_campaign=github).

#### Installation

```
npm i --save ag-grid-community solid-ag-grid
// or
yarn add ag-grid-community solid-ag-grid
// or
pnpm add ag-grid-community solid-ag-grid
```

#### Import the grid and styles

```ts
import type { Component } from "solid-js";
import AgGridSolid from "ag-grid-solid";

import "ag-grid-community/styles/ag-grid.css";
import "ag-grid-community/styles/ag-theme-alpine.css";
```

### Render the grid as the `AgGridSolid` child component

```ts
const App: Component = () => {
  const columnDefs = [
    { field: 'make' },
    { field: 'model' },
    { field: 'price' },
  ];
  const rowData = [
    { make: 'Toyota', model: 'Celica', price: 35000 },
    { make: 'Ford', model: 'Mondeo', price: 32000 },
    { make: 'Porsche', model: 'Boxster', price: 72000 },
  ];
  const defaultColDef = {
    flex: 1,
  };
  return (
    <div class="ag-theme-alpine" style={{ height: '100%' }}>
      <AgGridSolid
        columnDefs={columnDefs}
        rowData={rowData}
        defaultColDef={defaultColDef}
      />
    </div>
  );
};

export default App;
```

## Grid Component

Once the Solid grid component is imported, it can then be inserted into the Solid application using JSX.

```jsx
<AgGridSolid
    rowData={...}
    columnDefs={...}
/>
```

It's best to place the grid component inside another DOM element that has a set size. The grid will then fill the size of the parent element. You also need to import CSS files for a) the core CSS which is mandatory and b) a grid theme which is optional. The theme also needs to be specified as a CSS class in a parent element to the grid.

```jsx
import AgGridSolid from 'ag-grid-solid';

import 'ag-grid-community/styles/ag-grid.css'; // grid core CSS
import "ag-grid-community/styles/ag-theme-quartz.css"; // optional theme

const MySolidApp = ()=> {
  return (
    // set fixed size to parent div, and apply grid theme ag-theme-quartz
    <div style={{height: '500px'}} class="ag-theme-quartz">
      <AgGridSolid
        rowData={...}
        columnDefs={...}
      />
    </div>
  );
};

```

## Binding Properties

You can use [Grid Properties](./grid-options/), either bind Solid Signals (for changing properties) or directly (if static properties). [Grid Events](./grid-events/) are also bound via properties.

```jsx
import AgGridSolid from "ag-grid-solid";

import "ag-grid-community/styles/ag-grid.css"; // grid core CSS
import "ag-grid-community/styles/ag-theme-quartz.css"; // optional theme

const MySolidApp = () => {
  // use signal, as row data will change
  const [rowData, setRowData] = createSignal();
  // if columns will change, best use a signal, however if column definitions
  // are static, we don't need to use a signal
  const columnDefs = [{ field: "name" }, { field: "age" }];
  // event listener
  const selectionChangedCallback = (e) => {
    console.log("selection has changed", e);
  };
  return (
    <div style={{ height: "500px" }} class="ag-theme-quartz">
      <AgGridSolid
        rowData={rowData()} // use signal
        columnDefs={columnDefs} // no signal
        rowSelection="single" // no signal, inline
        onSelectionChanged={selectionChangedCallback} // listen for grid event
      />
    </div>
  );
};
```

## Grid API

The grid API is accessed as a Solid Ref.

```jsx
const MySolidApp = ()=> {
  let grid; // ref for the grid
  const myAction = ()=> {
    // use grid api
    gridRef.api.selectAll();
    // use grid column api
    gridRef.api.applyColumnState(...);
  };
  return (
    <div style={{height: '500px'}} class="ag-theme-quartz">
      <AgGridSolid
        rowData={...}
        columnDefs={...}
        ref={gridRef}
      />
    </div>
  );
};
```

If using TypeScript, the type to use is `AgGridSolidRef`.

```jsx
import AgGridSolid, {AgGridSolidRef} from 'ag-grid-solid';

const MySolidApp = ()=> {
  let grid: AgGridSolidRef;
  // ...
};
```

## Examples

### Custom Cells

The Custom Cells examples demonstrates using [Cell Renderer](./component-cell-renderer/) to customise the cells in the Age Column. Note that the Cell Renderer is a standard Solid Component and is set onto the grid using the Column Definitions.

[Open in StackBlitz](https://stackblitz.com/edit/solidjs-template-z3ncqk?embed=1&file=src/App.tsx)

See [Cell Renderers](./component-cell-renderer/) for full details on creating React Cell Renderers and then apply this knowledge to Solid.

### Using Cell Editors

Below is an example showing different types of Solid [Cell Editors](./cell-editors/). Edit any cell by double clicking the mouse. The Gold and Silver Columns use custom Solid Components. Gold edits inside the cell and and Silver edits in a popup (`cellEditorPopup=true`).

A custom Cell Editor component requires the component to expose an API from the componet to the grid. Using React this is done using an Imperative Handle. In Solid this is done by calling `ref(api)` on the props.

```jsx
const api = {
    ...
};

props.ref(api);
```

[Open in StackBlitz](https://stackblitz.com/edit/solidjs-template-bhhxsm?embed=1&file=src/App.tsx)

See [Cell Editors](./cell-editors/) for full details on creating React Cell Editors and then apply this knowledge to Solid.

### Customising Headers

This example demonstrates custom Column Headers and Column Group Headers using Solid components.

[Open in StackBlitz](https://stackblitz.com/edit/solidjs-template-wnpr7s?embed=1&file=src/App.tsx)

See Column Headers and Column Group Headers for full details on creating these components with React and then apply this knowledge to Solid.

### Advanced Grid Features

Below is an example of AG Grid Solid showing more advanced features such as [Row Grouping](./grouping/), [Range Selection](./range-selection/) and [Integrated Charting](./integrated-charts/).

[Open in StackBlitz](https://stackblitz.com/edit/solidjs-template-qsmpa3?embed=1&file=src/App.tsx)

### Master Detail

When the master grid is AG Grid Solid, then the detail grids also use AG Grid Solid. In the example both Master and Detail grids are using Solid Cell Renderers.

[Open in StackBlitz](https://stackblitz.com/edit/solidjs-template-vt3cco?embed=1&file=src/App.tsx)

### Modules

If using [AG Grid Modules](./modules/), the dependencies will be different.

```jsx
"dependencies": {
    "@ag-grid-community/core": "~{% $agGridVersion %}",
    "@ag-grid-community/client-side-row-model": "~{% $agGridVersion %}",
    "@ag-grid-community/solid": "~{% $agGridVersion %}",
   ...
```

And the import will also be different.

```jsx
import AgGridSolid from "@ag-grid-community/solid";
```

The example below shows an AG Grid Solid example using modules.

## Contributing

AG Grid is developed by a team of co-located developers in London. If you want to join the team check out our [jobs listing](https://www.ag-grid.com/ag-grid-jobs-board?utm_source=solid-ag-grid-readme&utm_medium=repository&utm_campaign=github) or send your application to info@ag-grid.com.

## License

This project is licensed under the MIT license. See the [LICENSE file](./LICENSE.txt) for more info.
