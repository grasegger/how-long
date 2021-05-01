<script>
  import Duration from "luxon/src/duration";
  import timestamps from "../timescales.yaml";
  let step = 0;
  $: time = eval(timestamps[step]["time"]);
  $: dur = Duration.fromObject({ seconds: time });

  $: hours = parseInt(dur.as("hours"));
  $: minutes = dur.as("minutes");
  $: days = dur
    .as("days")
    .toString()
    .replace(/\..*/, hours >= 24 ? ".5" : "");

  let result = false;

  function handleClick(event) {
    step = parseInt(event.srcElement.dataset["step"]);
  }

  function goBack() {
    step--;
    step = step < 0 ? 0 : step;
  }

  function toggleResult() {
    result = !result;
  }
</script>

<div class="main">
  <div>
    Choose the button that fits your task the most. If you think you found it
    you can toggle the results. Your current selection is highlighted in green.
  </div>
  <button disabled={step > 0 ? "" : "disabled"} on:click={goBack}
    >Go back</button
  >
  {#each timestamps as timestamp, i}
    <button
      on:click={handleClick}
      data-step={i}
      style="display: {i == step || i == step + 1 ? 'block' : 'none'}"
      data-current={i == step}
    >
      {timestamp["label"]}
    </button>
  {/each}

  <div>
    <button on:click={toggleResult}>Toggle Result</button>
    <div style="display: {result ? 'block' : 'none'}" on:click={goBack}>
      You need:<br />
      {minutes} Minutes or<br />
      {hours} Hours or<br />
      {days} Days <br />
    </div>
  </div>
</div>

<style>
  .main {
    display: grid;
    grid-gap: 3rem;
    min-height: 90vh;
    max-width: 500px;
    margin: 0 auto;
    padding: 3rem;
    grid-template-rows: repeat(5, 1fr);
  }

  button[data-current="true"] {
    background: green;
    color: white;
  }
</style>
