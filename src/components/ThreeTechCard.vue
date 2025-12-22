<script setup lang="ts">
import {
  onMounted,
  onUnmounted,
  ref,
  type PropType,
  nextTick,
  watch,
} from "vue";
import * as THREE from "three";
import { X } from "lucide-vue-next";
import { useI18n } from "vue-i18n";

const { t } = useI18n();

const props = defineProps({
  title: { type: String, required: true },
  description: { type: String, required: true },
  scenarios: { type: Array as PropType<string[]>, default: () => [] },
  advantages: { type: Array as PropType<string[]>, default: () => [] },
  cases: { type: String, default: "" },
});

const containerRef = ref<HTMLDivElement | null>(null);
const isExpanded = ref(false);
const isDragging = ref(false);
const hasDragged = ref(false);
const previousMousePosition = { x: 0, y: 0 };
const clickStartPos = { x: 0, y: 0 };

let targetRotationY = 0;
let targetRotationX = 0;

const INITIAL_TILT_X = -0.15; // Increased tilt
const INITIAL_TILT_Y = 0.3; // Increased rotation

let scene: THREE.Scene;
let camera: THREE.PerspectiveCamera;
let renderer: THREE.WebGLRenderer;
let card: THREE.Mesh;
let animationId: number;
let materialFront: THREE.MeshStandardMaterial;
let materialBack: THREE.MeshStandardMaterial;

// Helper to create texture from canvas
const createCardTexture = (isBack: boolean) => {
  const canvas = document.createElement("canvas");
  canvas.width = 512;
  canvas.height = 800; // Aspect ratio similar to card
  const ctx = canvas.getContext("2d");
  if (!ctx) return new THREE.CanvasTexture(canvas);

  // Background
  const gradient = ctx.createLinearGradient(0, 0, 512, 800);
  if (isBack) {
    gradient.addColorStop(0, "#0f172a"); // Darker slate
    gradient.addColorStop(1, "#1e293b");
  } else {
    gradient.addColorStop(0, "#1e293b");
    gradient.addColorStop(1, "#0f172a");
  }
  ctx.fillStyle = gradient;
  ctx.fillRect(0, 0, 512, 800);

  // Grid Pattern for Back
  if (isBack) {
    ctx.strokeStyle = "rgba(255, 255, 255, 0.05)";
    ctx.lineWidth = 2;
    const gridSize = 40;
    for (let x = 0; x <= 512; x += gridSize) {
      ctx.beginPath();
      ctx.moveTo(x, 0);
      ctx.lineTo(x, 800);
      ctx.stroke();
    }
    for (let y = 0; y <= 800; y += gridSize) {
      ctx.beginPath();
      ctx.moveTo(0, y);
      ctx.lineTo(512, y);
      ctx.stroke();
    }
  }

  // Border
  ctx.strokeStyle = isBack
    ? "rgba(59, 130, 246, 0.3)"
    : "rgba(255, 255, 255, 0.1)";
  ctx.lineWidth = 20;
  ctx.strokeRect(0, 0, 512, 800);

  // Text Content
  ctx.fillStyle = "#ffffff";
  ctx.textAlign = "center";
  ctx.textBaseline = "middle";

  if (!isBack) {
    // Front
    ctx.font = "bold 60px 'Segoe UI', Arial";
    ctx.shadowColor = "rgba(0, 0, 0, 0.5)";
    ctx.shadowBlur = 10;
    ctx.fillText(props.title, 256, 300);
    ctx.shadowBlur = 0;

    ctx.font = "30px 'Segoe UI', Arial";
    ctx.fillStyle = "#94a3b8";
    wrapText(ctx, props.description, 256, 400, 400, 40);

    ctx.font = "italic 24px 'Segoe UI', Arial";
    ctx.fillStyle = "#3b82f6";
    ctx.fillText(t("card.expand"), 256, 700);
  } else {
    // Back
    ctx.textAlign = "left";
    let y = 80;

    // Header
    ctx.font = "bold 40px 'Segoe UI', Arial";
    ctx.fillStyle = "#3b82f6"; // Blue accent
    ctx.fillText(t("card.specs"), 40, y);

    // Divider
    y += 30;
    ctx.strokeStyle = "#3b82f6";
    ctx.lineWidth = 4;
    ctx.beginPath();
    ctx.moveTo(40, y);
    ctx.lineTo(150, y);
    ctx.stroke();

    y += 50;

    ctx.font = "bold 32px 'Segoe UI', Arial";
    ctx.fillStyle = "#e2e8f0";
    ctx.fillText(t("card.scenarios"), 40, y);
    y += 45;
    ctx.font = "26px 'Segoe UI', Arial";
    ctx.fillStyle = "#94a3b8";
    props.scenarios.forEach((item) => {
      ctx.fillText("• " + item, 50, y);
      y += 35;
    });

    y += 40;
    ctx.font = "bold 32px 'Segoe UI', Arial";
    ctx.fillStyle = "#e2e8f0";
    ctx.fillText(t("card.advantages"), 40, y);
    y += 45;
    ctx.font = "26px 'Segoe UI', Arial";
    ctx.fillStyle = "#94a3b8";
    props.advantages.forEach((item) => {
      ctx.fillText("• " + item, 50, y);
      y += 35;
    });

    y += 40;
    ctx.font = "bold 32px 'Segoe UI', Arial";
    ctx.fillStyle = "#e2e8f0";
    ctx.fillText(t("card.cases"), 40, y);
    y += 45;
    ctx.font = "italic 24px 'Segoe UI', Arial";
    ctx.fillStyle = "#cbd5e1";
    wrapText(ctx, props.cases, 256, y, 440, 32);
  }

  const texture = new THREE.CanvasTexture(canvas);
  texture.colorSpace = THREE.SRGBColorSpace;
  return texture;
};

