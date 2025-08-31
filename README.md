
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3D Humanoid Game with Custom Figure</title>
    <style>
        body { margin: 0; overflow: hidden; }
        canvas { display: block; }
        #info { position: absolute; top: 10px; left: 10px; color: white; font-family: Arial, sans-serif; background: rgba(0, 0, 0, 0.7); padding: 10px; }
        #startPrompt { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); color: white; font-family: Arial, sans-serif; background: rgba(0, 0, 0, 0.7); padding: 20px; text-align: center; }
    </style>
</head>
<body>
    <div id="info">WASD: Move | Mouse or Arrow Keys: Rotate | Shift: Lock/Unlock Mouse</div>
    <div id="startPrompt">Click to start the game and lock the mouse!</div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r134/three.min.js"></script>
    <script>
        // Scene setup
        const scene = new THREE.Scene();
        scene.background = new THREE.Color(0x87ceeb); // Sky blue background
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer();
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        // Create custom humanoid figure
        const humanoid = new THREE.Group();

        // Head (sphere)
        const headGeometry = new THREE.SphereGeometry(0.3, 32, 32);
        const headMaterial = new THREE.MeshStandardMaterial({ color: 0xf0c8a0 }); // Light skin tone
        const head = new THREE.Mesh(headGeometry, headMaterial);
        head.position.y = 1.6;
        humanoid.add(head);

        // Torso (cylinder)
        const torsoGeometry = new THREE.CylinderGeometry(0.3, 0.3, 0.8, 32);
        const torsoMaterial = new THREE.MeshStandardMaterial({ color: 0xf0c8a0 }); // Light skin tone
        const torso = new THREE.Mesh(torsoGeometry, torsoMaterial);
        torso.position.y = 1.0;
        humanoid.add(torso);

        // Shorts (cube)
        const shortsGeometry = new THREE.BoxGeometry(0.4, 0.2, 0.2);
        const shortsMaterial = new THREE.MeshStandardMaterial({ color: 0x1a1a1a }); // Black shorts
        const shorts = new THREE.Mesh(shortsGeometry, shortsMaterial);
        shorts.position.y = 0.5;
        humanoid.add(shorts);

        // Left Arm (cylinder, outstretched)
        const armGeometry = new THREE.CylinderGeometry(0.1, 0.1, 0.8, 32);
        const armMaterial = new THREE.MeshStandardMaterial({ color: 0xf0c8a0 }); // Light skin tone
        const leftArm = new THREE.Mesh(armGeometry, armMaterial);
        leftArm.position.set(-0.7, 1.2, 0);
        leftArm.rotation.z = Math.PI / 2; // Horizontal
        humanoid.add(leftArm);

        // Right Arm (cylinder, outstretched)
        const rightArm = new THREE.Mesh(armGeometry, armMaterial);
        rightArm.position.set(0.7, 1.2, 0);
        rightArm.rotation.z = Math.PI / 2; // Horizontal
        humanoid.add(rightArm);

        // Left Leg (cylinder)
        const legGeometry = new THREE.CylinderGeometry(0.15, 0.15, 0.7, 32);
        const legMaterial = new THREE.MeshStandardMaterial({ color: 0xf0c8a0 }); // Light skin tone
        const leftLeg = new THREE.Mesh(legGeometry, legMaterial);
        leftLeg.position.set(-0.2, 0.2, 0);
        humanoid.add(leftLeg);

        // Right Leg (cylinder)
        const rightLeg = new THREE.Mesh(legGeometry, legMaterial);
        rightLeg.position.set(0.2, 0.2, 0);
        humanoid.add(rightLeg);

        scene.add(humanoid);

        // Load grass texture for ground
        const textureLoader = new THREE.TextureLoader();
        const grassTexture = textureLoader.load(
            'https://threejs.org/examples/textures/terrain/grasslight-big.jpg'
        );
        grassTexture.wrapS = grassTexture.wrapT = THREE.RepeatWrapping;
        grassTexture.repeat.set(10, 10);

        // Create ground plane with grass texture
        const groundGeometry = new THREE.PlaneGeometry(50, 50);
        const groundMaterial = new THREE.MeshStandardMaterial({
            map: grassTexture,
            side: THREE.DoubleSide
        });
        const ground = new THREE.Mesh(groundGeometry, groundMaterial);
        ground.rotation.x = -Math.PI / 2;
        ground.position.y = -0.1;
        scene.add(ground);

        // Add lighting
        const ambientLight = new THREE.AmbientLight(0x404040);
        scene.add(ambientLight);
        const pointLight = new THREE.PointLight(0xffffff, 1);
        pointLight.position.set(5, 5, 5);
        scene.add(pointLight);

        // Camera setup (third-person view)
        camera.position.set(0, 2, 5);

        // Keyboard input
        const keys = {};
        window.addEventListener('keydown', (event) => {
            keys[event.key.toLowerCase()] = true;
            if (event.key === 'Shift') {
                if (document.pointerLockElement === renderer.domElement) {
                    document.exitPointerLock();
                } else {
                    try {
                        renderer.domElement.requestPointerLock();
                    } catch (error) {
                        console.error('Pointer Lock failed:', error);
                        alert('Pointer Lock not supported. Use arrow keys to rotate.');
                    }
                }
            }
        });
        window.addEventListener('keyup', (event) => {
            keys[event.key.toLowerCase()] = false;
        });

        // Mouse movement for rotation
        let isMouseLocked = false;
        document.addEventListener('pointerlockchange', () => {
            isMouseLocked = document.pointerLockElement === renderer.domElement;
            if (isMouseLocked) {
                document.getElementById('startPrompt').style.display = 'none';
            }
        });

        // Request pointer lock on page load after a user click
        let hasClicked = false;
        document.addEventListener('click', () => {
            if (!hasClicked) {
                hasClicked = true;
                try {
                    renderer.domElement.requestPointerLock();
                } catch (error) {
                    console.error('Pointer Lock failed:', error);
                    alert('Pointer Lock not supported in this environment. Use arrow keys to rotate.');
                    document.getElementById('startPrompt').style.display = 'none';
                }
            }
        });

        const mouseSensitivity = 0.002;
        const rotateSpeed = 0.05;
        document.addEventListener('mousemove', (event) => {
            if (isMouseLocked) {
                const movementX = event.movementX || 0;
                humanoid.rotation.y -= movementX * mouseSensitivity;
            }
        });

        // Animation loop
        const moveSpeed = 0.1;
        function animate() {
            requestAnimationFrame(animate);

            // Movement (WASD)
            if (keys['w']) {
                humanoid.position.z -= moveSpeed * Math.cos(humanoid.rotation.y);
                humanoid.position.x -= moveSpeed * Math.sin(humanoid.rotation.y);
            }
            if (keys['s']) {
                humanoid.position.z += moveSpeed * Math.cos(humanoid.rotation.y);
                humanoid.position.x += moveSpeed * Math.sin(humanoid.rotation.y);
            }
            if (keys['a']) {
                humanoid.position.x -= moveSpeed * Math.cos(humanoid.rotation.y);
                humanoid.position.z -= moveSpeed * Math.sin(humanoid.rotation.y);
            }
            if (keys['d']) {
                humanoid.position.x += moveSpeed * Math.cos(humanoid.rotation.y);
                humanoid.position.z += moveSpeed * Math.sin(humanoid.rotation.y);
            }

            // Fallback rotation (Arrow keys)
            if (!isMouseLocked) {
                if (keys['arrowleft']) humanoid.rotation.y += rotateSpeed;
                if (keys['arrowright']) humanoid.rotation.y -= rotateSpeed;
            }

            // Update camera to follow humanoid (third-person)
            camera.position.x = humanoid.position.x + 5 * Math.sin(humanoid.rotation.y);
            camera.position.z = humanoid.position.z + 5 * Math.cos(humanoid.rotation.y);
            camera.position.y = humanoid.position.y + 2;
            camera.lookAt(humanoid.position);

            renderer.render(scene, camera);
        }
        animate();

        // Handle window resize
        window.addEventListener('resize', () => {
            renderer.setSize(window.innerWidth, window.innerHeight);
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
        });

        // Show start prompt on load
        window.addEventListener('load', () => {
            document.getElementById('startPrompt').style.display = 'block';
        });
    </script>
</body>
</html>
