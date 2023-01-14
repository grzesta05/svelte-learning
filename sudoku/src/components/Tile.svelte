<script lang="ts">
  export let value;
</script>

<input
  bind:value
  type="text"
  min="1"
  max="9"
  maxlength={1}
  on:keyup={(e) => {
    e.preventDefault();
    if (!isNaN(e.target.value)) {
      value = e.target.value;
    } else {
      value = "";
    }
    console.log();
  }}
/>

<style>
  input {
    background: transparent;
    border: 0.4vw solid blueviolet;
    margin: 0.5vw !important;
    width: 5vw;
    height: 5vw;
    max-width: 75px;
    max-height: 75px;
    border-radius: 5px;
    color: blueviolet;
    font-size: 2rem;
    text-align: center;
  }
</style>
