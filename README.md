# 1586290
Bons
<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Robot Virtual gwtoniiiiii</title>
  <style>
    * { box-sizing: border-box; }

    body {
      margin: 0;
      overflow-x: hidden;
      background: #101725;
      color: #eaf2ff;
      font-family: system-ui, sans-serif;
    }

    header {
      padding: 16px 20px 6px;
    }

    h1 {
      margin: 0;
      font-size: 22px;
    }

    header p {
      margin: 6px 0 0;
      color: #adb9cc;
    }

    main {
      display: grid;
      grid-template-columns: minmax(0, 1fr) 320px;
      gap: 14px;
      padding: 14px 20px 20px;
    }

    #scene {
      min-height: 520px;
      overflow: hidden;
      border: 1px solid #293850;
      border-radius: 12px;
      background: #182337;
    }

    aside {
      height: fit-content;
      padding: 16px;
      border: 1px solid #293850;
      border-radius: 12px;
      background: #182337;
    }

    label {
      display: block;
      margin: 14px 0 6px;
    }

    input {
      width: 100%;
      padding: 11px;
      border: 1px solid #394b67;
      border-radius: 7px;
      background: #101725;
      color: white;
      font-size: 16px;
    }

    button {
      min-height: 42px;
      border: 0;
      border-radius: 7px;
      background: #3478f6;
      color: white;
      cursor: pointer;
    }

    button:hover {
      background: #4a89ff;
    }

    .actions {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 8px;
      margin-top: 8px;
    }

    #status,
    #detections,
    .help {
      margin-top: 12px;
      padding: 11px;
      border-radius: 7px;
      background: #101725;
      line-height: 1.5;
    }

    #status {
      color: #a8d7ff;
    }

    #detections {
      color: #d2dbec;
    }

    .help {
      color: #adb9cc;
      font-size: 13px;
    }

    @media (max-width: 800px) {
      main {
        grid-template-columns: 1fr;
        padding: 10px;
      }

      #scene {
        min-height: 390px;
      }

      aside {
        margin-bottom: 10px;
      }
    }
  </style>
</head>

