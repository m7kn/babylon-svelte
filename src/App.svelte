<script>
	import { Engine } from "@babylonjs/core/Engines/engine";
	import { Scene } from "@babylonjs/core/scene";
	import "@babylonjs/core/Meshes/meshBuilder";
	import "@babylonjs/core/Materials/standardMaterial";	
	import { Mesh } from "@babylonjs/core/Meshes/mesh";	
	import { Vector3 } from "@babylonjs/core/Maths/math";
	import { FreeCamera } from "@babylonjs/core/Cameras/freeCamera";
	import { ArcRotateCamera } from "@babylonjs/core/Cameras/arcRotateCamera";
	import { TouchCamera } from "@babylonjs/core/Cameras/touchCamera";	
	import { PointLight } from "@babylonjs/core/Lights/pointLight";

	var createScene = function (canvas, engine) {
			var scene = new Scene(engine);

			var light0 = new PointLight("Omni", new Vector3(0, 2, 8), scene);
			var box1 = Mesh.CreateBox("b1", 1.0, scene);
			var box2 = Mesh.CreateBox("b2", 1.0, scene);
			box2.position.x = -3;
			var box3 = Mesh.CreateBox("b3", 1.0, scene);
			box3.position.x = 3;

			var arcCamera = new ArcRotateCamera("ArcRotateCamera", 1, 0.8, 10, new Vector3(0, 0, 0), scene);
			arcCamera.setPosition(new Vector3(0, 0, 50));
			arcCamera.target = new Vector3(3, 0, 0);

			var freeCamera = new FreeCamera("FreeCamera", new Vector3(0, 0, 5), scene);
			freeCamera.rotation = new Vector3(0, Math.PI, 0);

			var touchCamera = new TouchCamera("TouchCamera", new Vector3(0, 0, 10), scene);
			touchCamera.rotation = new Vector3(0, Math.PI, 0);

			scene.activeCamera = freeCamera;
			freeCamera.attachControl(canvas, true);

			return scene;
	}	

	var startRender = function() {
		var canvas = document.getElementById("renderCanvas");
		var engine = new Engine(canvas, true);
		var scene = createScene(canvas, engine);

		engine.runRenderLoop(() => {
			scene.render();
		}); 
	}
</script>

<main>
	<canvas id="renderCanvas" use:startRender></canvas>
</main>

<style>
</style>