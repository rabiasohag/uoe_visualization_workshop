# Building the Visualisation

This guide explains the main parts of the workshop visualisation.

It is designed to sit alongside the live demonstration. You do not need to understand every line of code immediately. The aim is to help you recognise what each part of the file is doing and where you can make small changes during the workshop.

The visualisation uses:

- **HTML** to structure the page;
- **CSS** to style the page;
- **JavaScript** to load data and build the visualisation;
- **Leaflet** to create an interactive map;
- **D3.js** to create a timeline chart.

---

## Contents

- [What you will build](#what-you-will-build)
- [Before you start](#before-you-start)
- [Project files](#project-files)
- [The main file: index.html](#the-main-file-indexhtml)
- [Part 1: HTML page structure](#part-1-html-page-structure)
- [Part 2: Loading external libraries](#part-2-loading-external-libraries)
- [Part 3: Page layout and styling](#part-3-page-layout-and-styling)
- [Part 4: Creating the Leaflet map](#part-4-creating-the-leaflet-map)
- [Part 5: Adding map tiles](#part-5-adding-map-tiles)
- [Part 6: Loading the data](#part-6-loading-the-data)
- [Part 7: Adding markers to the map](#part-7-adding-markers-to-the-map)
- [Part 8: Preparing the timeline data](#part-8-preparing-the-timeline-data)
- [Part 9: Creating the SVG chart area](#part-9-creating-the-svg-chart-area)
- [Part 10: Creating scales and axes](#part-10-creating-scales-and-axes)
- [Part 11: Drawing the line chart](#part-11-drawing-the-line-chart)
- [Part 12: Adding points to the chart](#part-12-adding-points-to-the-chart)
- [Part 13: Interactivity](#part-13-interactivity)
- [Debugging tips](#debugging-tips)
- [Key terms](#key-terms)
- [Next step](#next-step)

---

## What you will build

In this workshop, you will work with a web-based visualisation that combines:

- a map showing places from the dataset;
- a timeline showing records by entry year;
- data loaded from a JSON file;
- basic interaction through map markers and popups.

The current starter file creates two main visual areas:

```text
Map area              D3 visualisation area
---------             ----------------------
Leaflet map           SVG timeline chart
```

The map and chart are shown side by side in the browser.

---

## Before you start

Before working through this page, make sure you have completed:

[Setup before the workshop](./Part_00_Pre_Workshop_Setup.md)

You should have:

- your own fork or copy of the workshop repository;
- the project open in VS Code;
- Live Preview or Live Server installed;
- `index.html` opening successfully in your browser.

> **Important:** Open the project using Live Preview or Live Server.  
> Do not open `index.html` by double-clicking it, because the project needs to load data from the `data/` folder.

---

## Project files

The project currently contains files similar to this:

```text
uoe_visualization_workshop/
  index.html
  data/
    women_doctors_geo.json
```

The exact structure may change as the workshop materials develop.

The most important file for this part of the workshop is:

```text
index.html
```

This file contains:

- the page structure;
- links to D3.js and Leaflet;
- the map container;
- the chart container;
- the JavaScript code that loads the data and builds the visualisation.

---

## The main file: index.html

Open this file in VS Code:

```text
index.html
```

This is the main webpage. It contains three broad parts:

| Part | What it does |
|---|---|
| `<head>` | Loads metadata, styles, and external libraries |
| `<body>` | Contains the visible page structure |
| `<script>` | Contains the JavaScript that builds the map and chart |

---

## Part 1: HTML page structure

HTML gives the page its structure.

The starter file begins with a standard HTML document structure:

```html
<!doctype html>
<html lang="en">
  <head>
    ...
  </head>

  <body>
    ...
  </body>
</html>
```

The two most important parts are:

| HTML section | Purpose |
|---|---|
| `<head>` | Information for the browser, such as page title, CSS, and script links |
| `<body>` | Content that appears on the page |

In this workshop, the visible page content is inside the `<body>`.

The page has:

```html
<header>
  <h1>Digital Humanities Visualization</h1>
  <p>D3.js + Leaflet starter project</p>
</header>
```

This creates the title area at the top of the page.

It also has:

```html
<main>
  <div id="map"></div>

  <div id="viz">
    <h2>D3 Visualization</h2>
    <svg id="chart"></svg>
  </div>
</main>
```

This creates the two main visualisation areas:

| Element | Purpose |
|---|---|
| `<div id="map"></div>` | The container where Leaflet will place the map |
| `<svg id="chart"></svg>` | The drawing area where D3 will place the chart |

---

## Part 2: Loading external libraries

The visualisation uses two JavaScript libraries:

- **Leaflet** for the map;
- **D3.js** for loading and visualising data.

These are loaded in the `<head>` of the page.

```html
<!-- Leaflet CSS -->
<link
  rel="stylesheet"
  href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
/>

<!-- D3 -->
<script src="https://d3js.org/d3.v7.min.js"></script>

<!-- Leaflet JS -->
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
```

The Leaflet CSS controls how the map looks.

The D3 script makes D3 functions available, such as:

```js
d3.select()
d3.json()
d3.rollups()
d3.scaleLinear()
d3.axisBottom()
d3.line()
```

The Leaflet script makes Leaflet functions available, such as:

```js
L.map()
L.tileLayer()
L.marker()
```

---

## Part 3: Page layout and styling

The visual layout is controlled by CSS inside the `<style>` block.

For example:

```css
main {
  display: grid;
  grid-template-columns: 1fr 1fr;
  height: calc(100vh - 70px);
}
```

This creates a two-column layout:

- the map appears on the left;
- the D3 visualisation appears on the right.

The map area is styled with:

```css
#map {
  height: 100%;
  width: 100%;
}
```

Leaflet maps need a visible height. If the map container has no height, the map may not appear.

The SVG chart is styled with:

```css
svg {
  width: 100%;
  height: 600px;
  border: 1px solid #ddd;
}
```

This makes the chart area visible and gives it a border.

---

## Part 4: Creating the Leaflet map

The map is created with this line:

```js
const map = L.map("map").setView([55.9533, -3.1883], 5);
```

This does three things:

| Code | Meaning |
|---|---|
| `L.map("map")` | Creates a Leaflet map inside the element with `id="map"` |
| `[55.9533, -3.1883]` | Sets the starting centre point of the map |
| `5` | Sets the starting zoom level |

The coordinates used here point to Edinburgh.

You can try changing the zoom level:

```js
const map = L.map("map").setView([55.9533, -3.1883], 6);
```

A larger number zooms in.  
A smaller number zooms out.

---

## Part 5: Adding map tiles

The map needs background tiles. These are the map images that show land, roads, labels, and other base map details.

The starter file uses OpenStreetMap tiles:

```js
L.tileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
  attribution: "&copy; OpenStreetMap contributors",
}).addTo(map);
```

This adds the tile layer to the map.

The attribution line credits the tile provider.

---

## Part 6: Loading the data

The data is loaded with D3:

```js
d3.json("data/women_doctors_geo.json")
  .then((data) => {
    console.log(data);

    // Code using the data goes here
  })
  .catch((error) => {
    console.error("Error loading JSON:", error);
  });
```

This means:

| Code | Meaning |
|---|---|
| `d3.json(...)` | Load a JSON data file |
| `.then((data) => { ... })` | Run this code after the data loads successfully |
| `console.log(data)` | Print the data in the browser console |
| `.catch((error) => { ... })` | Show an error if the data does not load |

The data path is:

```text
data/women_doctors_geo.json
```

This means the file should be inside the `data/` folder.

---

## Part 7: Adding markers to the map

After the data loads, the code loops through each record:

```js
data.forEach((element) => {
  if (
    element.source_data?.birthplace?.lat &&
    element.source_data?.birthplace?.lon
  ) {
    L.marker([
      element.source_data.birthplace.lat,
      element.source_data.birthplace.lon,
    ])
      .addTo(map)
      .bindPopup("University of Edinburgh");
  }
});
```

This checks whether each record has:

- a birthplace latitude;
- a birthplace longitude.

If both exist, a marker is added to the map.

The popup currently says:

```js
.bindPopup("University of Edinburgh");
```

This is a basic placeholder.

During the workshop, this could be changed to show more useful information from the data.

For example:

```js
.bindPopup("Birthplace record");
```

or, if the relevant fields are available:

```js
.bindPopup(`
  <strong>Entry year:</strong> ${element.source_data?.entry_year ?? "Unknown"}
`);
```

---

## Part 8: Preparing the timeline data

The timeline uses the `entry_year` field.

First, the code extracts the years:

```js
const years = data
  .map((d) => Number(d.source_data?.entry_year))
  .filter((year) => Number.isFinite(year) && year > 0);
```

This does three things:

| Code | Meaning |
|---|---|
| `.map(...)` | Go through each record and extract the entry year |
| `Number(...)` | Convert the year into a number |
| `.filter(...)` | Keep only valid years |

Then the code groups the years and counts how many records appear in each year:

```js
const timelineData = d3
  .rollups(
    years,
    (values) => values.length,
    (year) => year,
  )
  .map(([year, count]) => ({ year, count }))
  .sort((a, b) => a.year - b.year);
```

The result is a new dataset that looks conceptually like this:

```text
year    count
----    -----
1870    2
1871    5
1872    4
```

This new grouped data is used to draw the timeline.

---

## Part 9: Creating the SVG chart area

The D3 chart is drawn inside this SVG element:

```html
<svg id="chart"></svg>
```

The JavaScript selects that SVG using D3:

```js
const svg = d3.select("#chart");
```

The chart dimensions are set with:

```js
const width = 900;
const height = 600;
const margin = { top: 30, right: 30, bottom: 60, left: 60 };
```

The margins create space around the chart for labels and axes.

The SVG is made responsive with:

```js
svg.attr("viewBox", `0 0 ${width} ${height}`);
```

The `viewBox` helps the SVG scale to the available space.

---

## Part 10: Creating scales and axes

D3 scales map data values to screen positions.

For the x-axis, the data values are years.

```js
const x = d3
  .scaleLinear()
  .domain(d3.extent(timelineData, (d) => d.year))
  .range([margin.left, width - margin.right]);
```

This says:

| Part | Meaning |
|---|---|
| `scaleLinear()` | Create a linear scale |
| `domain(...)` | The data range, from earliest year to latest year |
| `range(...)` | The screen range, from left side to right side |

For the y-axis, the data values are counts.

```js
const y = d3
  .scaleLinear()
  .domain([0, d3.max(timelineData, (d) => d.count)])
  .range([height - margin.bottom, margin.top]);
```

This maps record counts to vertical screen positions.

The x-axis is drawn with:

```js
svg
  .append("g")
  .attr("transform", `translate(0,${height - margin.bottom})`)
  .call(d3.axisBottom(x).tickFormat(d3.format("d")));
```

The y-axis is drawn with:

```js
svg
  .append("g")
  .attr("transform", `translate(${margin.left},0)`)
  .call(d3.axisLeft(y));
```

The axis labels are added with SVG text:

```js
svg
  .append("text")
  .attr("x", width / 2)
  .attr("y", height - 15)
  .attr("text-anchor", "middle")
  .text("Entry Year");
```

```js
svg
  .append("text")
  .attr("transform", "rotate(-90)")
  .attr("x", -height / 2)
  .attr("y", 20)
  .attr("text-anchor", "middle")
  .text("Number of Records");
```

---

## Part 11: Drawing the line chart

The line generator is created with:

```js
const line = d3
  .line()
  .x((d) => x(d.year))
  .y((d) => y(d.count));
```

This tells D3 how to convert each data point into a position on the chart.

The line is drawn as an SVG path:

```js
svg
  .append("path")
  .datum(timelineData)
  .attr("fill", "none")
  .attr("stroke", "steelblue")
  .attr("stroke-width", 2)
  .attr("d", line);
```

Important parts:

| Code | Meaning |
|---|---|
| `.append("path")` | Add a flexible SVG shape |
| `.datum(timelineData)` | Attach the timeline data |
| `fill: none` | Do not fill the inside of the path |
| `stroke: steelblue` | Set the line colour |
| `stroke-width: 2` | Set the line thickness |
| `.attr("d", line)` | Use the D3 line generator to draw the path |

---

## Part 12: Adding points to the chart

The starter file also adds circles to show each year/count point:

```js
svg
  .selectAll(".point")
  .data(timelineData)
  .join("circle")
  .attr("class", "point")
  .attr("cx", (d) => x(d.year))
  .attr("cy", (d) => y(d.count))
  .attr("r", 3)
  .attr("fill", "steelblue");
```

This creates one circle for each item in `timelineData`.

| Code | Meaning |
|---|---|
| `.selectAll(".point")` | Select all existing points |
| `.data(timelineData)` | Connect the data to the points |
| `.join("circle")` | Create circles for the data |
| `cx` | x-position of each circle |
| `cy` | y-position of each circle |
| `r` | radius of each circle |
| `fill` | circle colour |

---

## Part 13: Interactivity

The starter project currently includes simple map interactivity through popups:

```js
.bindPopup("University of Edinburgh");
```

When someone clicks a marker, the popup appears.

There is also a note in the code:

```js
// add slider for the timeline
```

This means the timeline slider is not currently implemented

Possible interactive features include:

- clicking a map marker to show information;
- hovering over a chart point to see the year and count;
- changing marker size or colour;
- filtering records by year;
- linking the map and timeline.

---
### Check the internet connection

The starter file loads D3 and Leaflet from online links.

If you are offline, these links may not load:

```html
<script src="https://d3js.org/d3.v7.min.js"></script>
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
```


## Key terms

| Term | Meaning |
|---|---|
| **HTML** | The structure of the webpage |
| **CSS** | The styling of the webpage |
| **JavaScript** | The programming language used to load data and create interaction |
| **D3.js** | A JavaScript library for data-driven visualisation |
| **Leaflet** | A JavaScript library for interactive maps |
| **JSON** | A structured data format |
| **GeoJSON** | A data format for geographic information |
| **SVG** | A drawing area for shapes, lines, circles, and text |
| **Scale** | A function that maps data values to screen positions |
| **Axis** | A visual guide showing values on a chart |
| **Marker** | A point placed on a map |
| **Popup** | A small information box that appears when a marker is clicked |
| **Console** | A browser tool for checking messages and errors |

---


## Next step

After building and editing the visualisation, continue to:

[Publishing with GitHub Pages](./Part_02_Publishing_With_GitHub_Pages.md)


[![License: CC BY-NC 4.0](https://licensebuttons.net/l/by-nc/4.0/80x15.png)](https://creativecommons.org/licenses/by-nc/4.0/)
