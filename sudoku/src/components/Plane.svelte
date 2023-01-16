<script lang="ts">
  import { onMount } from "svelte";
  import Tile from "./Tile.svelte";
  import * as sudoku from "sudoku";
  import reshape1Dto2D from "../utils/reshape";

  export let array;
  export let answers;
  answers = answers.map((val) => val + 1);
  const onArrow = (e) => {
    let focusShift = e.detail.focusShift;
    const inputs = document.querySelectorAll("input.tile");
    let focusedIndex = e.detail.i * 9 + e.detail.o;

    if (focusedIndex + focusShift < 81 && focusedIndex + focusShift >= 0) {
      if (!inputs.item(focusedIndex + focusShift).disabled) {
        inputs.item(focusedIndex + focusShift).focus();
      } else {
        while (
          focusedIndex + focusShift < 81 &&
          focusedIndex + focusShift >= 0
        ) {
          focusShift += focusShift > 0 ? 1 : -1;
          if (!inputs.item(focusedIndex + focusShift).disabled) {
            inputs.item(focusedIndex + focusShift).focus();
            return;
          }
        }
      }
    }
  };

  const onAnswer = (e) => {
    //winning condition here
  };
</script>

<div class="plane--container">
  {#each reshape1Dto2D(answers, [9, 9]) as row, i}
    <div class="row">
      {#each row as tile, o}
        <Tile
          position={{ i: i, o: o }}
          expectedValue={tile}
          on:answer={onAnswer}
          on:arrow={onArrow}
          value={array[i][o]}
        />
      {/each}
    </div>
  {/each}
</div>

<style>
  .plane--container {
    width: 10%;
  }
  .row:nth-child(3n) {
    margin-bottom: 0.8vw !important;
  }
  .row {
    display: flex;
    flex-wrap: nowrap;
  }
</style>
