<script lang="ts">
  import { createEventDispatcher } from "svelte";

  export let value;
  const dispatch = createEventDispatcher();
  const handledKeys = {
    ArrowRight: 1,
    ArrowLeft: -1,
    ArrowDown: 9,
    ArrowUp: -9,
  };
</script>

<input
  class="tile"
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
    border: 0.4vw solid blueviolet;
    margin: 0.5vw !important;
    width: 5vw;
    height: 5vw;
    max-width: 75px;
    max-height: 75px;
    border-radius: 5px;
    color: blueviolet;
    font-size: 2rem;
    text-align: center;
    caret-color: transparent;
    transition: all 0.3s;
  }
  input:focus {
    border: 0.4vw solid blue;
    transition: all 0.3s;
    color: blue;
  }
</style>
