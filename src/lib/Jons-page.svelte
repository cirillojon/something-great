<script lang="ts">
  import { onMount } from 'svelte';

  onMount(() => {
    console.log('Jon component mounted');
  });
</script>

<div class="page-container">
  <h1>Jon's Page</h1>
  <p>This is the crazy page.</p>
  <div class="link-container">
    <a href="https://github.com/cirillojon" target="_blank" rel="noopener"></a>
    <a href="https://cirillojon.herokuapp.com/" target="_blank" rel="noopener"></a>
  </div>
  <a href="https://react-app-sandbox.netlify.app/" target="_blank" rel="noopener" class="card">
    <h2>React Site with Games</h2>
    <p>Check it out!</p>
  </a>
</div>
