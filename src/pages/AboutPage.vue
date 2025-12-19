<template>
  <q-page class="aboutpage-container">
    <section class="visual-section">
      <div class="gradient-overlay"></div>
      <canvas id="BlobBg"></canvas>
      <div class="wrapper">
        <div class="contents-wrap">
          <div class="title-wrap">
            <div class="keyworld-wrap">
              <span class="bracket font_ibm font_600">[</span>
              <span class="typing-text font_ibm font_600">{{
                currentEnText
              }}</span>
              <span class="bracket font_ibm font_600">]</span>
            </div>
            <span class="stac-text font_ibm">Publisher</span>
          </div>
          <div class="intro-text">
            <span>안녕하세요! 최적의 디지털 경험을 설계하는 </span>
            <span>1년차 퍼블리셔, 이윤정입니다.</span>
          </div>
          <div class="profile-img">
            <img
              src="../assets/imgs/about/profile_img.png"
              alt="이윤정 프로필이미지"
            />
          </div>
          <div class="scroll-icon">
            <span class="font_ibm">SCROLL</span>
            <div class="icon">
              <img src="../assets/imgs/icon/arrow_down_b9.svg" alt="" />
            </div>
          </div>
        </div>
      </div>
    </section>
    <section class="rolling-section">
      <div class="rolling-wrapper">
        <div class="rolling-content">
          <div class="rolling-item" v-for="n in 10" :key="'name-' + n">
            <span class="font_mon font_300">LEE YUNJEONG</span>
            <div class="circle-icon">
              <div class="dev-icon">
                <svg width="105" height="58" viewBox="0 0 105 58" fill="none">
                  <path
                    d="M77.2891 41.3765L98.6214 29.5825V29.0669L77.2891 17.3374V10.4414L105.002 25.9734V32.676L77.2891 48.208V41.3765Z"
                    fill="white"
                  />
                  <path
                    d="M38.5156 57.8744L60.2346 0H66.4861L44.7671 57.8744H38.5156Z"
                    fill="white"
                  />
                  <path
                    d="M0 32.676V25.9734L27.7127 10.4414V17.2729L6.38036 29.0669V29.5825L27.7127 41.312V48.208L0 32.676Z"
                    fill="white"
                  />
                </svg>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="rolling-wrapper">
        <div class="rolling-content">
          <div class="rolling-item" v-for="n in 10" :key="'job-' + n">
            <span class="font_mon font_300">PUBLISHER</span>
            <div class="rolling-icon">
              <img src="../assets/imgs/about/mac_3d_icon.png" alt="mac" />
            </div>
          </div>
        </div>
      </div>
    </section>
    <section class="intro-section" ref="introSection">
      <div class="pin-wrapper">
        <div class="title-wrap">
          <h2 class="split-text font_400">
            Curiosity about how design is implemented <br />
            on the web led me to start my career as a publisher.
          </h2>
          <p class="split-text font_400">
            디자인이 웹사이트에서 구현되는 과정에 대한 호기심으로 퍼블리셔의
            길을 시작했습니다.
          </p>
        </div>
      </div>
    </section>
    <section class="horizontal-section" ref="horizontalSection">
      <div class="pin-wrapper">
        <div class="section-title">Profile</div>
        <div class=""></div>
      </div>
    </section>
    <!-- <section class="project-section">
      <div class="title-wrap">
        <span class="">SELECTED</span>
        <h2>PROJECT</h2>
        <span class="">2024~2025</span>
      </div>
    </section>
    <section class="project-list-section">
      <div class="wrapper">
        <div id="gsap-container" ref="containerRef"></div>
      </div>
    </section> -->
  </q-page>
</template>

<script>
// ⭐ useMouse는 @vueuse/core에서, 나머지는 vue 자체에서 가져옵니다.
import { useMouse } from "@vueuse/core";
import { shallowRef, ref, nextTick } from "vue";
import * as THREE from "three";
import { gsap } from "gsap";
import { projects } from "src/data/projects";
import { onMounted } from "vue";
import { ScrollTrigger } from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

// ============================================
// Three.js 셰이더 코드 (Vertex 및 Fragment)
// ============================================

const vertexShader = `
    varying vec2 vUv;
    void main() {
      vec3 pos = position.xyz;
      gl_Position = projectionMatrix * modelViewMatrix * vec4(pos, 1.);
      vUv = uv;
    }
  `;

