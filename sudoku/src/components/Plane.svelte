<script lang="ts">
  import { onMount } from "svelte";
  import Tile from "./Tile.svelte";
  const rowID = "row";
  const array = new Array(9).fill(new Array(9).fill(""));

  const onArrow = (e) => {
    const focusShift = e.detail.focusShift;
    const inputs = document.querySelectorAll("input.tile");
    let focusedIndex;
    inputs.forEach((e, k) => {
      if (e == document.activeElement) {
        focusedIndex = k;
        return;
      }
    });
    console.log(focusedIndex);
    if (focusedIndex + focusShift < 81 && focusedIndex + focusShift >= 0) {
      inputs.item(focusedIndex + focusShift).focus();
    }
  };
</script>

<div>
  {#each array as row}
    <div class="row">
      {#each row as tile}
        <Tile expectedValue={1} on:arrow={onArrow} value={tile} />
      {/each}
    </div>
  {/each}
</div>

<style>
  .row {
    margin: 0%;
  }
  .row:nth-child(3n) {
    margin-bottom: 0.8vw !important;
  }
</style>
