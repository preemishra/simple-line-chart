<script>
  import dataCopy from "./data/dataCopy";
  import LineChart from "./components/LineChart.svelte";
  import data from "./data/data.js";
  import graphConfiguration from "./config/graph";
 

</script>

<LineChart {data} {graphConfiguration}/>
<LineChart data={dataCopy} {graphConfiguration}/>