const wrapText = (
  ctx: CanvasRenderingContext2D,
  text: string,
  x: number,
  y: number,
  maxWidth: number,
  lineHeight: number
) => {
  const words = text.split(" ");
  let line = "";
  const isCenter = ctx.textAlign === "center";

  for (let n = 0; n < words.length; n++) {
    const testLine = line + words[n] + " ";
    const metrics = ctx.measureText(testLine);
    const testWidth = metrics.width;
    if (testWidth > maxWidth && n > 0) {
      ctx.fillText(line, isCenter ? x : x + (isCenter ? 0 : 0), y); // Adjust x if left aligned
      line = words[n] + " ";
      y += lineHeight;
    } else {
      line = testLine;
    }
  }
  ctx.fillText(line, x, y);
};

const updateTextures = () => {
  if (!materialFront || !materialBack) return;

  const frontTexture = createCardTexture(false);
  const backTexture = createCardTexture(true);

  materialFront.map = frontTexture;
  materialBack.map = backTexture;

  materialFront.needsUpdate = true;
  materialBack.needsUpdate = true;
};

// Watch for prop changes or locale changes to update texture
watch(
  () => [
    props.title,
    props.description,
    props.scenarios,
    props.advantages,
    props.cases,
    t("card.expand"),
  ],
  () => {
    updateTextures();
  },
  { deep: true }
);

const initThree = () => {
  if (!containerRef.value) return;

  const width = containerRef.value.clientWidth;
  const height = containerRef.value.clientHeight;

  // Scene
  scene = new THREE.Scene();

  // Camera
  camera = new THREE.PerspectiveCamera(50, width / height, 0.1, 1000);
  camera.position.z = 5;

  // Renderer
  renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true });
  renderer.setSize(width, height);
  renderer.setPixelRatio(window.devicePixelRatio);
  containerRef.value.appendChild(renderer.domElement);

  // Geometry
  const geometry = new THREE.BoxGeometry(2.5, 4, 0.1);

  // Materials
  const frontTexture = createCardTexture(false);
  const backTexture = createCardTexture(true);

  const materialSide = new THREE.MeshStandardMaterial({ color: 0x334155 });
  materialFront = new THREE.MeshStandardMaterial({
    map: frontTexture,
    roughness: 0.4,
    metalness: 0.1,
  });
  materialBack = new THREE.MeshStandardMaterial({
    map: backTexture,
    roughness: 0.4,
    metalness: 0.1,
  });

  // Order: right, left, top, bottom, front, back
  const materials = [
    materialSide,
    materialSide,
    materialSide,
    materialSide,
    materialFront,
    materialBack,
  ];

  card = new THREE.Mesh(geometry, materials);

  // Initial state: Flat
  card.rotation.x = 0;
  card.rotation.y = 0;

  scene.add(card);

  // Lights
  const ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
  scene.add(ambientLight);

  const directionalLight = new THREE.DirectionalLight(0xffffff, 1);
  directionalLight.position.set(5, 5, 5);
  scene.add(directionalLight);

  // Animation Loop
  const animate = () => {
    animationId = requestAnimationFrame(animate);

    if (!isExpanded.value && !isDragging.value) {
      // Smooth rotation to target (hover effect)
      card.rotation.y += (targetRotationY - card.rotation.y) * 0.1;
      card.rotation.x += (targetRotationX - card.rotation.x) * 0.1;

      // Gentle floating
      card.position.y = Math.sin(Date.now() * 0.001) * 0.1;
    }

    renderer.render(scene, camera);
  };

  animate();
};

const onMouseEnter = () => {
  if (!isExpanded.value) {
    targetRotationY = Math.PI; // 180 degrees
    targetRotationX = 0;
  }
};

const onMouseLeave = () => {
  if (!isExpanded.value) {
    targetRotationY = 0;
    targetRotationX = 0;
  }
};

