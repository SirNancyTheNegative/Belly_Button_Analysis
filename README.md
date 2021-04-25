# Belly_Button_Analysis

## Overview of Project

### Purpose

This project demonstrates the use of JavaScript and HTML through Plotly to create a dynamic website dedicated to analysis of microscopic specimens living within people's navels. Using a series of datasets separated by ID, numerous charts were made in order to help visualize the data and supporting factoids. Or rather, the bases of those charts were made, and through the use of a simple trigger, the charts were set to update in order to dynamically form information for any of the datasets provided. The process was simple: Get the data, format it correctly in the charts, and draw the charts.

## Process

### Getting The Data

In order to make a chart with data, you need to be able to get the data in the first place. The datasets in question were held in one singular place: a JSON file that contained a multi-layered object with the ID of each dataset being its key, in a sense. The first step was to get to it in the first place.

Using d3, this was a simple matter, as d3.json() was a function that let us read the JSON file right away, and adding `.then()` would let us use that data however we'd need to. Often it's to pull important data from areas of particular interest, and in this case, we need both the metadata and the samples. Accessing them is simply a matter of writing:

```
d3.json("samples.json").then((data) => {
  var samples = data.samples;
  ...
  var metadata = data.metadata;
  ...
}
```

After that, we can use that data within the brackets however we need.

### Forming the Charts

There were three charts we'd decided to use in order to best demonstrate the data: A horizontal bar graph of the ten most prominent specimens for any given dataset, a bubble chart of *all* the specimens of a given dataset, and a gauge chart that demonstrates the washing frequency of the person whose navel makes up the given dataset.

The creation of the formermost was the most taxing: Because the IDs are in numeric form, they need to be changed to another form in order to not be graphed numerically. Because JavaScript otherwise recognizes everything as a string, fixing this was simply a matter of adding a string identifier to the beginning (e.g. changing `940` to `OTU 940`) and making that a new variable. In the case of this data, since we need an array of them, we can do this with an inline function to the entire array with `.map()`.

We also only need the ten most prominent sample values, so we can use ".slice(0,10)" to obtain those ten largest, and save that as its own variable if we needed to use it for more than just the one graph. Since we don't, the x values for this graph will be `sample_values.slice(0,10).reverse()`. The `.reverse()` is important, because otherwise, when we post our graph, it won't curve in a descending fashion. We want the most prominent sample at the top.

In order to make the graph, we make a data array with a single object containing the data we've taken so far (reversed and limited to 10 entries to be consistent), and a layout object with an appropriate title and ticks for the y axis, before we plot it using the `'bar'` tag we've allocated in the HTML.

The bubble chart was much the same process, instead using the entire sample values array as data points, though in the data we include a couple other tags -- particularly:

```
marker: {
  size: sample_values,
  color: otu_ids
}
```

This will make it so each id has a different color, and will make it so each bubble is bigger the larger the value of the sample.

Last but not least was the gauge chart. Unlike the bar graph and the bubble chart, the gauge only had one item to worry about: The `wfreq` part of the metadata, as that was its value. It was more a matter of customization in order to make it look appealing to the eye than it was any kind of discrepancy with data.

### Finishing the Site

Now that all the code is done, it's time to work on the site. Normally, it doesn't look too great -- the background is white, the jumbotron is grey, and any color on the page comes from the various little decisions on the graphs. It could be better.

The first thing I decided to change was the font. Sans serif fonts are alright, but I would much rather have some kind of serif font than something with nothing on it. So I added the following tag to the body of the page:
```
"font-family:Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;"
```
That way, there would be a number of serifed fonts that could appear, with Times New Roman being somewhat of a catch-all, just in case none of the other fonts were installed on a particular device.

The second change I decided to take care of was the color. White often causes eye strain, particularly on account of the blue light coming from it. To help remedy this, I changed the color of the background to a pleasant, creamy "mocassin" color, and went so far as to change the background on the Jumbotron and even the graphs to a somewhat darker shade. I could have gone with a darker color, but I felt this was an alright choice.

The last major change I made was to reorient the charts. While having the bar chart and the gauge on top seemed like an okay idea, I felt it better to have the more visually-appealing bubble chart above the two other charts instead, and have them share a row underneath, where the bubble chart used to be.
