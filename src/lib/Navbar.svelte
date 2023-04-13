<nav>
    <ul>
      <li>
        <a href="#home" on:click={handleClick} class:selected={activeLink === 'home'}>
          Home
        </a>
      </li>
      <li>
        <a href="#about" on:click={handleClick} class:selected={activeLink === 'about'}>
          About
        </a>
      </li>
      <li>
        <a href="#contact" on:click={handleClick} class:selected={activeLink === 'contact'}>
          Contact
        </a>
      </li>
    </ul>
  </nav>
  
  <script lang="ts">
    let activeLink: string = 'home';
  
    function handleClick(event: MouseEvent) {
      const target = event.target as HTMLAnchorElement;
      activeLink = target.textContent.toLowerCase();
    }
  </script>
  
  <style>
    /* Style the navbar */
    nav {
      background-color: #333;
      color: white;
      display: flex;
      justify-content: center;
      padding: 10px;
    }
  
    ul {
      list-style: none;
      margin: 0;
      padding: 0;
    }
  
    li {
      display: inline-block;
      margin-right: 20px;
    }
  
    a {
      color: white;
      text-decoration: none;
    }
  
    /* Highlight the selected link */
    .selected {
      font-weight: bold;
    }
  </style>
  