<script>
  import * as d3 from "d3";
  import { scaleLinear } from "d3-scale";
  import { max } from "d3-array";
  export let data;
  export let graphConfiguration
  import Tooltip from "./shared/Tooltip.svelte";
  import Yaxis from "./shared/Yaxis.svelte";
  import Xaxis from "./shared/Xaxis.svelte";
  let width = graphConfiguration.dimensions.width;
  let height = graphConfiguration.dimensions.height;
  let margin = graphConfiguration.margin
  let xScale = scaleLinear()
    .domain([0, 100])
    .range([0, width - margin.left - margin.right]);
    let xTicks = xScale.ticks(10);
  let yScale = scaleLinear()
    .domain([0, max(data, (d) => d.hours)])
    .range([height - margin.top - margin.bottom, 0]);
    let yTicks = yScale.ticks(5);
  let path = d3
    .line()
    .x((d) => xScale(d.grade))
    .y((d) => yScale(d.hours))
    .curve(d3.curveNatural);

  let hoveredData;
  $: console.log(hoveredData);
  let xPath = `M${margin.left + 0.5},6V0H${width - margin.right + 1}V6`;
  let yPath = `M-6,${height + margin.bottom -240}H0.5V0.5H-6`;
</script>

<div
  class="container"
  on:mouseleave={() => {
    hoveredData = null;
  }}
>
  <h1>Student study hours VS Scored grade</h1>
  <svg {width} {height}>
    <Xaxis {height} {xScale} {margin} {xPath} {xTicks}/>
    <Yaxis {height} {width} {yScale} {margin} {yPath} {yTicks}/>

    <g class="circles" transform="translate({margin.left} {margin.top})">
      <text
        x="250"
        y="75"
        fill="brown"
        font-size="2em"
        transform="rotate(90 50,-20)">Study Hours</text
      >
      <text x="650" y="700" fill="brown" font-size="2em">Scored Grade</text>
      <path
        d={path(data.sort((a, b) => a.grade - b.grade))}
        fill="none"
        stroke="green"
        stroke-width="5"
      />
      {#each data.sort((a, b) => a.grade - b.grade) as student}
        <circle
          cx={xScale(student.grade)}
          cy={yScale(student.hours)}
          r={hoveredData && hoveredData == student ? "20" : "10"}
          opacity={hoveredData ? (hoveredData == student ? "1" : ".3") : "1"}
          fill="brown"
          stroke="yellow"
          on:mouseover={() => {
            hoveredData = student;
          }}
          on:focus={() => {
            hoveredData = student;
          }}
          tabIndex="0"
        />
      {/each}
    </g>
  </svg>
  {#if hoveredData}
    <Tooltip data={hoveredData} {xScale} {yScale} />
  {/if}
</div>

<style>
  circle {
    transition: r 300ms ease, opacity 300ms ease;
    cursor: pointer;
  }

  circle:focus {
    outline: none;
  }
  .container {
    text-align: center;
  }
  h1 {
    color: brown;
    text-align: center;
  }
  svg{
    border: 1px solid black;
  }
</style>