const fragmentShader = `
    uniform float uTime;
    uniform vec2 uResolution;
    uniform vec3 uColor1;
    uniform vec3 uColor2;
    uniform vec3 uColor3;
    uniform vec3 uColor4;
    uniform vec3 uColor5;
    uniform vec3 uColor6;
    uniform float uSpeed;
    uniform float uIntensity;
    uniform sampler2D uTouchTexture;
    uniform float uGrainIntensity;
    uniform float uZoom;
    uniform vec3 uDarkNavy;
    uniform float uGradientSize;
    uniform float uGradientCount;
    uniform float uColor1Weight;
    uniform float uColor2Weight;

    varying vec2 vUv;

    #define PI 3.14159265359

    // Grain function for film grain effect
    float grain(vec2 uv, float time) {
      vec2 grainUv = uv * uResolution * 0.5;
      float grainValue = fract(sin(dot(grainUv + time, vec2(12.9898, 78.233))) * 43758.5453);
      return grainValue * 2.0 - 1.0;
    }

    vec3 getGradientColor(vec2 uv, float time) {
      float gradientRadius = uGradientSize;

      // Multiple animated centers
      vec2 center1 = vec2(0.5 + sin(time * uSpeed * 0.4) * 0.4, 0.5 + cos(time * uSpeed * 0.5) * 0.4);
    vec2 center2 = vec2(0.5 + cos(time * uSpeed * 0.6 + 1.0) * 0.5, 0.5 + sin(time * uSpeed * 0.45 + 0.5) * 0.5);
    vec2 center3 = vec2(0.5 + sin(time * uSpeed * 0.35 + 2.0) * 0.45, 0.5 + cos(time * uSpeed * 0.55 + 1.5) * 0.45);
    vec2 center4 = vec2(0.5 + cos(time * uSpeed * 0.5 + 3.0) * 0.4, 0.5 + sin(time * uSpeed * 0.4 + 2.5) * 0.4);
    vec2 center5 = vec2(0.5 + sin(time * uSpeed * 0.7 + 4.0) * 0.35, 0.5 + cos(time * uSpeed * 0.6 + 3.5) * 0.35);
    vec2 center6 = vec2(0.5 + cos(time * uSpeed * 0.45 + 5.0) * 0.5, 0.5 + sin(time * uSpeed * 0.65 + 4.5) * 0.5);

      float dist1 = length(uv - center1);
      float dist2 = length(uv - center2);
      float dist3 = length(uv - center3);
      float dist4 = length(uv - center4);
      float dist5 = length(uv - center5);
      float dist6 = length(uv - center6);

      float influence1 = 1.0 - smoothstep(0.0, gradientRadius, dist1);
      float influence2 = 1.0 - smoothstep(0.0, gradientRadius, dist2);
      float influence3 = 1.0 - smoothstep(0.0, gradientRadius, dist3);
      float influence4 = 1.0 - smoothstep(0.0, gradientRadius, dist4);
      float influence5 = 1.0 - smoothstep(0.0, gradientRadius, dist5);
      float influence6 = 1.0 - smoothstep(0.0, gradientRadius, dist6);

      float totalInfluence = (influence1 + influence2 + influence3 + influence4 + influence5 + influence6) / uGradientCount;

      // Touch Texture influence
      vec4 touch = texture2D(uTouchTexture, uv);
      float touchIntensity = touch.r * 0.5 + touch.g * 0.5;

      // Color mixing
      vec3 colorA = mix(uColor1, uColor2, sin(uTime * 0.5));
      vec3 colorB = mix(uColor3, uColor4, cos(uTime * 0.4));

      vec3 blendedColor = mix(uDarkNavy, colorA, uColor1Weight * (influence1 + influence3 + influence5));
      blendedColor = mix(blendedColor, colorB, uColor2Weight * (influence2 + influence4 + influence6));

      blendedColor = mix(blendedColor, uDarkNavy, 0.4);
      blendedColor += touchIntensity * 0.5;

      return blendedColor;
    }

    void main() {
      vec2 uv = vUv;

      uv = (uv - 0.5) / uZoom + 0.5;

      vec3 finalColor = getGradientColor(uv, uTime);

      // Add Film Grain
      float noise = grain(gl_FragCoord.xy / uResolution.xy, uTime);
      finalColor += noise * uGrainIntensity;

      gl_FragColor = vec4(finalColor, 1.0);
    }
  `;

