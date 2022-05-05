<!-- 

Line chart code adapted from Anup Joseph's Line Chart with Svelte and D3
https://dev.to/learners/line-chart-with-svelte-and-d3-3086 

Code structured using Amelia Wattenberger's design pattern for prototyping in D3
https://observablehq.com/@wattenberger/prototyping-in-d3

-->

<script>
// ****************************************************************************************
// 0. Setup - imports and variable declarations
// ****************************************************************************************

import { onMount } from "svelte";
import { csv, extent, scaleLinear, scaleTime, line, curveNatural, timeFormat } from "d3";


// create data structure for storing the dataset
let dataset = [];

let scaleX;
let scaleY;

let lineGenerator;
let lineObject;

// ****************************************************************************************
// 1. Access and parse data
// ****************************************************************************************

// function convert data imported as text to correct type
const processRow = function(data){
	data.temperature = +data.temperature;
	data.timestamp = new Date(data.timestamp);

	return data;
}

// read data from tutorial github repository
const url_string = "https://gist.githubusercontent.com/curran/60b40877ef898f19aeb8/raw/9476be5bd15fb15a6d5c733dd79788fb679c9be9/week_temperature_sf.csv";

onMount(async() => {
	dataset = await csv(url_string, processRow)
						.then((d) => {return d;});
});

// ****************************************************************************************
// 2. Create accessor functions
// ****************************************************************************************

let accessX = (d) => d.timestamp;
let accessY = (d) => d.temperature;

// ****************************************************************************************
// 3. Define chart structure
// ****************************************************************************************

let dms = {
	width: 900,
	height: 600,
	marginTop: 15,
	marginBottom: 50,
	marginLeft: 50,
	marginRight: 20
};

dms.boundedWidth = dms.width - dms.marginLeft - dms.marginRight;
dms.boundedHeight = dms.height - dms.marginTop - dms.marginBottom;

// ****************************************************************************************
// 4. Create scales
// ****************************************************************************************

// need to be reactive as there is a wait for data to load from github
$: {
	scaleX = scaleTime()
			.domain(extent(dataset, accessX))
			.range([0, dms.boundedWidth]);

	scaleY = scaleLinear()
			.domain(extent(dataset, accessY))
			.range([dms.boundedHeight, 0]);
}

// ****************************************************************************************
// 5. Create elements of the chart
// ****************************************************************************************

// need to be reactive as there is a wait for data to load from github
$: {
	// create function for generating lines
	lineGenerator = line()
						.curve(curveNatural)
						.x(d => scaleX(accessX(d)))
						.y(d => scaleY(accessY(d)));

	// create the line object for plotting
	lineObject = lineGenerator(dataset);
}

</script>

<main>
	<svg width = {dms.width} height = {dms.height}>
		<g transform={`translate(${dms.marginLeft}, ${dms.marginRight})`}>
			
			<!-- add data points -->
			{#each dataset as d, i}
				<circle
					cx = {scaleX(accessX(d))}
					cy = {scaleY(accessY(d))}
					r = "5"
				/>
			{/each}

			<!-- add line  -->
			<path class="line" d={lineObject}/>

		</g>

	</svg>
</main>

<style>
	.line{
		fill: transparent;
		stroke: rgb(0, 0, 0);
	}

	/* main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	} */
</style>