<!-- 

Axis code adapted from Anup Joseph's Line Chart with Svelte and D3
https://dev.to/learners/line-chart-with-svelte-and-d3-3086 

The axis code in Anup's article was based on code from Tom Fevrier
https://github.com/TomFevrier/svelte-d3-demo/blob/master/src/Axis.svelte

-->
<script>

    // libraries
    import { select, selectAll } from "d3-selection";
    import { axisBottom, axisLeft } from "d3-axis";
    import { timeFormat } from "d3";

    // no svelte components imported

    // props - accessible from svelte compoments
    export let boundedHeight;
    export let margin;
    export let axisPosition;    // either 'left' or 'bottom'
    export let scale;

    // component variables
    let transform;              // to positon axis
    let axisContainer;          // container for html elements that make up D3 axis
                                // which is bound to <g> element below
                                
    let axisGen;                // function which will generate the axis

    // create the axis
    // need to be reactive as there is a wait for data to load from github
    $: {

        //select(axisContainer).html(null); 

        // create axis generator and define axis format
        axisGen = axisBottom(scale)
                .tickSizeOuter(0)
                .tickFormat((d) => timeFormat("%a")(d));

        // create axis within the container
        select(axisContainer)
            .call(axisGen);
            //.attr("class", "xAxisGroup")
            //.attr("stroke","#E04836")

        // style the axis
        // difficult to do this in css as axis is yet to be created in DOM
        select(axisContainer)           // axis baseline
            .selectAll("path")
            .attr("stroke", "#808080");

        select(axisContainer)           // axis ticks
            .selectAll(".tick line")
            .attr("stroke", "#CECECE");

        select(axisContainer)           // axis text
            .attr("font-size", 20)
            .attr("font-family", "Lato");
    }


</script>

<g bind:this={axisContainer}/>


<style>
    @import url('https://fonts.googleapis.com/css2?family=Lato&display=swap');
</style>