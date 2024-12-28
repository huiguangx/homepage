<template>
  <div ref="refContainer"></div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount, onUpdated, watch } from "vue";
import * as THREE from "three";
import { TWEEN } from "three/examples/jsm/libs/tween.module.min.js";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import { loadGLTFModel } from "../lib/model";

const GLBs = [
  { name: "EXT", path: "/data/lynkco09/model/Lynkco09_EXT_d.glb" },
  { name: "INT", path: "/data/lynkco09/model/Lynkco09_INT_d.glb" },
  { name: "Sunproof", path: "/data/lynkco09/model/Lynkco09_Sunproof_d.glb" },
  { name: "Trunk", path: "/data/lynkco09/model/Lynkco09_Trunk_d.glb" },
  { name: "Tires", path: "/data/lynkco09/model/Lynkco09_Tires_d.glb" },
  { name: "LBDoor", path: "/data/lynkco09/model/Lynkco09_LBDoor_d.glb" },
  { name: "LFDoor", path: "/data/lynkco09/model/Lynkco09_LFDoor_d.glb" },
  { name: "RFDoor", path: "/data/lynkco09/model/Lynkco09_RFDoor_d.glb" },
  { name: "RBDoor", path: "/data/lynkco09/model/Lynkco09_RBDoor_d.glb" },
];

let controls = null;
let camera = null;
let scene = null;
let models = [];
let tweenCollection = {
  LBDoor: { tween: null, from: { value: null }, to: { value: null } },
  RBDoor: { tween: null, from: { value: null }, to: { value: null } },
  LFDoor: { tween: null, from: { value: null }, to: { value: null } },
  RFDoor: { tween: null, from: { value: null }, to: { value: null } },
  Trunk: { tween: null, from: { value: null }, to: { value: null } },
};

const loading = ref(true);
const refContainer = ref(null);
const refRenderer = ref(null);

function easeOutCirc(x) {
  return Math.sqrt(1 - Math.pow(x - 1, 4));
}

const handleWindowResize = () => {
  const container = refContainer.value;
  const renderer = refRenderer.value;
  if (container && renderer) {
    const scW = container.clientWidth;
    const scH = container.clientHeight;
    renderer.setSize(scW, scH);
  }
};

const pickupObjects = (event) => {
  const container = refContainer.value;
  if (container) {
    const scW = container.clientWidth;
    const scH = container.clientHeight;
    const offsetLeft = container.offsetLeft;
    const offsetTop = container.offsetTop;
    let mouse = new THREE.Vector2();
    mouse.x = ((event.clientX - offsetLeft) / scW) * 2 - 1;
    mouse.y = -((event.clientY - offsetTop) / scH) * 2 + 1;
    let raycaster = new THREE.Raycaster();
    raycaster.setFromCamera(mouse, camera);
    console.log("mouse:", mouse);

    let intersects = raycaster.intersectObjects(scene.children);
    if (intersects.length > 0) {
      if (
        intersects[0].object.name.includes("Door") ||
        intersects[0].object.name.includes("Trunk")
      ) {
        let doorName = intersects[0].object.name.split("_")[0];
        let door = models.find((item) => item.name === doorName);
        if (door && door.outer && door.status) {
          setupTweenDoor(door, door.status);
        }
      }
      if (intersects[0].object.name.includes("INT")) {
        let INT = models.find((item) => item.name === "INT");
        setupTweenCarIn(INT);
      }
    }
  }
};
// const pickupObjects = (event) => {
//   const container = refContainer.value;
//   if (container) {
//     const scW = container.clientWidth;
//     const scH = container.clientHeight;
//     const offsetLeft = container.offsetLeft;
//     const offsetTop = container.offsetTop;
//     let mouse = new THREE.Vector2();
//     mouse.x = ((event.clientX - offsetLeft) / scW) * 2 - 1;
//     mouse.y = -((event.clientY - offsetTop) / scH) * 2 + 1;
//     let raycaster = new THREE.Raycaster();
//     raycaster.setFromCamera(mouse, camera);

//     let intersects = raycaster.intersectObjects(scene.children);
//     console.log("pickupObjects", intersects);
//     if (intersects.length > 0) {
//       console.log("Clicked object:", intersects[0].object.name);

//       if (
//         intersects[0].object.name.includes("Door") ||
//         intersects[0].object.name.includes("Trunk")
//       ) {
//         let doorName = intersects[0].object.name.split("_")[0];
//         let door = models.find((item) => item.name === doorName);
//         if (door) {
//           console.log("Door found:", door);
//           if (door.outer && door.status) {
//             console.log("Animating door:", door.name, "to", door.status);
//             setupTweenDoor(door, door.status);
//           } else {
//             console.error("Door missing 'outer' or 'status'");
//           }
//         } else {
//           console.error("Door not found in models");
//         }
//       }

//       if (intersects[0].object.name.includes("INT")) {
//         let INT = models.find((item) => item.name === "INT");
//         if (INT) {
//           console.log("Animating car interior:", INT.name);
//           setupTweenCarIn(INT);
//         } else {
//           console.error("Interior model not found");
//         }
//       }
//     }
//   }
// };

