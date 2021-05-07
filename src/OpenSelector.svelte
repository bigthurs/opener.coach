<script>
  import Chess from "./Chess.svelte";
  import OpeningLines from "./utils/OpeningLines";

  let selectedColor;
  let selectedLine;
  let selectionsComplete = false;

  function selectColor(color) {
    selectedColor = color;
  }

  function selectLine(line) {
    selectedLine = line;
    selectionsComplete = true;
  }
</script>

<main>
  {#if selectedColor === undefined && selectionsComplete === false}
    Select color
    <div class="color-buttons">
      <button on:click|preventDefault={() => selectColor("white")}>
        White
      </button>
      <button on:click|preventDefault={() => selectColor("black")}>
        Black
      </button>
    </div>
  {:else if selectedColor !== undefined && selectionsComplete === false}
    Select line
    <div class="line-buttons">
      {#each OpeningLines as openingLine (openingLine)}
        {#if openingLine.player === selectedColor}
          <button on:click={() => selectLine(openingLine)}
            >{openingLine.name}</button
          >
        {/if}
      {/each}
    </div>
  {:else if selectionsComplete === true}
    <Chess {selectedLine} />
  {/if}
</main>

<style>
  button {
    margin-top: 1rem;
  }

  .color-buttons {
    display: flex;
    flex-direction: column;
    justify-content: center;
    width: 20vw;
    height: 20vh;
  }

  .line-buttons {
    display: flex;
    flex-direction: column;
    justify-content: center;
    width: 20vw;
    height: 20vh;
  }
</style>
