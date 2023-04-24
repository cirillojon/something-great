<script lang="ts">
  import { onMount, onDestroy } from 'svelte';
  import { writable } from 'svelte/store';
  import * as THREE from 'three-full';

  const balance = writable<number>(100);
  const betAmount = writable<number>(0);
  const result = writable<string>('');
  const purchasedItems = writable<{ name: string, object: THREE.Object3D, renderer: THREE.WebGLRenderer }[]>([]);

  let scene: THREE.Scene;
  let camera: THREE.PerspectiveCamera;
  let renderer: THREE.WebGLRenderer;
  let itemContainer: HTMLElement;

  function createItem(): { item: THREE.Object3D, renderer: THREE.WebGLRenderer } {
  const geometry = new THREE.BoxGeometry(2, 2, 2);
  const material = new THREE.MeshBasicMaterial({ color: Math.random() * 0xffffff });
  const item = new THREE.Mesh(geometry, material);
  item.visible = false;

  const newRenderer = new THREE.WebGLRenderer({ antialias: true });
  newRenderer.setSize(300, 300);
  newRenderer.setClearColor(0xffffff, 1);

  return { item, renderer: newRenderer };
}


  onMount(() => {
  scene = new THREE.Scene();
  camera = new THREE.PerspectiveCamera(75, 1, 0.1, 1000);
  camera.position.z = 10;
  renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(300, 300);
  renderer.setClearColor(0xffffff, 1);

  animate();
  itemContainer.appendChild(renderer.domElement);
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

    const { item, renderer: itemRenderer } = createItem();
    const itemPosition = $purchasedItems.length > 0 ? 3 * $purchasedItems.length : 0;
    item.position.set(itemPosition, 0, 0);
    
    item.visible = true;
    scene.add(item);

    purchasedItems.update(items => [...items, { name: 'Cube', object: item, renderer: itemRenderer }]);
  } else {
    result.set('Sorry, you do not have enough money to purchase this item.');
  }
}


  function animate() {
    requestAnimationFrame(animate);
    scene.traverse((object) => {
      if (object instanceof THREE.Mesh) {
        object.rotation.x += 0.01;
        object.rotation.y += 0.01;
      }
    });
    renderer.render(scene, camera);
  }
</script>

<div class="page-container" style="background-color: white;">
  <h1>Coin Toss Game</h1>
  <p>Balance: $ {$balance}</p>
  <label for="guess-input">Guess heads or tails:</label>
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
      {#each $purchasedItems as {name, object, renderer}, i}
      {#if object.visible}
      <li>
        {name} ({i+1})
        <div bind:this="{renderer.domElement}"></div>
      </li>
      {/if}
      {/each}
    </ul>
  </div>
  <div bind:this="{itemContainer}"></div>
</div>