const containerRef = ref(null); // #container 역할 (3D 공간)
const viewMode = ref("grid");
const projectsData = ref(projects); // 프로젝트 데이터
const hoveredProjectId = ref(null);

const n = projects.length;

const handleBoxMouseEnter = (e) => {
  gsap.to(e.currentTarget, {
    duration: 0.3,
    rotationX: -14,
    y: "-130%",
    ease: "back.out(6)",
  });
};
const handleBoxMouseLeave = (e) => {
  gsap.to(e.currentTarget, {
    duration: 0.4,
    rotationX: 0,
    y: "-50%",
  });
};

// ... 기존 변수 선언들 (n, projectsData 등)
const handleMouseMove = (e) => {
  // 주신 코드의 로직을 유지하되, 전체적인 회전 중심을 마우스에 따라 제어
  gsap.to(".box", {
    duration: 0.6,
    rotationY: (i) =>
      45 + (i / n) * 206.5 + 90 * (e.clientX / window.innerWidth),
  });
};

const initGSAPProjectList = (container) => {
  if (!container || n === 0) return;

  // 1. 컨테이너 원근감 설정 (주신 코드 수치 800)
  gsap.set(container, { perspective: 800 });

  projectsData.value.forEach((project, i) => {
    const b = document.createElement("div");
    b.classList.add("box");

    // 글자 반전을 해결하기 위해 card-content로 감쌉니다.
    b.innerHTML = `
      <div class="card-content">
        <div class="project-title">${project.name}</div>
        <div class="project-date">${project.date}</div>
      </div>
    `;

    // 2. 호버 이벤트 (주신 로직 그대로: 위로 튀어오르며 회전)
    b.onmouseenter = () => {
      gsap.to(b, {
        duration: 0.3,
        rotationX: -14,
        y: "-130%",
        ease: "back.out(6)",
      });
    };
    b.onmouseleave = () => {
      gsap.to(b, { duration: 0.4, rotationX: 0, y: "-50%" });
    };

    container.appendChild(b);

    // 3. 초기 위치 설정 (주신 수치 그대로)
    gsap.set(b, {
      left: "50%",
      top: "50%",
      x: "-50%",
      y: "-50%",
      z: 600,
      width: 300,
      height: 600,
      borderRadius: 20,
      background: `hsl(${(i / n) * 360}, 100%, 50%)`,
      position: "absolute",
    });

    // 4. 등장 애니메이션 (수정 포인트: transformOrigin)
    gsap.fromTo(
      b,
      {
        scaleY: 0,
        zIndex: i < n / 2 ? -i : i,
        rotationY: 90 + (i / n) * 206.5,
        // ⭐ 핵심: -1060%를 +1060%로 변경 (축이 앞으로 오면서 안으로 굽음)
        // 만약 카드가 너무 겹친다면 이 수치를 1500% 등으로 더 키우세요.
        transformOrigin: "50% 50% 1200%",
      },
      {
        scaleY: 1,
        duration: 1,
        delay: 1 - 0.5 * (i / n),
        ease: "elastic",
      }
    );
  });

  window.addEventListener("mousemove", handleMouseMove);
};
// ============================================
// TouchTexture Class (캔버스에 마우스 궤적을 그림)
// ============================================

class TouchTexture {
  constructor() {
    this.size = 64;
    this.width = this.height = this.size;
    this.maxAge = 64;
    this.radius = 0.16 * this.size;
    this.speed = 1 / this.maxAge;
    this.trail = [];
    this.last = null;
    this.initTexture();
  }

  initTexture() {
    this.canvas = document.createElement("canvas");
    this.canvas.width = this.width;
    this.canvas.height = this.height;
    this.ctx = this.canvas.getContext("2d");
    this.ctx.fillStyle = "black";
    this.ctx.fillRect(0, 0, this.canvas.width, this.canvas.height);
    this.texture = new THREE.Texture(this.canvas);
  }

  update() {
    this.clear();
    let speed = this.speed;
    for (let i = this.trail.length - 1; i >= 0; i--) {
      const point = this.trail[i];
      let f = point.force * speed * (1 - point.age / this.maxAge);
      point.x += point.vx * f;
      point.y += point.vy * f;
      point.age++;
      if (point.age > this.maxAge) {
        this.trail.splice(i, 1);
      } else {
        this.drawPoint(point);
      }
    }
    this.texture.needsUpdate = true;
  }

  clear() {
    this.ctx.fillStyle = "black";
    this.ctx.fillRect(0, 0, this.canvas.width, this.canvas.height);
  }

