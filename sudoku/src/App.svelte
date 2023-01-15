<script lang="ts">
  import Plane from "./components/Plane.svelte";
  import * as sudoku from "sudoku";
  import reshape1Dto2D from "./utils/reshape";

  let shownNumbers = sudoku.makepuzzle();
  const correctNumbers = sudoku.solvepuzzle(shownNumbers);
  shownNumbers = shownNumbers.map((val) => (val != null ? val + 1 : null));
  shownNumbers = reshape1Dto2D(shownNumbers, [9, 9]);

  let inputNumbers = new Array(81);
</script>

<div class="app-container">
  <div>
    <Plane array={shownNumbers} answers={correctNumbers} />
    <p>
      Instructions: Click an input to insert your answer. Use arrows to navigate
      around the plane and Backspace to delete answer. <span
        style="color:var(--error)">Tiles Marked as red are errors</span
      >
    </p>
  </div>
</div>

<style>
  .app-container {
    display: flex;
    width: 100%;
    align-items: center;
    justify-content: center;
    height: 100vh;
  }
  p {
    text-align: center;
    font-size: 1.2rem;
    width: 50%;
    margin: auto;
  }
</style>
