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
	import { Color3 } from "@babylonjs/core/Maths/math";
	import { AdvancedDynamicTexture, StackPanel, Control, Button } from "@babylonjs/gui"

	var arcCamera, freeCamera, box1, box2, box3, panel;
	var arcCameraOn = true;

	var createScene = function (canvas, engine) {
			var scene = new Scene(engine);

			var light0 = new PointLight("Omni", new Vector3(0, 2, 8), scene);
			light0.diffuse = new Color3(1, 0.5, 0.3);

			var light1 = new PointLight("Omni2", new Vector3(-3, 0, -5), scene);
			light1.diffuse = new Color3(0.3, 0, 0.5);

			box1 = Mesh.CreateBox("b1", 1.0, scene);
			box2 = Mesh.CreateBox("b2", 1.0, scene);
			box2.position.x = -3;
			box3 = Mesh.CreateBox("b3", 1.0, scene);
			box3.position.x = 3;

			arcCamera = new ArcRotateCamera("ArcRotateCamera", 1, 0.8, 1, new Vector3(0, 0, 0), scene);
			arcCamera.setPosition(new Vector3(0, 0, 15));
			arcCamera.target = new Vector3(3, 0, 0);

			freeCamera = new FreeCamera("FreeCamera", new Vector3(0, 0, 5), scene);
			freeCamera.rotation = new Vector3(0, Math.PI, 0);

			var touchCamera = new TouchCamera("TouchCamera", new Vector3(0, 0, 10), scene);
			touchCamera.rotation = new Vector3(0, Math.PI, 0);

			//scene.activeCamera = freeCamera;
			freeCamera.attachControl(canvas, true);
			scene.activeCamera = arcCamera;
			arcCamera.attachControl(canvas, true);
			//scene.activeCamera = touchCamera;
			//touchCamera.attachControl(canvas, true);

			var advancedTexture = AdvancedDynamicTexture.CreateFullscreenUI("ui1");

			panel = new StackPanel();  
			panel.width = 0.25;
			panel.rotation = 0;
			panel.horizontalAlignment = Control.HORIZONTAL_ALIGNMENT_LEFT;
			advancedTexture.addControl(panel);

			var button1 = Button.CreateSimpleButton("but1", "Change camera");
			button1.width = 0.4;
			button1.height = "40px";
			button1.color = "white";
			button1.cornerRadius = 20;
			button1.background = "green";
			button1.onPointerUpObservable.add(function() {
				if (arcCameraOn) {
					arcCameraOn = false;
					scene.activeCamera = freeCamera;
					//freeCamera.attachControl(canvas, true);
				} else {
					arcCameraOn = true;
					scene.activeCamera = arcCamera;
					//arcCamera.attachControl(canvas, true);
				}
			});
			panel.addControl(button1);

			return scene;
	}	

	var startRender = function() {
		var canvas = document.getElementById("renderCanvas");
		var engine = new Engine(canvas, true);
		var scene = createScene(canvas, engine);

		engine.runRenderLoop(() => {
			arcCamera.alpha -= 0.01;
			arcCamera.beta += 0;
			box1.rotation.x += 0.01;
			box2.rotation.y += 0.01;
			box3.rotation.z += 0.01;
			panel.rotation += 0.01;
			scene.render();
		}); 
	}
</script>

<main>
	<canvas id="renderCanvas" use:startRender></canvas>
</main>

<style>
</style>