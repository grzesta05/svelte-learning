<script lang="ts">
  import { createEventDispatcher, onMount } from "svelte";
  type Position = { i: number; o: number };

  export let input;
  export let hints: Array<number> = [];
  export let expectedValue;
  export let position: Position;
  export let writeAsHint;
  let disabled = false;

  let value = input.value;
  onMount(() => {
    if (input.isInitial) {
      disabled = true;
    }
  });
  const dispatch = createEventDispatcher();
  const handledKeys = {
    ArrowRight: 1,
    ArrowLeft: -1,
    ArrowDown: 9,
    ArrowUp: -9,
  };
</script>

<div style="position: relative;">
  {#key hints}
    <div class="hints">{hints}</div>
  {/key}
  <input
    {disabled}
    class={"tile " + (value == expectedValue || value == null ? "" : "error")}
    bind:value
    type="text"
    min="1"
    max="9"
    maxlength={1}
    on:keydown={(e) => {
      e.preventDefault();
      //Check if a number
      if (!isNaN(e.key) && e.key != "0") {
        if (writeAsHint) {
          if (hints.find((val) => e.key == val.toString()) == undefined) {
            hints = [...hints, e.key];
          }
        } else {
          value = e.key;
        }
        dispatch("change", { val: value, hints: hints, ...position });
        //Check if backspace
      } else if (e.key == "Backspace") {
        if (writeAsHint) {
          hints = [...hints.splice(0, hints.length - 1)];
        } else {
          value = "";
        }
        dispatch("change", { val: value, hints: hints, ...position });
        //Check if arrows
      } else if (handledKeys[e.code] != undefined) {
        console.log("Arrows");
        dispatch("arrow", { focusShift: handledKeys[e.code], ...position });
      }
    }}
  />
</div>

<style>
  .hints {
    position: absolute;
    text-align: center;
    width: 100%;
    top: 1vw;
    margin: auto;
    line-break: auto;
  }
  input {
    background: transparent;
    border: 0.4vw solid var(--primary);
    margin: 0.5vw;
    width: 6vw;
    height: 6vw;
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
  .tile:disabled {
    background-color: var(--primary);
    color: var(--background-color);
  }
</style>