const handleResize = () => {
  if (!containerRef.value) return;
  const width = containerRef.value.clientWidth;
  const height = containerRef.value.clientHeight;
  camera.aspect = width / height;
  camera.updateProjectionMatrix();
  renderer.setSize(width, height);
};

const toggleExpand = async () => {
  if (hasDragged.value) return; // Prevent click if dragged

  isExpanded.value = !isExpanded.value;

  await nextTick();
  handleResize();

  if (isExpanded.value) {
    camera.position.z = 7; // Zoom out a bit for full screen
    document.body.style.overflow = "hidden"; // Prevent scrolling

    // Set tilt when expanded
    card.rotation.set(INITIAL_TILT_X, INITIAL_TILT_Y, 0);
    targetRotationY = INITIAL_TILT_Y;
    targetRotationX = INITIAL_TILT_X;
  } else {
    camera.position.z = 5;
    // Reset to flat when collapsed
    card.rotation.set(0, 0, 0);
    targetRotationY = 0;
    targetRotationX = 0;
    document.body.style.overflow = "";
  }
};

const closeCard = async () => {
  isExpanded.value = false;
  await nextTick();
  handleResize();

  camera.position.z = 5;
  card.rotation.set(0, 0, 0);
  targetRotationY = 0;
  targetRotationX = 0;
  document.body.style.overflow = "";
};

const onMouseDown = (event: MouseEvent) => {
  if (!isExpanded.value) return;
  isDragging.value = true;
  hasDragged.value = false;
  previousMousePosition.x = event.clientX;
  previousMousePosition.y = event.clientY;
  clickStartPos.x = event.clientX;
  clickStartPos.y = event.clientY;
};

const onMouseMove = (event: MouseEvent) => {
  if (!isExpanded.value || !isDragging.value) return;

  const deltaX = event.clientX - previousMousePosition.x;
  const deltaY = event.clientY - previousMousePosition.y;

  // Check if moved significantly to consider it a drag
  if (
    Math.abs(event.clientX - clickStartPos.x) > 5 ||
    Math.abs(event.clientY - clickStartPos.y) > 5
  ) {
    hasDragged.value = true;
  }

  card.rotation.y += deltaX * 0.01;
  card.rotation.x += deltaY * 0.01;

  previousMousePosition.x = event.clientX;
  previousMousePosition.y = event.clientY;
};

const onMouseUp = () => {
  isDragging.value = false;
};

onMounted(async () => {
  await nextTick();
  initThree();
  window.addEventListener("resize", handleResize);
  window.addEventListener("mouseup", onMouseUp);
});

onUnmounted(() => {
  cancelAnimationFrame(animationId);
  window.removeEventListener("resize", handleResize);
  window.removeEventListener("mouseup", onMouseUp);
  if (renderer) {
    renderer.dispose();
  }
});
</script>

<template>
  <div class="card-placeholder" :style="{ height: '450px' }">
    <Teleport to="body" :disabled="!isExpanded">
      <div
        :class="['card-wrapper', { expanded: isExpanded }]"
        @click="toggleExpand"
        :style="isExpanded ? {} : { position: 'relative', height: '100%' }"
      >
        <div
          ref="containerRef"
          class="three-card-container"
          @mouseenter="onMouseEnter"
          @mouseleave="onMouseLeave"
          @mousedown.stop="onMouseDown"
          @mousemove.stop="onMouseMove"
        ></div>

        <button v-if="isExpanded" class="close-btn" @click.stop="closeCard">
          <X :size="32" />
        </button>

        <div v-if="isExpanded" class="instructions">{{ t("card.drag") }}</div>
      </div>
    </Teleport>
  </div>
</template>

<style scoped>
.card-placeholder {
  width: 100%;
  position: relative;
  overflow: hidden; /* Prevent layout blowout during transition */
}

.card-wrapper {
  width: 100%;
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
  z-index: 1;
}

.card-wrapper.expanded {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: 9999;
  background: rgba(15, 23, 42, 0.6); /* Semi-transparent base */
  backdrop-filter: blur(20px); /* Heavy blur for focus */
  -webkit-backdrop-filter: blur(20px);
  display: flex;
  align-items: center;
  justify-content: center;
}

.three-card-container {
  width: 100%;
  height: 100%;
  cursor: pointer;
}

.expanded .three-card-container {
  cursor: grab;
  width: 100%;
  height: 100%;
}

.expanded .three-card-container:active {
  cursor: grabbing;
}

.close-btn {
  position: absolute;
  top: 2rem;
  right: 2rem;
  background: rgba(255, 255, 255, 0.1);
  border: none;
  border-radius: 50%;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  cursor: pointer;
  transition: background 0.3s ease;
  z-index: 10000;
}

.close-btn:hover {
  background: rgba(255, 255, 255, 0.2);
}

.instructions {
  position: absolute;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%);
  color: rgba(255, 255, 255, 0.6);
  font-size: 1rem;
  pointer-events: none;
  z-index: 10000;
}
</style>
