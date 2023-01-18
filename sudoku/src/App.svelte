<script lang="ts">
  import Plane from "./components/Plane.svelte";
  import * as sudoku from "sudoku";
  import reshape1Dto2D from "./utils/reshape";
  import FileManage from "./components/FileManage.svelte";
  import GameStatistics from "./components/GameStatistics.svelte";
  import saveToFile from "./utils/saveTofile";

  let shownNumbers;
  let correctNumbers;
  let hints;
  let inputNumbers;

  shownNumbers = sudoku.makepuzzle();
  correctNumbers = sudoku.solvepuzzle(shownNumbers);

  shownNumbers = shownNumbers.map((val) => (val != null ? val + 1 : null));
  shownNumbers = reshape1Dto2D(shownNumbers, [9, 9]);

  inputNumbers = new Array(81).fill(null);
  hints = new Array(81);
  let writeAsHint = false;

  const onChange = (e) => {
    inputNumbers = e.detail.input;
    hints = e.detail.hints;
  };
</script>

<div class="app-container">
  <div>
    {#key writeAsHint}
      <Plane
        {writeAsHint}
        input={inputNumbers}
        shown={shownNumbers}
        answers={correctNumbers}
        {hints}
        on:change={onChange}
      />
    {/key}

    <p>
      Instructions: Click an input to insert your answer. Use arrows to navigate
      around the plane and Backspace to delete answer. <span
        style="color:var(--error)">Tiles Marked as red are errors</span
      >
    </p>
  </div>
  <div>
    <button
      class={writeAsHint && "hint-clicked"}
      on:click={() => {
        writeAsHint = !writeAsHint;
      }}>Hint</button
    >
    <GameStatistics
      input={inputNumbers}
      answers={correctNumbers}
      preFilled={shownNumbers.flat().filter((val) => val != null).length}
    />
    <FileManage
      on:load={() => {
        saveToFile({});
      }}
      on:save={() => {
        saveToFile(JSON.stringify({ shownNumbers, inputNumbers, hints }));
      }}
    />
  </div>
</div>

<style>
  .app-container {
    display: flex;
    width: 100%;
    align-items: center;
    justify-content: space-around;
    height: 100vh;
  }
  p {
    text-align: center;
    font-size: 1.2rem;
    width: 50%;
    margin: auto;
  }
  button {
    width: 100%;
    padding: 1vw;
    margin: 1vw;
    background-color: var(--background-color);
    font-size: 2rem;
    color: var(--primary);
    border: 0.5vw solid var(--primary);
    border-radius: 10px;
    transition: all 0.2s;
  }
  .hint-clicked {
    background-color: var(--primary);
    color: var(--background-color);
    transition: all 0.2s;
  }
</style>
