<script lang="ts">
  import { onMount } from 'svelte';
  
  onMount(() => {
    console.log('Contact component mounted');
  });
</script>

<style>

  .container {
    display: flex;
    justify-content: space-between;
  }


  .contributer {
    width: 100%;
  }

  .brian {
    border-right: 1px solid;
  }

  .jon {
    border-left: 1px solid;
  }
</style>

<h1>Contact</h1>
<p>Get in touch with us!</p>

<div class="container">
  <div class="contributer brian">
    <h2>Brian Chang</h2>
  </div>

  <div class="contributer jon">
    <h2>Jon Cirillo</h2>
  </div>
</div>
