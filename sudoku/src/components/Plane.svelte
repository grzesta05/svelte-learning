<script lang="ts">
  import { onMount } from "svelte";
  import Tile from "./Tile.svelte";
  const rowID = "row";
  const array = new Array(9).fill(new Array(9).fill(""));
  const correctArray = new Array(9).fill(new Array(9).fill(false));

  const onArrow = (e) => {
    const focusShift = e.detail.focusShift;
    const inputs = document.querySelectorAll("input.tile");
    let focusedIndex = e.detail.i * 9 + e.detail.o;

    if (focusedIndex + focusShift < 81 && focusedIndex + focusShift >= 0) {
      inputs.item(focusedIndex + focusShift).focus();
    }
  };

  const onAnswer = (e) => {
    correctArray[e.detail.i][e.detail.o] = e.detail.correct;
  };
</script>

<div>
  {#each array as row, i}
    <div class="row">
      {#each row as tile, o}
        <Tile
          position={{ i: i, o: o }}
          expectedValue={1}
          on:answer={onAnswer}
          on:arrow={onArrow}
          value={tile}
        />
      {/each}
    </div>
  {/each}
</div>

<style>
  .row:nth-child(3n) {
    margin-bottom: 0.8vw !important;
  }
</style>
