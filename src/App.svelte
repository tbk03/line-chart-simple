<!-- code minimally adapted from Anup Joseph's Line Chart with Svelte and D3
https://dev.to/learners/line-chart-with-svelte-and-d3-3086 -->

<script>
import { onMount } from "svelte";
import { csv, extent, scaleLinear, scaleTime, line, curveNatural, timeFormat } from "d3";


// create data structure for storing the dataset
let dataset = [];
	
// convert data imported as text to correct type
const processRow = function(data){
	data.temperature = +data.temperature;
	data.timestamp = new Date(data.timestamp);

	return data;
}

// read data from tutorial github repository
onMount(async() => {
	dataset = await csv("https://gist.githubusercontent.com/curran/60b40877ef898f19aeb8/raw/9476be5bd15fb15a6d5c733dd79788fb679c9be9/week_temperature_sf.csv", 
						processRow)
						.then((d) => {return d;});
});

const url_string = "https://gist.githubusercontent.com/curran/60b40877ef898f19aeb8/raw/9476be5bd15fb15a6d5c733dd79788fb679c9be9/week_temperature_sf.csv";

// async function readData(url){
//     let dataset = await csv(url)
// 					.then(processRow);
// 	console.log(dataset);
// 	return dataset;
//   }

// readData(url_string);

// dataset = csv(url_string)
// 			.then(processRow);

// Create scales 
$: yScale = scaleLinear()
			.domain(extent(dataset, (d) => d.temperature))
			.range([0, 100]);

$: console.log(yScale);
console.log("at end of script");
$: console.log(dataset);

</script>

<main>
	<h1>Hello Chris!</h1>
	<p>{dataset}</p>
</main>

<style>
	main {
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
	}
</style>