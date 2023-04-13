<script lang="ts">
    import { onMount } from 'svelte';
    import github from './assets/github.png'
    import galaxy from './assets/galaxy.png'
  
    onMount(() => {
      console.log('Jon component mounted');
    });
  </script>
  
  <style>
    .page-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      background: linear-gradient(135deg, #1a237e, #7986cb);
      background-size: 200% 200%;
      animation: gradient 10s ease infinite;
    }
  
    h1 {
      color: white;
      font-size: 3rem;
      margin-bottom: 1rem;
    }
  
    p {
      color: white;
      font-size: 1.2rem;
      margin-bottom: 2rem;
    }
  
    .link-container {
      display: flex;
      align-items: center;
      gap: 1rem;
      margin-bottom: 2rem;
    }
  
    .card {
      width: 300px;
      height: 200px;
      background-color: white;
      border-radius: 8px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      text-decoration: none;
    }
  
    .card h2 {
      font-size: 1.5rem;
      margin-bottom: 1rem;
    }
  
    @keyframes gradient {
      0% {
        background-position: 0% 50%;
      }
      50% {
        background-position: 100% 50%;
      }
      100% {
        background-position: 0% 50%;
      }
    }
  </style>
  
  <div class="page-container">
    <h1>Jon's Page</h1>
    <p>This is the crazy page.</p>
    <div class="link-container">
      <a href="https://github.com/cirillojon" target="_blank" rel="noopener">
        <img src={github} alt="GitHub" width="24" height="24">
      </a>
      <a href="https://cirillojon.herokuapp.com/" target="_blank" rel="noopener">
        <img src={galaxy} alt="Portfolio" width="24" height="24">
      </a>
    </div>
    <a href="https://react-app-sandbox.netlify.app/" target="_blank" rel="noopener" class="card">
      <h2>React Site with Games</h2>
      <p>Check it out!</p>
    </a>
  </div>
  