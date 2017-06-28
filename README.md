# JavaScript-ThreeJS
<!DOCTYPE html>


<html>
	<head>
		<script src="three.min.js"></script>

		<script>
			// функция за създаване на сцената
			function createScene(addFrame)
			{
				// нагласяване на цвета и центрирането на текста с имената ви
				document.getElementsByTagName('h1')[0].style = 'color:white; text-align:center;';

				// създаване на рисувателното поле на цял екран
				renderer = new THREE.WebGLRenderer({antialias:true});
				renderer.setSize( window.innerWidth, window.innerHeight );
				document.body.appendChild( renderer.domElement );
				renderer.domElement.style = 'width:100%; height:100%; position:fixed; top:0; left:0; z-index:-1;';
				
0				// създаване на сцена и камера
				scene = new THREE.Scene();
				camera = new THREE.PerspectiveCamera( 30, window.innerWidth/window.innerHeight, 0.1, 1000 );

				// създаване на земята като плоска равнина
				var ground = new THREE.Mesh(
					new THREE.PlaneGeometry(13000,13000),
					new THREE.MeshPhongMaterial({color:'goldenrod'})
				);
				ground.position.set(0,-11,0);
				ground.rotation.set(-Math.PI/2,0,0);
				scene.add( ground );

				// ако сме поискали (чрез параметър true),
				// създаваме полупрозрачна рамка около зоната,
				//в която трябва да е поместен йероглифа
				if (addFrame)
				{
					// прозрачи стени
					var walls = new THREE.Mesh(
						new THREE.BoxGeometry(21,21,11),
						new THREE.MeshPhongMaterial({color:'white',shininess:220,opacity:0.6,transparent:true,side:THREE.DoubleSide})
					);
					scene.add(walls);

					// тънък бял кант
					var frame = new THREE.BoxHelper(walls);
					frame.material.color.set('white');
					scene.add( frame );
				}

				// създаване на четири светлини с различни цветове
				lights=[];
				var colors=['dodgerblue','hotpink','cyan','fuchsia'];
				for (var i=0; i<4; i++)
				{
					lights[i] = new THREE.PointLight(colors[i],1);
					scene.add(lights[i]);
				}
				
				// активиране на анимацията
				drawFrame();
			}
			
			// функция за анимиране на сцената
			var t = 0; // време
			function drawFrame()
			{
				requestAnimationFrame( drawFrame );
				
				t++;
				
				// леко въртене на сцената
				scene.rotation.x = Math.sin(t/135)/10;
				scene.rotation.y = Math.sin(t/200)/2;
				scene.rotation.z = Math.cos(t/111)/10;

				// приближаване и отдалечаване на камерата
				camera.position.set(0,3,60+10*Math.sin(t/250));
				camera.lookAt(new THREE.Vector3(0,0,0));

				// движение на светлините
				for (var i=0; i<4; i++)
				{
					var angle = t/100+Math.PI/2*i;
					lights[i].position.set(
						15*Math.cos(angle),
						10*Math.sin(1.5*angle)+5,
						10);
				}
				
				//рисуване на сцената
				renderer.render( scene, camera );
			}		
		</script>
	</head>
	
	<body>
		<!-- тук сложете вашите данни -->
		<h1>Канджи</h1>

		<script>
			createScene(true); // true=има рамка; false=няма рамка

			
			/*for (var i=-9; i<=9; i+=3)
			{
				var obj = new THREE.Mesh( new THREE.BoxGeometry(20,1,10), new THREE.MeshPhongMaterial() );
				obj.position.y = i;
				obj.rotation.z = Math.random()*0.2-0.1;
				scene.add( obj );
			}*/
			
			var wall = new THREE.Mesh( new THREE.BoxGeometry(1,14,10),new THREE.MeshPhongMaterial() );
			wall.position.y = -3;
			scene.add(wall);
			
			var wall = new THREE.Mesh( new THREE.BoxGeometry(1,7,10),new THREE.MeshPhongMaterial() );
			wall.position.x = -4.5;
			scene.add(wall);

			var wall = new THREE.Mesh( new THREE.BoxGeometry(1,7,10),new THREE.MeshPhongMaterial() );
			wall.position.x = 4.5;
			scene.add(wall);
			
			var wall = new THREE.Mesh( new THREE.BoxGeometry(1,2,10),new THREE.MeshPhongMaterial() );
			wall.position.y = 9;
			scene.add(wall);
			
			var wall = new THREE.Mesh( new THREE.BoxGeometry(14,1,10),new THREE.MeshPhongMaterial() );
			wall.position.y = 1;
			scene.add(wall);
			
			var wall = new THREE.Mesh( new THREE.BoxGeometry(10,1,10),new THREE.MeshPhongMaterial() );
			wall.position.y = -3;
			scene.add(wall);
			
			var wall = new THREE.Mesh( new THREE.BoxGeometry(13,1,10),new THREE.MeshPhongMaterial() );
			wall.position.y = -5;
			scene.add(wall);
			
			var wall = new THREE.Mesh( new THREE.BoxGeometry(13,1,10),new THREE.MeshPhongMaterial() );
			wall.position.y = 8;
			scene.add(wall);
			
			
			var shape = new THREE.Shape();
			shape.moveTo(   0, -5.5 );
			shape.lineTo(	4.5 , -5.5 );
			shape.quadraticCurveTo( -2.2,-4.6, 7.5, -8.5);
			shape.lineTo(	6.5,-9);
			shape.quadraticCurveTo(-2.5,-5.0,1.9,-4.5);
			
			
			var extrudeSettings = { amount: 10, bevelEnabled: false, curveSegments: 50};
			var geometry = new THREE.ExtrudeGeometry( shape, extrudeSettings );
			
			var border = new THREE.Mesh( geometry, new THREE.MeshPhongMaterial({shininess:140,specular:'white'}) );
			border.position.z = -5;
			scene.add( border );
						
			
			var border2 = new THREE.Mesh( geometry, new THREE.MeshPhongMaterial({shininess:140,specular:'white'}) );
			border2.position.set (0, 0, 5);
			border2.rotation.y = Math.PI ;
			scene.add( border2 );
			
			var shape = new THREE.Shape();
			shape.moveTo(   -6, 6 );
			shape.lineTo(	-5.25 , 4.5 );
			shape.quadraticCurveTo( 2,2, 5.5, 5);
			shape.lineTo(	5.5,6);
			shape.quadraticCurveTo(2,2,-6,6);
			
			var extrudeSettings = {amount : 10,bevelEnabled: false, curveSegments: 1};
			var geometry = new THREE.ExtrudeGeometry(shape,extrudeSettings);
			
			var border3 = new THREE.Mesh( geometry, new THREE.MeshPhongMaterial({shininess:140, specular:'white'}) );
			border3.position.z = -5;
			scene.add(border3);
			
			var shape = new THREE.Shape();
			shape.moveTo(	-3.5,6 );
			shape.lineTo(	-2,6 );
			shape.lineTo(	-0.5, 8);
			shape.lineTo(	-2,8);
			
			var extrudeSettings = {amount : 10,bevelEnabled: false, curveSegments: 1};
			var geometry = new THREE.ExtrudeGeometry(shape,extrudeSettings);
			
			var border4 = new THREE.Mesh( geometry, new THREE.MeshPhongMaterial({shininess:140, specular:'white'}) );
			border4.position.z = -5;
			scene.add(border4);
			
			var shape = new THREE.Shape();
			shape.moveTo(	6, 4);
			shape.lineTo(	7, 5);
			shape.quadraticCurveTo(6,7,4,8.5);
			shape.lineTo(	3, 7.5);
			shape.quadraticCurveTo(5,6,6,4);
			
			var extrudeSettings = {amount : 10,bevelEnabled: false, curveSegments: 50};
			var geometry = new THREE.ExtrudeGeometry(shape,extrudeSettings);
			
			var border5 = new THREE.Mesh( geometry, new THREE.MeshPhongMaterial({shininess:140, specular:'white'}) );
			border5.position.z = -5;
			scene.add(border5);
			
			
			
		</script>
	</body>
</html>
