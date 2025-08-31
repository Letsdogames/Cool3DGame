
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cool3DGame</title>
    <style>
        body { margin: 0; overflow: hidden; }
        canvas { display: block; }
        #info {
            position: absolute;
            top: 30px;
            left: 10px;
            color: white;
            font-family: Arial, sans-serif;
            background: rgba(0, 0, 0, 0.7);
            padding: 10px;
            z-index: 10;
        }
        #startPrompt {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            color: white;
            font-family: Arial, sans-serif;
            background: rgba(0, 0, 0, 0.7);
            padding: 20px;
            text-align: center;
            z-index: 20;
        }
        h1, h2, h3, h4, h5, h6 { display: none; }
    </style>
</head>
<body>
    <div id="info">WASD: Move | Space: Jump | Mouse: Look | Shift: Lock/Unlock Mouse | Arrow Keys: Rotate (Fallback)</div>
    <div id="startPrompt">Click to start the game and lock the mouse!</div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r134/three.min.js"></script>
    <script>
        // Scene setup
        const scene = new THREE.Scene();
        scene.background = new THREE.Color(0x87ceeb);
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer({ antialias: true }); // Enable antialiasing for smoother visuals
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);

        // Create custom humanoid figure
        const humanoid = new THREE.Group();
        const headGeometry = new THREE.SphereGeometry(0.3, 32, 32);
        const headMaterial = new THREE.MeshStandardMaterial({ color: 0xf0c8a0 });
        const head = new THREE.Mesh(headGeometry, headMaterial);
        head.position.y = 1.6;
        humanoid.add(head);

        const torsoGeometry = new THREE.CylinderGeometry(0.3, 0.3, 0.8, 32);
        const torsoMaterial = new THREE.MeshStandardMaterial({ color: 0xf0c8a0 });
        const torso = new THREE.Mesh(torsoGeometry, torsoMaterial);
        torso.position.y = 1.0;
        humanoid.add(torso);

        const shortsGeometry = new THREE.BoxGeometry(0.4, 0.2, 0.2);
        const shortsMaterial = new THREE.MeshStandardMaterial({ color: 0x1a1a1a });
        const shorts = new THREE.Mesh(shortsGeometry, shortsMaterial);
        shorts.position.y = 0.5;
        humanoid.add(shorts);

        const armGeometry = new THREE.CylinderGeometry(0.1, 0.1, 0.8, 32);
        const armMaterial = new THREE.MeshStandardMaterial({ color: 0xf0c8a0 });
        const leftArm = new THREE.Mesh(armGeometry, armMaterial);
        leftArm.position.set(-0.7, 1.2, 0);
        leftArm.rotation.z = Math.PI / 2;
        humanoid.add(leftArm);

        const rightArm = new THREE.Mesh(armGeometry, armMaterial);
        rightArm.position.set(0.7, 1.2, 0);
        rightArm.rotation.z = Math.PI / 2;
        humanoid.add(rightArm);

        const legGeometry = new THREE.CylinderGeometry(0.15, 0.15, 0.7, 32);
        const legMaterial = new THREE.MeshStandardMaterial({ color: 0xf0c8a0 });
        const leftLeg = new THREE.Mesh(legGeometry, legMaterial);
        leftLeg.position.set(-0.2, 0.2, 0);
        humanoid.add(leftLeg);

        const rightLeg = new THREE.Mesh(legGeometry, legMaterial);
        rightLeg.position.set(0.2, 0.2, 0);
        humanoid.add(rightLeg);

        scene.add(humanoid);

        // Ground plane with grass texture
        const textureLoader = new THREE.TextureLoader();
        const grassTexture = textureLoader.load(
            'https://threejs.org/examples/textures/terrain/grasslight-big.jpg'
        );
        grassTexture.wrapS = grassTexture.wrapT = THREE.RepeatWrapping;
        grassTexture.repeat.set(10, 10);
        const groundGeometry = new THREE.PlaneGeometry(50, 50);
        const groundMaterial = new THREE.MeshStandardMaterial({
            map: grassTexture,
            side: THREE.DoubleSide
        });
        const ground = new THREE.Mesh(groundGeometry, groundMaterial);
        ground.rotation.x = -Math.PI / 2;
        ground.position.y = -0.1;
        scene.add(ground);

        // Lighting
        const ambientLight = new THREE.AmbientLight(0x404040);
        scene.add(ambientLight);
        const pointLight = new THREE.PointLight(0xffffff, 1);
        pointLight.position.set(5, 5, 5);
        scene.add(pointLight);

        // Camera setup (third-person)
        camera.position.set(0, 2, 5);

        // Keyboard input
        const keys = {};
        window.addEventListener('keydown', (event) => {
            keys[event.key.toLowerCase()] = true;
            if (event.key === 'Shift') {
                if (document.pointerLockElement === renderer.domElement) {
                    document.exitPointerLock();
                } else {
                    renderer.domElement.requestPointerLock();
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
            document.getElementById('startPrompt').style.display = isMouseLocked ? 'none' : 'block';
        });

        document.addEventListener('click', () => {
            if (!isMouseLocked) {
                renderer.domElement.requestPointerLock().catch((error) => {
                    console.error('Pointer Lock failed:', error);
                    alert('Pointer Lock not supported. Use arrow keys to rotate.');
                    document.getElementById('startPrompt').style.display = 'none';
                });
            }
        });

        const mouseSensitivity = 0.002;
        const rotateSpeed = 0.05;
        let yaw = 0; // Horizontal rotation
        let pitch = 0; // Vertical rotation
        const maxPitch = Math.PI / 2; // Limit vertical look to 90 degrees
        document.addEventListener('mousemove', (event) => {
            if (isMouseLocked) {
                yaw -= event.movementX * mouseSensitivity;
                pitch -= event.movementY * mouseSensitivity;
                pitch = Math.max(-maxPitch, Math.min(maxPitch, pitch)); // Clamp pitch
            }
        });

        // Movement and physics
        const moveSpeed = 0.1;
        const gravity = 0.02; // Increased gravity for less floaty jumps
        const jumpStrength = 0.4; // Adjusted for balance
        let velocity = new THREE.Vector3(0, 0, 0);
        let isJumping = false;
        let lastJumpTime = 0;
        const jumpCooldown = 500; // 500ms cooldown between jumps
        let lastTime = performance.now();

        // Camera smoothing
        const cameraOffset = new THREE.Vector3(0, 2, 5); // Base offset from humanoid
        const cameraLerpSpeed = 0.1; // Smoothing factor

        function animate() {
            requestAnimationFrame(animate);

            const currentTime = performance.now();
            const deltaTime = (currentTime - lastTime) / 16.67; // Normalize to ~60 FPS
            lastTime = currentTime;

            // Movement (WASD) relative to camera yaw
            let moveX = 0;
            let moveZ = 0;
            if (keys['w']) moveZ -= 1;
            if (keys['s']) moveZ += 1;
            if (keys['a']) moveX -= 1;
            if (keys['d']) moveX += 1;

            // Normalize movement
            const magnitude = Math.sqrt(moveX * moveX + moveZ * moveZ);
            if (magnitude > 0) {
                moveX = (moveX / magnitude) * moveSpeed * deltaTime;
                moveZ = (moveZ / magnitude) * moveSpeed * deltaTime;
            }

            // Apply movement relative to yaw
            const moveDir = new THREE.Vector3(
                moveX * Math.cos(yaw) - moveZ * Math.sin(yaw),
                0,
                moveZ * Math.cos(yaw) + moveX * Math.sin(yaw)
            );
            humanoid.position.add(moveDir);

            // Apply gravity
            velocity.y -= gravity * deltaTime;
            humanoid.position.y += velocity.y * deltaTime;

            // Ground collision
            if (humanoid.position.y <= 0) {
                humanoid.position.y = 0;
                velocity.y = 0;
                isJumping = false;
            }

            // Jumping with cooldown
            if (keys[' '] && !isJumping && currentTime - lastJumpTime > jumpCooldown) {
                velocity.y = jumpStrength;
                isJumping = true;
                lastJumpTime = currentTime;
            }

            // Fallback rotation (arrow keys)
            if (!isMouseLocked) {
                if (keys['arrowleft']) yaw += rotateSpeed * deltaTime;
                if (keys['arrowright']) yaw -= rotateSpeed * deltaTime;
            }

            // Update humanoid rotation
            humanoid.rotation.y = yaw;

            // Smooth camera positioning
            const targetCameraPos = humanoid.position.clone().add(
                cameraOffset.clone().applyAxisAngle(new THREE.Vector3(0, 1, 0), yaw)
            );
            camera.position.lerp(targetCameraPos, cameraLerpSpeed);
            camera.position.y = humanoid.position.y + 2; // Keep height consistent
            camera.lookAt(humanoid.position.clone().add(new THREE.Vector3(0, 1, 0)));

            renderer.render(scene, camera);
        }
        animate();

        // Handle window resize
        window.addEventListener('resize', () => {
            renderer.setSize(window.innerWidth, window.innerHeight);
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
        });

        // Show start prompt
        window.addEventListener('load', () => {
            document.getElementById('startPrompt').style.display = 'block';
        });
    </script>
</body>
</html>