  addTouch(point) {
    let force = 0;
    let vx = 0;
    let vy = 0;
    const last = this.last;
    if (last) {
      const dx = point.x - last.x;
      const dy = point.y - last.y;
      if (dx === 0 && dy === 0) return;
      const dd = dx * dx + dy * dy;
      let d = Math.sqrt(dd);
      vx = dx / d;
      vy = dy / d;
      force = Math.min(dd * 20000, 2.0);
    }
    this.last = { x: point.x, y: point.y };
    this.trail.push({ x: point.x, y: point.y, age: 0, force, vx, vy });
  }

  drawPoint(point) {
    const pos = {
      x: point.x * this.width,
      y: (1 - point.y) * this.height,
    };

    let intensity = 1;
    if (point.age < this.maxAge * 0.3) {
      intensity = Math.sin((point.age / (this.maxAge * 0.3)) * (Math.PI / 2));
    } else {
      const t = 1 - (point.age - this.maxAge * 0.3) / (this.maxAge * 0.7);
      intensity = -t * (t - 2);
    }
    intensity *= point.force;

    const radius = this.radius;
    let color = `${((point.vx + 1) / 2) * 255}, ${
      ((point.vy + 1) / 2) * 255
    }, ${intensity * 255}`;

    let offset = this.size * 5;
    this.ctx.shadowOffsetX = offset;
    this.ctx.shadowOffsetY = offset;
    this.ctx.shadowBlur = radius * 1;
    this.ctx.shadowColor = `rgba(${color},${0.2 * intensity})`;

    this.ctx.beginPath();
    this.ctx.fillStyle = "rgba(255,0,0,1)";
    this.ctx.arc(pos.x - offset, pos.y - offset, radius, 0, Math.PI * 2);
    this.ctx.fill();
  }
}

// ----------------------------------------------------------------------

