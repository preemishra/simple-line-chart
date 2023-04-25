<script>
  export let height;
  export let xScale;
  export let margin;

 export let xTicks
</script>
<g>
  {#each xTicks as tick}
    <text
      x={xScale(tick)+margin.left}
      y={height - margin.bottom}
      dy="6"
      dominant-baseline="hanging">{tick}% </text
    >
    
  {/each}
  <text></text>
</g>

