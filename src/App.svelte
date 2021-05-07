<script>
  import Chess from "./Chess.svelte";
  import OpenSelector from "./OpenSelector.svelte";
</script>

<main>
  <div class="container">
    <OpenSelector />
  </div>
</main>

<style>
  .container {
    justify-content: center;
  }

  main {
    height: 95vh;
    width: 95vw;
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