const setupTweenDoor = (door, status) => {
  const { from, to } = door.rotateDirection[status];
  if (status == "open") {
    door.status = "close";
  }
  if (status == "close") {
    door.status = "open";
  }

  let lastLocation = null;
  if (tweenCollection[door.name].tween) {
    lastLocation = { value: tweenCollection[door.name].from.value };
    tweenCollection[door.name].tween.stop();
  } else {
    lastLocation = { value: from.value };
  }

  tweenCollection[door.name].tween = new TWEEN.Tween(lastLocation)
    .to(to, 1000)
    .easing(TWEEN.Easing.Cubic.InOut)
    .onUpdate(function (lastLocation) {
      door.outer.rotation[door.rotateDirection.rotateAxis] =
        THREE.MathUtils.degToRad(lastLocation.value);
      tweenCollection[door.name].from.value = lastLocation.value;
    })
    .onComplete(() => {
      tweenCollection[door.name] = {
        tween: null,
        from: { value: null },
        to: { value: null },
      };
    })
    .start();
};

const setupTweenCarIn = (model) => {
  const { x: cx, y: cy, z: cz } = camera.position;
  const { x: tocx, y: tocy, z: tocz } = model.carInCameraPosition;

  setupTweenCamera(
    { cx, cy, cz, ox: 0, oy: 0, oz: 0 },
    { cx: tocx, cy: tocy, cz: tocz, ox: 0, oy: tocy, oz: 0.1 }
  );

  function setupTweenCamera(source, target) {
    const carTween = new TWEEN.Tween(source)
      .to(target, 2000)
      .easing(TWEEN.Easing.Quadratic.Out);
    carTween.onUpdate(function (that) {
      camera.position.set(that.cx, that.cy, that.cz);
      controls.target.set(that.ox, that.oy, that.oz);
    });
    carTween.start();
  }
};

onMounted(() => {
  const container = refContainer.value;
  if (container) {
    const scW = container.clientWidth;
    const scH = container.clientHeight;

    const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(scW, scH);
    renderer.outputEncoding = THREE.sRGBEncoding;
    container.appendChild(renderer.domElement);
    refRenderer.value = renderer;
    scene = new THREE.Scene();

    const target = new THREE.Vector3(-0.5, 0.5, 0);
    const initialCameraPosition = new THREE.Vector3(
      5 * Math.sin(0.2 * Math.PI),
      2.5,
      5 * Math.cos(0.2 * Math.PI)
    );

    camera = new THREE.PerspectiveCamera(75, scW / scH, 0.1, 1000);
    camera.position.copy(initialCameraPosition);
    camera.lookAt(target);

    controls = new OrbitControls(camera, renderer.domElement);
    controls.minDistance = 4; // 最小缩放距离
    controls.maxDistance = 25; // 最大缩放距离
    controls.autoRotate = true;
    controls.target = target;

    const light1 = new THREE.DirectionalLight(0xffffff, 0.2);
    light1.position.set(0, 0, 10);
    scene.add(light1);

    Promise.all(
      GLBs.map((item) =>
        loadGLTFModel(scene, item, refRenderer, {
          receiveShadow: false,
          castShadow: false,
        })
      )
    ).then((res) => {
      models = res;
      // 遍历模型，设置颜色
      models.forEach((model) => {
        model.traverse((child) => {
          if (child.isMesh) {
            if (child.name === "Trunk_24") {
              child.material.color.set(0xffc03f);
            }
            if (child.name.includes("Tires")) {
              child.material.color.set(0x000000);
            }
          }
        });
      });
      animate();
      loading.value = false;
    });

    let req = null;
    let frame = 0;
    const animate = () => {
      req = requestAnimationFrame(animate);
      frame = frame <= 100 ? frame + 1 : frame;
      if (frame <= 100) {
        const p = initialCameraPosition;
        const rotSpeed = -easeOutCirc(frame / 120) * Math.PI * 20;

        camera.position.y = 2.5;
        camera.position.x = p.x * Math.cos(rotSpeed) + p.z * Math.sin(rotSpeed);
        camera.position.z = p.z * Math.cos(rotSpeed) - p.x * Math.sin(rotSpeed);
        camera.lookAt(target);
      } else {
        controls.update();
      }

      TWEEN.update();
      renderer.render(scene, camera);
    };

    onBeforeUnmount(() => {
      cancelAnimationFrame(req);
      renderer.domElement.remove();
      renderer.dispose();
    });
  }
});

onUpdated(() => {
  // Add additional updates if needed
});

watch(
  () => loading.value,
  (newVal) => {
    if (!newVal) {
      console.log("Model loaded");
      // Once the model is loaded, start handling window resizing and clicks
      window.addEventListener("resize", handleWindowResize);
      window.addEventListener("click", pickupObjects);
    }
  }
);
</script>

<style scoped></style>