<body>
  <header>
    <h1>Robot Virtual Test</h1>
    <p>Robot 3D dengan perintah teks, suara, deteksi objek, roda, dan lengan.</p>
  </header>

  <main>
    <div id="scene"></div>

    <aside>
      <strong>Panel Kendali</strong>

      <label for="command">Perintah robot</label>melompati semua benda
      <input
        id="command"
        type="text"
        placeholder="Contoh: cari kotak merah"
      />

      <div class="actions">
        <button id="send" type="button">Kirim</button>
        <button id="mic" type="button">🎙 Suara</button>Kita ganteng
      </div>

      <button
        id="scan"
        type="button"
        style="width: 100%; margin-top: 8px"
      >
        Pindai Kamera
      </button>

      <div id="status">Status: siap menerima perintah.</div>
      <div id="detections">Kamera:scan .</div>

      <div class="help">
        Contoh perintah:
        <b>maju</b>, <b>mundur</b>, <b>kiri</b>, <b>kanan</b>,
        <b>berhenti</b>, <b>pindai kamera</b>,
        <b>cari kotak merah</b>, atau <b>ambil kotak biru</b>.
      </div>
    </aside>
  </main>

  <script type="module">
    import * as THREE from
      "https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js";

    const mount = document.querySelector("#scene");
    const statusElement = document.querySelector("#status");
    const detectionElement = document.querySelector("#detections");
    const commandInput = document.querySelector("#command");

    const scene = new THREE.Scene();
    scene.background = new THREE.Color("#182337");

    const camera = new THREE.PerspectiveCamera(48, 1, 0.1, 100);
    camera.position.set(8, 10, 12);
    camera.lookAt(0, 0, 0);

    const renderer = new THREE.WebGLRenderer({ antialias: true });
    renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
    mount.appendChild(renderer.domElement);

    scene.add(new THREE.HemisphereLight(
      0xd9eaff,
      0x293449,
      2
    ));

    const sunlight = new THREE.DirectionalLight(0xffffff, 2.2);
    sunlight.position.set(5, 10, 6);
    scene.add(sunlight);

    const floor = new THREE.Mesh(
      new THREE.PlaneGeometry(24, 24),
      new THREE.MeshStandardMaterial({
        color: "#26364d",
        roughness: 0.9
      })
    );
    floor.rotation.x = -Math.PI / 2;
    floor.position.y = -0.03;
    scene.add(floor);

    const grid = new THREE.GridHelper(
      24,
      24,
      "#536781",
      "#34465d"
    );
    grid.position.y = 0;
    scene.add(grid);

    const robot = new THREE.Group();
    scene.add(robot);

    const bodyMaterial = new THREE.MeshStandardMaterial({
      color: "#2786ed"
    });

    const darkMaterial = new THREE.MeshStandardMaterial({
      color: "#172438"
    });

    const metalMaterial = new THREE.MeshStandardMaterial({
      color: "#a9c5e8",
      metalness: 0.45,
      roughness: 0.4
    });

    const body = new THREE.Mesh(
      new THREE.BoxGeometry(1.05, 0.65, 0.8),
      bodyMaterial
    );
    body.position.y = 0.66;
    robot.add(body);

    // Stempel nama gwtoniiiiii di badan robot.
    const labelCanvas = document.createElement("canvas");
    labelCanvas.width = 512;
    labelCanvas.height = 128;

    const labelContext = labelCanvas.getContext("2d");
    labelContext.fillStyle = "#eaf2ff";
    labelContext.font = "bold 64px system-ui, sans-serif";
    labelContext.textAlign = "center";
    labelContext.textBaseline = "middle";
    labelContext.fillText("gwtoniiiiii", 256, 64);

    const labelTexture = new THREE.CanvasTexture(labelCanvas);
    const labelMaterial = new THREE.MeshBasicMaterial({
      map: labelTexture,
      transparent: true,
      side: THREE.DoubleSide
    });

    const nameStamp = new THREE.Mesh(
      new THREE.PlaneGeometry(0.78, 0.20),
      labelMaterial
    );
    nameStamp.position.set(0, 0.66, -0.411);
    nameStamp.rotation.y = Math.PI;
    robot.add(nameStamp);

    const head = new THREE.Mesh(
      new THREE.SphereGeometry(0.25, 20, 16),
      metalMaterial
    );
    head.position.set(0, 1.15, -0.18);
    robot.add(head);

    const eye = new THREE.Mesh(
      new THREE.SphereGeometry(0.075, 12, 12),
      new THREE.MeshBasicMaterial({ color: "#65f0ff" })
    );
    eye.position.set(0, 1.16, -0.405);
    robot.add(eye);

    const wheels = [];

    for (const xPosition of [-0.58, 0.58]) {
      const wheel = new THREE.Mesh(
        new THREE.CylinderGeometry(0.24, 0.24, 0.16, 20),
        darkMaterial
      );
      wheel.rotation.z = Math.PI / 2;
      wheel.position.set(xPosition, 0.28, 0);
      robot.add(wheel);
      wheels.push(wheel);
    }

    const armPivot = new THREE.Group();
    armPivot.position.set(0, 0.9, -0.42);
    robot.add(armPivot);

    const upperArm = new THREE.Mesh(
      new THREE.BoxGeometry(0.16, 0.55, 0.16),
      metalMaterial
    );
    upperArm.position.y = -0.27;
    armPivot.add(upperArm);

    const claw = new THREE.Mesh(
      new THREE.BoxGeometry(0.32, 0.12, 0.2),
      new THREE.MeshStandardMaterial({ color: "#ffc857" })
    );
    claw.position.set(0, -0.58, -0.04);
    armPivot.add(claw);

    const objectColors = [
      { name: "merah", color: "#ef4444", x: -3, z: -4 },
      { name: "biru", color: "#3b82f6", x: 3, z: -5 },
      { name: "hijau", color: "#22c55e", x: 4, z: 2 },
      { name: "kuning", color: "#eab308", x: -4, z: 3 }
    ];

    const objects = [];

    for (const item of objectColors) {
      const mesh = new THREE.Mesh(
        new THREE.BoxGeometry(0.65, 0.65, 0.65),
        new THREE.MeshStandardMaterial({ color: item.color })
      );

      mesh.position.set(item.x, 0.33, item.z);
      scene.add(mesh);

      objects.push({
        ...item,
        mesh,
        taken: false
      });
    }

    let target = null;
    let armGoal = 0;
    let actionAfterArrival = "";

    function setStatus(message) {
      statusElement.textContent = `Status: ${message}`;
    }

    function detectObjects() {
      const forward = new THREE.Vector3(0, 0, -1);
      forward.applyQuaternion(robot.quaternion).normalize();

      const visibleObjects = objects.filter(item => {
        if (item.taken) {
          return false;
        }

        const relativePosition = item.mesh.position
          .clone()
          .sub(robot.position);

        relativePosition.y = 0;

        const distance = relativePosition.length();

        if (distance > 8) {
          return false;
        }

        relativePosition.normalize();
        return forward.dot(relativePosition) > 0.35;
      });

      if (visibleObjects.length > 0) {
        const names = visibleObjects
          .map(item => `kotak ${item.name}`)
          .join(", ");

        detectionElement.textContent =
          `Kamera mendeteksi: ${names}.`;
      } else {
        detectionElement.textContent =
          "Kamera tidak menemukan objek di area pandang.";
      }

      return visibleObjects;
    }

    function findObject(command) {
      const visibleObjects = detectObjects();

      const selectedColor = objectColors.find(item =>
        command.includes(item.name)
      );

      if (selectedColor) {
        return visibleObjects.find(item =>
          item.name === selectedColor.name
        );
      }

      return visibleObjects[0];
    }

    function moveToObject(item, action = "") {
      if (!item) {
        setStatus(
          "objek tidak ditemukan di depan robot. Coba pindai atau putar robot."
        );
        return;
      }

      target = item;
      actionAfterArrival = action;
      setStatus(`bergerak menuju kotak ${item.name}...`);
    }

    function handleCommand(rawCommand) {
      const command = rawCommand.toLowerCase().trim();
      console.log("Perintah diterima:", command);

      if (!command) {
        return;
      }

      commandInput.value = command;

      if (
        command.includes("pindai") ||
        command.includes("deteksi")
      ) {
        const visibleObjects = detectObjects();
        setStatus(
          visibleObjects.length > 0
            ? `${visibleObjects.length} objek terdeteksi`
            : "tidak ada objek terdeteksi"
        );
        return;
      }

      if (command.includes("cari")) {
        const item = findObject(command);

        if (item) {
          moveToObject(item);
        } else {
          setStatus(
            "objek tidak ditemukan di depan robot."
          );
        }
        return;
      }

      if (command.includes("ambil")) {
        moveToObject(findObject(command), "ambil");
        return;
      }

      if (command.includes("maju")) {
        target = {
          point: robot.position.clone().add(
            new THREE.Vector3(0, 0, -1)
              .applyQuaternion(robot.quaternion)
              .multiplyScalar(3)
          )
        };
        setStatus("bergerak maju.");
        return;
      }

      if (command.includes("mundur")) {
        target = {
          point: robot.position.clone().add(
            new THREE.Vector3(0, 0, 1)
              .applyQuaternion(robot.quaternion)
              .multiplyScalar(3)
          )
        };
        setStatus("bergerak mundur.");
        return;
      }

      if (command.includes("kiri")) {
        robot.rotation.y += Math.PI / 2;
        target = null;
        setStatus("berputar ke kiri.");
        return;
      }

      if (command.includes("kanan")) {
        robot.rotation.y -= Math.PI / 2;
        target = null;
        setStatus("berputar ke kanan.");
        return;
      }

      if (command.includes("berhenti")) {
        target = null;
        armGoal = 0;
        setStatus("berhenti.");
        return;
      }

      setStatus(
        "perintah belum dikenali. Coba: maju, kiri, cari kotak merah, atau ambil kotak biru."
      );
    }

    document.querySelector("#send").addEventListener("click", () => {
      handleCommand(commandInput.value);
    });

    commandInput.addEventListener("keydown", event => {
      if (event.key === "Enter") {
        handleCommand(commandInput.value);
      }
    });

    document.querySelector("#scan").addEventListener("click", () => {
      detectObjects();
      setStatus("pemindaian kamera selesai.");
    });

    document.querySelector("#mic").addEventListener("click", () => {
      const SpeechRecognition =
        window.SpeechRecognition ||
        window.webkitSpeechRecognition;

      if (!SpeechRecognition) {
        setStatus(
          "pengenalan suara tidak tersedia. Coba Chrome atau Edge."
        );
        return;
      }

      const recognition = new SpeechRecognition();
      recognition.lang = "id-ID";
      recognition.interimResults = false;

      recognition.onstart = () => {
        setStatus("mendengarkan...");
      };

      recognition.onresult = event => {
        const spokenText =
          event.results[0][0].transcript;

        handleCommand(spokenText);
      };

      recognition.onerror = () => {
        setStatus(
          "suara tidak dapat dikenali. Coba lagi."
        );
      };

      recognition.start();
    });

    function resizeScene() {
      const width = mount.clientWidth;
      const height = mount.clientHeight;

      camera.aspect = width / height;
      camera.updateProjectionMatrix();
      renderer.setSize(width, height);
    }

    const resizeObserver = new ResizeObserver(resizeScene);
    resizeObserver.observe(mount);
    resizeScene();

    const clock = new THREE.Clock();

    function animate() {
      requestAnimationFrame(animate);

      const delta = clock.getDelta();

      if (target) {
        const destination = target.point
          ? target.point.clone()
          : target.mesh.position.clone()
              .add(new THREE.Vector3(0, 0, 1.2));

        destination.y = 0;

        const differenceX =
          destination.x - robot.position.x;

        const differenceZ =
          destination.z - robot.position.z;

        const distance = Math.hypot(
          differenceX,
          differenceZ
        );

        if (distance > 0.16) {
          const desiredYaw =
            Math.atan2(-differenceX, -differenceZ);

          let angle =
            desiredYaw - robot.rotation.y;

          angle = Math.atan2(
            Math.sin(angle),
            Math.cos(angle)
          );

          robot.rotation.y +=
            angle * Math.min(1, delta * 4);

          if (Math.abs(angle) < 0.45) {
            robot.position.x +=
              -Math.sin(robot.rotation.y) * delta * 1.5;

            robot.position.z +=
              -Math.cos(robot.rotation.y) * delta * 1.5;

            for (const wheel of wheels) {
              wheel.rotation.x += delta * 7;
            }
          }
        } else {
          if (
            actionAfterArrival === "ambil" &&
            target.mesh
          ) {
            target.taken = true;
            target.mesh.visible = false;
            armGoal = -1.2;
            setStatus(
              `kotak ${target.name} berhasil diambil.`
            );
          } else {
            setStatus("sampai di tujuan.");
          }

          target = null;
          actionAfterArrival = "";
        }
      }

      armPivot.rotation.x +=
        (armGoal - armPivot.rotation.x)
        * Math.min(1, delta * 3);

      renderer.render(scene, camera);
    }

    animate();
  </script>
</body>
</html>
