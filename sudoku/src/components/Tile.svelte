<script lang="ts">
  import { createEventDispatcher } from "svelte";

  export let value;
  export let expectedValue;
  const dispatch = createEventDispatcher();
  const handledKeys = {
    ArrowRight: 1,
    ArrowLeft: -1,
    ArrowDown: 9,
    ArrowUp: -9,
  };
</script>

<input
  class={"tile " + (value == expectedValue || value == "" ? "" : "error")}
  bind:value
  type="text"
  min="1"
  max="9"
  maxlength={1}
  on:keydown={(e) => {
    e.preventDefault();
    if (!isNaN(e.key) && e.key != "0") {
      value = e.key;
    } else if (e.key == "Backspace") {
      value = "";
    } else if (handledKeys[e.code] != undefined) {
      console.log("Arrows");
      dispatch("arrow", { focusShift: handledKeys[e.code] });
    }
  }}
/>

<style>
  input {
    background: transparent;
    border: 0.4vw solid var(--primary);
    margin: 0.5vw;
    width: 5vw;
    height: 5vw;
    max-width: 75px;
    max-height: 75px;
    border-radius: 5px;
    color: var(--primary);
    font-size: 2rem;
    text-align: center;
    caret-color: transparent;
    transition: all 0.3s;
  }
  input:focus {
    border: 0.4vw solid var(--primary-focus);
    transition: all 0.3s;
    color: var(--primary-focus);
  }
  input.error {
    border: 0.4vw solid var(--error);
    transition: all 0.3s;
    color: var(--error);
  }

  .tile:nth-child(3n) {
    margin-right: 0.8vw !important;
  }
</style>
