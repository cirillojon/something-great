<script>
	import {
		Mesh,
		BoxBufferGeometry,
		MeshStandardMaterial,
	} from "https://unpkg.com/svelthree@latest/dist/svelthree.mjs";	

	import {rotateCube} from "./animations.js"

	export let scene
	export let parent = undefined
	export let pos = [0,0,0] //default position
	export let scale = [1, 1, 1] //default scale
	
	let matDefault = {roughness: 0.5, metalness: 0.5} //default material props
	export let mat = undefined 
	
	//reactive update mat: apply provided mat props to matDefault props
	$: mat ? mat = {...mat, ...matDefault} : null
	
	export let animation = rotateCube //default animation

	let geometry = new BoxBufferGeometry(0.8, 0.8, 0.8);
	let material = new MeshStandardMaterial();
</script>

<Mesh {scene} {parent} let:parent
	{geometry}
	{material}
	{mat}
	{pos}
	{scale}
	{animation}
	aniauto
	receiveShadow
	castShadow >
		<slot {parent}/>
</Mesh>
	