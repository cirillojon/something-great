<script lang="ts">
  import { onMount, onDestroy } from 'svelte';
  import { writable } from 'svelte/store';
  import * as THREE from 'three-full';

  const balance = writable<number>(100);
  const betAmount = writable<number>(0);
  const result = writable<string>('');
  const purchasedItems = writable<{ name: string, object: THREE.Object3D }[]>([]);

  // create a Three.js scene and camera
  let scene: THREE.Scene;
  let camera: THREE.PerspectiveCamera;
  let renderer: THREE.WebGLRenderer;
  let item: THREE.Object3D;

  onMount(() => {
    scene = new THREE.Scene();
    camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
    camera.position.z = 5;
    renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setSize(300, 300);
    renderer.setClearColor(0xffffff, 1); 
    document.body.appendChild(renderer.domElement);
    
    // create a new Three.js object to represent the purchased item
    const geometry = new THREE.BoxGeometry(2,2,2);
    const material = new THREE.MeshBasicMaterial({ color: Math.random() * 0xffffff }); // random color
    item = new THREE.Mesh(geometry, material);

    // position the item based on how many have been purchased already
    const itemPosition = $purchasedItems.length > 0 ? 3 * $purchasedItems.length : 0;
    item.position.set(itemPosition, 0, 0);

    item.visible = false; // hide the purchased item
    scene.add(item);
  });

  function tossCoin() {
    const guessInput: HTMLInputElement = document.getElementById('guess-input') as HTMLInputElement;
    const guess: string = guessInput.value.toLowerCase();
    if (guess !== 'heads' && guess !== 'tails') {
      result.set('Please enter "heads" or "tails"');
    } else if ($betAmount === 0) {
      result.set('Please place a bet first');
    } else {
      const coin = Math.random() < 0.5 ? 'heads' : 'tails';
      if (guess === coin) {
        balance.update(current => current + $betAmount);
        result.set(`Congratulations, you guessed correctly! You won ${$betAmount}!`);
      } else {
        balance.update(current => current - $betAmount);
        result.set(`Sorry, the coin landed on ${coin}. Better luck next time! You lost ${$betAmount} :(`);
      }
      betAmount.set(0);
    }
  }

  function setBetAmount(amount: number) {
    betAmount.set(amount);
  }

  function purchaseItem() {
    if ($balance >= 100) {
      balance.update(current => current - 100);
      result.set('Congratulations, you have purchased the item!');
      
      // position the purchased item based on how many have been purchased already
      const itemPosition = $purchasedItems.length > 0 ? 3 * $purchasedItems.length : 0;
      item.position.set(itemPosition, 0, 0);
      
      item.visible = true; // show the purchased item

      purchasedItems.update(items => [...items, { name: 'Item', object: item }]);
} else {
result.set('Sorry, you do not have enough money to purchase this item.');
}
}

// animate the Three.js scene
function animate() {
requestAnimationFrame(animate);
renderer.render(scene, camera);
}

onMount(() => {
console.log('Coin Toss game mounted');
animate();
});

onDestroy(() => {
// remove the Three.js renderer and all objects from the DOM
document.body.removeChild(renderer.domElement);
scene.remove(item);
item.geometry.dispose();
item.material.dispose();
});

</script>
<div class="page-container" style="background-color: white;">
  <h1>Coin Toss Game</h1>
  <p>Balance: $ {$balance}</p>
  <label for ="guess-input">Guess heads or tails:</label>
  <input type="text" id="guess-input" name="guess-input">
  <br>
  <div class="bet-buttons">
    <button on:click={() => setBetAmount(10)}>Bet $10</button>
    <button on:click={() => setBetAmount(50)}>Bet $50</button>
    <button on:click={() => setBetAmount($balance)}>All in</button>
  </div>
  <p>Current bet: $ {$betAmount}</p>
  <button on:click={tossCoin}>Toss Coin</button>
  <p>{$result}</p>
  <button on:click={purchaseItem} style={$balance >= 100 ? '' : 'display: none;'}>Purchase Item for $100</button>
  <div>
    <h2>Purchased items:</h2>
    <ul>
      {#each $purchasedItems as {name, object}, i}
      {#if object.visible}
      <li>
        <div id="item-container"></div>
        {name} ({i+1})
      </li>
      {/if}
      {/each}
    </ul>
  </div>
</div>  