export default {
  name: "AboutPage",

  data() {
    return {
      texts: [
        { en: "User-Centric" },
        { en: "Pixel-Perfect" },
        { en: "Semantic" },
        { en: "Responsive" },
        { en: "Accessible" },
        { en: "Optimized" },
        { en: "Interactive" },
      ],
      index: 0,
      currentEnText: "",
      enIndex: 0,
      isDeleting: false,
      typingSpeed: 80,
      deletingSpeed: 70,
      delayAfterTyping: 2000,
      timer: null, // 타이머 추가

      rollingTls: [],
    };
  },

  methods: {
    // === 1. 타이핑 효과 메소드 (Options API) ===
    typeEn() {
      clearTimeout(this.timer);
      const currentIndex = this.index % this.texts.length;
      const currentFullText = this.texts[currentIndex].en;
      if (!this.isDeleting) {
        this.currentEnText = currentFullText.substring(0, this.enIndex + 1);
        this.enIndex++;
        if (this.enIndex >= currentFullText.length) {
          this.isDeleting = true;
          this.timer = setTimeout(this.typeEn, this.delayAfterTyping);
          return;
        }
        this.timer = setTimeout(this.typeEn, this.typingSpeed);
      } else {
        this.currentEnText = currentFullText.substring(0, this.enIndex - 1);
        this.enIndex--;
        if (this.enIndex <= 0) {
          this.isDeleting = false;
          this.index = (this.index + 1) % this.texts.length;
          this.timer = setTimeout(this.typeEn, this.typingSpeed);
          return;
        }
        this.timer = setTimeout(this.typeEn, this.deletingSpeed);
      }
    },
  },

  setup() {
    // ----------------------------------------------------------------------
    // Three.js 상태: shallowRef를 사용하여 반응성 프록시화 방지
    // ----------------------------------------------------------------------
    const touchTexture = shallowRef(null);
    const uniforms = shallowRef(null);
    const renderer = shallowRef(null);
    const scene = shallowRef(null);
    const camera = shallowRef(null);
    const animationFrameId = ref(null);
    const clock = new THREE.Clock(); // 일반 객체

    const { x: mouseX, y: mouseY } = useMouse();

    // === 1. 이벤트 핸들러 ===
    const onResize = () => {
      const width = window.innerWidth;
      const height = window.innerHeight;

      if (camera.value && renderer.value && uniforms.value) {
        camera.value.left = width / -2;
        camera.value.right = width / 2;
        camera.value.top = height / 2;
        camera.value.bottom = height / -2;
        camera.value.updateProjectionMatrix();

        renderer.value.setSize(width, height);
        uniforms.value.uResolution.value.set(width, height);
      }
    };

    const onMouseMove = (event) => {
      if (!touchTexture.value) return;

      const normalizedX = event.clientX / window.innerWidth;
      const normalizedY =
        (window.innerHeight - event.clientY) / window.innerHeight;

      touchTexture.value.addTouch({ x: normalizedX, y: normalizedY });
    };

    // === 2. 렌더링 루프 ===
    const animate = () => {
      const delta = clock.getDelta();
      if (uniforms.value && touchTexture.value) {
        // null 체크 추가
        uniforms.value.uTime.value += delta;
        touchTexture.value.update();
      }
      if (renderer.value && scene.value && camera.value) {
        // null 체크 추가
        renderer.value.render(scene.value, camera.value);
      }
      animationFrameId.value = requestAnimationFrame(animate);
    };

    // === 3. 초기화 ===
    const initThree = () => {
      const canvas = document.getElementById("BlobBg");
      if (!canvas) return;

      const width = window.innerWidth;
      const height = window.innerHeight;

      camera.value = new THREE.OrthographicCamera(
        width / -2,
        width / 2,
        height / 2,
        height / -2,
        1,
        1000
      );
      camera.value.position.z = 1;
      scene.value = new THREE.Scene();

      renderer.value = new THREE.WebGLRenderer({ canvas, antialias: true });
      renderer.value.setSize(width, height);
      renderer.value.setPixelRatio(window.devicePixelRatio);

      touchTexture.value = new TouchTexture();

      // uniforms에 할당
      const newBaseColor = new THREE.Vector3(0.33, 0.53, 0.96);

      const brightBlobColor = new THREE.Vector3(0.5, 0.7, 1.0);

      uniforms.value = {
        uTime: { value: 0 },
        uResolution: { value: new THREE.Vector2(width, height) },

        // ⭐ 1. 블롭의 밝은 색상 (밝은 톤 유지)
        uColor1: { value: brightBlobColor },
        uColor3: { value: brightBlobColor },
        uColor5: { value: brightBlobColor },

        // ⭐ 2. 블롭의 어두운 색상 (배경색과 유사하게 설정)
        uColor2: { value: newBaseColor },
        uColor4: { value: newBaseColor },
        uColor6: { value: newBaseColor },

        uSpeed: { value: 1.2 },
        uIntensity: { value: 1.8 },
        uTouchTexture: { value: touchTexture.value.texture },
        uGrainIntensity: { value: 0.02 },
        uZoom: { value: 1.0 },

        // ⭐ 3. 배경의 기본 어두운 색상 (가장 중요)
        uDarkNavy: { value: newBaseColor }, // #5486F5로 설정

        uGradientSize: { value: 0.5 },
        uGradientCount: { value: 4.0 },
        uColor1Weight: { value: 1.0 },
        uColor2Weight: { value: 1.0 },
      };

      const geometry = new THREE.PlaneGeometry(width, height, 1, 1);
      const material = new THREE.ShaderMaterial({
        uniforms: uniforms.value,
        vertexShader: vertexShader,
        fragmentShader: fragmentShader,
      });

      const mesh = new THREE.Mesh(geometry, material);
      scene.value.add(mesh);

      // 이벤트 리스너 등록
      window.addEventListener("resize", onResize);
      window.addEventListener("mousemove", onMouseMove);

      animate();
    };

    const initRollingText = () => {
      // .rolling-content 클래스를 가진 모든 요소를 선택
      const wrappers = document.querySelectorAll(".rolling-content");
      const tls = [];

      wrappers.forEach((el, idx) => {
        const isSecondLine = idx === 1;
        const width = el.offsetWidth / 2; // 전체 아이템의 절반 너비

        // 첫 번째 줄: 0 -> -width (왼쪽으로 이동)
        // 두 번째 줄: -width -> 0 (오른쪽으로 이동)
        const startX = isSecondLine ? -width : 0;
        const endX = isSecondLine ? 0 : -width;

        // 초기 위치 세팅
        gsap.set(el, { x: startX });

        const tl = gsap.to(el, {
          x: endX,
          duration: 35, // 속도가 너무 빠르면 30으로 늘리세요
          ease: "none",
          repeat: -1,
        });

        ScrollTrigger.create({
          trigger: ".rolling-section",
          start: "top bottom",
          end: "bottom top",
          onUpdate: (self) => {
            const velocity = Math.abs(self.getVelocity() / 150);
            const speed = 1 + velocity;
            gsap.to(tl, { timeScale: speed, duration: 0.3 });
            gsap.to(tl, { timeScale: 1, delay: 0.5, duration: 1 });
          },
        });

        tls.push(tl);
      });
      return tls;
    };

    // 템플릿 및 Options API에서 접근할 모든 상태와 함수를 반환합니다.
    return {
      mouseX,
      mouseY,
      // Options API 라이프사이클 훅에서 호출/정리하기 위해 반환
      initThree,
      onResize,
      onMouseMove,
      animate,

      touchTexture,
      uniforms,
      renderer,
      scene,
      camera,
      animationFrameId,
      clock,

      containerRef,
      initGSAPProjectList,
      handleMouseMove,

      // Options API에서 사용하지 않지만 정리 필요
      handleBoxMouseEnter,
      handleBoxMouseLeave,

      initRollingText,
    };
  },

  // ⭐ Options API 라이프사이클 훅: 타이핑 시작 및 Three.js 초기화/정리
  mounted() {
    // 1. 타이핑 시작 (methods에 정의됨)
    this.typeEn();

    // 2. Three.js 초기화 (setup에서 반환된 함수 호출)
    this.$nextTick(() => {
      if (this.initThree) {
        this.initThree();
      }
      if (this.containerRef && this.initGSAPProjectList) {
        // ref를 통해 DOM 요소 접근
        this.initGSAPProjectList(this.containerRef);
      }

      this.rollingTls = this.initRollingText();
    });

    const initIntroAnimation = () => {
      const targets = document.querySelectorAll(".intro-section .split-text");
      if (targets.length === 0) return;

      targets.forEach((target) => {
        // 1. innerHTML을 사용하여 <br> 태그를 포함한 구조를 가져옵니다.
        const content = target.innerHTML;
        target.innerHTML = ""; // 기존 내용 비우기

        // 2. <br> 태그를 기준으로 먼저 나누거나, 임시 요소를 생성해 처리합니다.
        // 여기서는 가장 확실한 방법인 정규식을 사용하여 태그와 글자를 분리합니다.
        const parts = content.split(/(<br\s*\/?>)/i);

        parts.forEach((part) => {
          if (part.toLowerCase().startsWith("<br")) {
            // <br> 태그라면 그대로 다시 넣어줍니다.
            target.innerHTML += part;
          } else {
            // 일반 텍스트라면 한 글자씩 쪼개서 span을 넣습니다.
            [...part].forEach((char) => {
              const span = document.createElement("span");
              span.classList.add("char");
              span.innerText = char === " " ? "\u00A0" : char;
              span.setAttribute("data-char", char);
              target.appendChild(span);
            });
          }
        });
      });

      // 애니메이션 부분 (기존과 동일)
      const allChars = document.querySelectorAll(".intro-section .char");
      const tl = gsap.timeline({
        scrollTrigger: {
          trigger: ".intro-section",
          start: "top top",
          end: "+=2000",
          pin: true,
          scrub: 1,
        },
      });

      tl.to(allChars, {
        "--fill-width": "100%",
        stagger: 0.1,
        ease: "none",
      });
    };

    initIntroAnimation();
  },

  beforeUnmount() {
    // 타이핑 타이머 정리
    clearTimeout(this.timer);

    // Three.js 정리 (setup에서 반환된 상태에 접근)
    if (this.animationFrameId.value) {
      cancelAnimationFrame(this.animationFrameId.value);
    }
    // setup에서 정의된 이벤트 리스너 함수를 제거
    window.removeEventListener("resize", this.onResize);
    window.removeEventListener("mousemove", this.onMouseMove);

    if (this.renderer.value) {
      this.renderer.value.dispose();
    }
    if (this.touchTexture.value) {
      this.touchTexture.value.texture.dispose();
      this.touchTexture.value.canvas = null;
    }

    window.removeEventListener("mousemove", this.handleMouseMove);

    // 생성된 .box 요소들의 이벤트 리스너 정리 (선택 사항이지만 안전함)
    const boxes = document.querySelectorAll(".box");
    boxes.forEach((b) => {
      b.removeEventListener("mouseenter", this.handleBoxMouseEnter);
      b.removeEventListener("mouseleave", this.handleBoxMouseLeave);
    });

    ScrollTrigger.getAll().forEach((t) => t.kill());
    this.rollingTls.forEach((tl) => tl.kill());
  },
};
</script>
