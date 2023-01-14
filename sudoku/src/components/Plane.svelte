<script lang="ts">
  import Tile from "./Tile.svelte";
</script>

{#each [0, 1, 2, 3, 4, 5, 6, 7, 8] as i}
  {#each [0, 1, 2, 3, 4, 5, 6, 7, 8] as o}
    <Tile value={""} />
  {/each}
  <br />
{/each}
