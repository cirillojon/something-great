<script>
  import {
    Canvas,
    Scene,
    PerspectiveCamera,
    DirectionalLight,
    AmbientLight,
    WebGLRenderer,
  } from "https://unpkg.com/svelthree@latest/dist/svelthree.mjs";
	
	import Cube from "./Cube.svelte"
	import {rotateCube} from "./animations.js"
	
	let parentScale = [1, 1, 1]
	let childScale1 = [0.5, 0.5, 0.5]
	let childScale2 = [0.4, 0.4, 0.4]
	
	let offsetPos1 = 0.8
	let pos1 = {
		left: 	[offsetPos1, 0, 0],
		right: 	[-offsetPos1, 0, 0],
		top: 		[0, offsetPos1, 0],
		bottom: [0, -offsetPos1, 0],
		front: 	[0, 0, offsetPos1],
		back: 	[0, 0, -offsetPos1]
	}
	
	let offsetPos2 = 0.7
	let pos2 = {
		left: 	[offsetPos2, 0, 0],
		right: 	[-offsetPos2, 0, 0],
		top: 		[0, offsetPos2, 0],
		bottom: [0, -offsetPos2, 0],
		front: 	[0, 0, offsetPos2],
		back: 	[0, 0, -offsetPos2]
	}
	
	let svelteColor = 0xff3e00
	let yellow = 0xF6E05E
	let green = 0x68D391
	let blue = 0x63B3ED
  
</script>

<Canvas let:sti w={500} h={500}>

  <Scene {sti} let:scene id="scene1" props={{ background: 0xedf2f7 }}>

    <PerspectiveCamera {scene} id="cam1" pos={[0, 0, 3]} lookAt={[0, 0, 0]} />
    <AmbientLight {scene} intensity={1.25} />
    <DirectionalLight {scene} pos={[3, 3, 3]} castShadow shadowMapSize={8*512}/>

		<!-- Cube component as parent (level 0) -->
		<Cube {scene} scale={parentScale} let:parent mat={{color: svelteColor}}>
			
			<!-- Cube components as children (level 1) -->
			<Cube {scene} {parent} scale={childScale1} pos={pos1.left} 		mat={{color: yellow}}/>
			<Cube {scene} {parent} scale={childScale1} pos={pos1.right} 	mat={{color: yellow}}/>
			<Cube {scene} {parent} scale={childScale1} pos={pos1.top} 		mat={{color: yellow}}/>
			<Cube {scene} {parent} scale={childScale1} pos={pos1.bottom} 	mat={{color: yellow}}/>
			<Cube {scene} {parent} scale={childScale1} pos={pos1.back} 		mat={{color: yellow}}/>
			
			<!-- Cube component as child / parent (level 1) -->
			<Cube {scene} {parent} let:parent scale={childScale2} pos={pos1.front} mat={{color: green}}>
				
				<!-- Cube components as children (level 2) -->
				<Cube {scene} {parent} scale={childScale2} pos={pos2.left} 		mat={{color: blue}}/>
				<Cube {scene} {parent} scale={childScale2} pos={pos2.right} 	mat={{color: blue}}/>
				<Cube {scene} {parent} scale={childScale2} pos={pos2.top} 		mat={{color: blue}}/>
				<Cube {scene} {parent} scale={childScale2} pos={pos2.bottom} 	mat={{color: blue}}/>
				<Cube {scene} {parent} scale={childScale2} pos={pos2.front} 	mat={{color: blue}}/>
				<Cube {scene} {parent} scale={childScale2} pos={pos2.back} 		mat={{color: blue}}/>
			</Cube>
			
		</Cube>
		
  </Scene>

  <WebGLRenderer
    {sti}
    sceneId="scene1"
    camId="cam1"
    config={{ antialias: true, alpha: true }} enableShadowMap />

</Canvas>
