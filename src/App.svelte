<script lang="ts">
  import { Router, Route, Link, link } from 'svelte-navigator';
  import Home from './Home.svelte';
  import About from './About.svelte';
  import Contact from './Contact.svelte';
  import JonsPage from './Jons-page.svelte';


  let currentPath = '/';
  const handleNavigation = (path) => {
    currentPath = path;
  };
</script>

<style>
  nav {
    display: flex;
    justify-content: space-around;
    margin-bottom: 2rem;
  }

  a {
    text-decoration: none;
    color: inherit;
  }

  a.active {
    font-weight: bold;
  }
</style>

<Router>
  <nav>
    <a href="/" use:link class:active={currentPath === '/'} on:click={() => handleNavigation('/')}>Home</a>
    <a href="/about" use:link class:active={currentPath === '/about'} on:click={() => handleNavigation('/about')}>About</a>
    <a href="/contact" use:link class:active={currentPath === '/contact'} on:click={() => handleNavigation('/contact')}>Contact</a>
    <a href="/jons-page" use:link class:active={currentPath === '/jons-page'} on:click={() => handleNavigation('/jons-page')}>Jon's Page</a>
  </nav>

  <Route path="/" component="{Home}" />
  <Route path="/about" component="{About}" />
  <Route path="/contact" component="{Contact}" />
  <Route path="/jons-page" component="{JonsPage}" />
</Router>
