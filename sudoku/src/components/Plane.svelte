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

{#each array as row}
  {#each row as tile}
    <Tile on:arrow={onArrow} value={tile} />
  {/each}
  <br />
{/each}
