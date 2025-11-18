<template>
  <q-page class="mainpage-container">
    <div id="pixel-container" class="pixel-container"></div>
    <div class="container-wrap">
      <div class="wrapper">
        <div class="title-wrap">
          <div class="sub-text">
            <span ref="subText" class="sub-text-content font_ibm"
              >사용자 중심의 퍼블리셔</span
            >
          </div>
          <div class="focus-keyword">
            <span ref="dynamicWord" class="dynamic-word font_900"
              >User-Centric</span
            >
          </div>
          <span class="font_ibm static-text text-r">Web Publisher.</span>
        </div>
        <div class="main-wrap">
          <div class="folder-list">
            <ul>
              <li>
                <router-link to="">
                  <div class="icon">
                    <svg
                      width="184"
                      height="154"
                      viewBox="0 0 184 154"
                      fill="none"
                      xmlns="http://www.w3.org/2000/svg"
                    >
                      <path
                        d="M82.8457 12.8299L75.9345 4.25572C74.1677 2.07321 71.4656 0.77417 68.5816 0.77417H9.68009C4.53559 0.800128 0.352539 4.87934 0.352539 9.89393V144.221C0.352539 149.236 4.53559 153.315 9.68009 153.315H173.861C179.031 153.315 183.188 149.236 183.188 144.221V25.4052C183.188 20.3906 179.005 16.3115 173.861 16.3115H90.1986C87.3406 16.3114 84.6385 15.0383 82.8457 12.8299Z"
                        fill="#279ACF"
                      />
                      <path
                        d="M20.2042 22.4158H163.364C169.497 22.4158 174.479 27.9239 174.479 34.7053V133.073C174.479 137.828 171.001 141.674 166.7 141.674H16.8672C12.5668 141.674 9.08887 137.828 9.08887 133.073V34.7053C9.08887 27.9239 14.0707 22.4158 20.2042 22.4158Z"
                        fill="white"
                      />
                      <path
                        d="M12.6753 34.4846H170.958C177.739 34.4846 183.248 39.9928 183.248 46.7742V145.142C183.248 149.897 179.402 153.742 174.648 153.742H8.98584C4.23108 153.742 0.385742 149.897 0.385742 145.142V46.7742C0.385742 39.9928 5.89392 34.4846 12.6753 34.4846Z"
                        fill="#5FC9F9"
                      />
                    </svg>
                  </div>
                  <span class="font_ibm font_500">ABOUT</span>
                </router-link>
              </li>
              <li>
                <router-link to="">
                  <div class="icon-wrapper" ref="folderItem">
                    <div class="icon folder-icon">
                      <svg
                        width="184"
                        height="154"
                        viewBox="0 0 184 154"
                        fill="none"
                        xmlns="http://www.w3.org/2000/svg"
                      >
                        <path
                          d="M82.8457 12.8299L75.9345 4.25572C74.1677 2.07321 71.4656 0.77417 68.5816 0.77417H9.68009C4.53559 0.800128 0.352539 4.87934 0.352539 9.89393V144.221C0.352539 149.236 4.53559 153.315 9.68009 153.315H173.861C179.031 153.315 183.188 149.236 183.188 144.221V25.4052C183.188 20.3906 179.005 16.3115 173.861 16.3115H90.1986C87.3406 16.3114 84.6385 15.0383 82.8457 12.8299Z"
                          fill="#279ACF"
                        />
                        <path
                          class="folder-center"
                          d="M20.2042 22.4158H163.364C169.497 22.4158 174.479 27.9239 174.479 34.7053V133.073C174.479 137.828 171.001 141.674 166.7 141.674H16.8672C12.5668 141.674 9.08887 137.828 9.08887 133.073V34.7053C9.08887 27.9239 14.0707 22.4158 20.2042 22.4158Z"
                          fill="white"
                        />
                        <path
                          class="folder-front"
                          ref="folderFront"
                          d="M12.6753 34.4846H170.958C177.739 34.4846 183.248 39.9928 183.248 46.7742V145.142C183.248 149.897 179.402 153.742 174.648 153.742H8.98584C4.23108 153.742 0.385742 149.897 0.385742 145.142V46.7742C0.385742 39.9928 5.89392 34.4846 12.6753 34.4846Z"
                          fill="#5FC9F9"
                        />
                      </svg>
                    </div>

                    <div class="drop-icons">
                      <div class="drop-icon" data-index="0">📄</div>
                      <div class="drop-icon" data-index="1">💻</div>
                      <div class="drop-icon" data-index="2">✨</div>
                    </div>
                  </div>
                  <span class="font_ibm font_500">WORK</span>
                </router-link>
              </li>
            </ul>
          </div>
        </div>
        <div class="dock-wrap">
          <ul>
            <li>
              <button>
                <div class="icon">
                  <img
                    src="../assets/imgs/index/terminal.png"
                    alt="terminal icon"
                  />
                </div>
              </button>
            </li>
            <li>
              <button>
                <div class="icon">
                  <img
                    src="../assets/imgs/index/terminal.png"
                    alt="terminal icon"
                  />
                </div>
              </button>
            </li>
            <li>
              <button>
                <div class="icon">
                  <img
                    src="../assets/imgs/index/terminal.png"
                    alt="terminal icon"
                  />
                </div>
              </button>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, onMounted, onBeforeUnmount, ref } from "vue";
import gsap from "gsap"; // GSAP import 추가
import pixelEffect from "src/assets/js/stacking-pixels.js";

export default defineComponent({
  name: "IndexPage",

  // [수정] setup() 함수에 ref를 정의하고 반환하여 템플릿의 ref와 연결
  setup() {
    // 템플릿에서 ref="folderItem"과 ref="folderFront"를 연결
    const folderItem = ref(null);
    const folderFront = ref(null);

    // GSAP Timeline 객체를 저장할 변수
    const folderTimeline = ref(null);

    return { folderItem, folderFront, folderTimeline };
  },

  data() {
    return {
      wordPairs: [
        { sub: "강력한 성능의 퍼블리셔", main: "Performant" },
        { sub: "유연한 반응형 퍼블리셔", main: "Responsive" },
        { sub: "구조적인 시맨틱 퍼블리셔", main: "Semantic" },
        { sub: "생동감 넘치는 퍼블리셔", main: "Interactive" },
        { sub: "모두를 위한 퍼블리셔", main: "Accessible" },
      ],
    };
  },

  methods: {
    // [추가] 폴더 열림 및 아이콘 드롭 효과 초기화
    // initFolderHoverEffect() {
    //   const wrapper = this.$refs.folderItem;
    //   const front = this.$refs.folderFront; // SVG path for the front flap
    //   // wrapper 내부에 있는 drop-icons를 찾습니다.
    //   const dropIconsWrapper = wrapper.querySelector(".drop-icons");
    //   if (!dropIconsWrapper) return;
    //   const dropIcons = dropIconsWrapper.querySelectorAll(".drop-icon");
    //   // Timeline 생성 및 상태 설정
    //   const tl = gsap.timeline({
    //     paused: true,
    //     reversed: true,
    //   });
    //   this.folderTimeline = tl; // 타임라인을 ref에 저장
    //   // 드롭 아이콘 초기 상태 설정 (화면에 보이지 않게)
    //   gsap.set(dropIcons, {
    //     opacity: 0,
    //     y: 0, // 초기 y 위치는 0으로 두거나 CSS로 조정
    //     scale: 0.5,
    //   });
    //   // --- 1. 폴더 열림 애니메이션 ---
    //   tl.to(
    //     front,
    //     {
    //       duration: 0.4,
    //       transformOrigin: "50% 100%",
    //       rotationX: 30,
    //       z: -10,
    //       ease: "power2.out",
    //     },
    //     0
    //   );
    //   // --- 2. 아이콘 드롭 애니메이션 (0.2초 후 시작) ---
    //   tl.fromTo(
    //     dropIcons,
    //     {
    //       y: -50, // 위에서 시작
    //       opacity: 0,
    //       scale: 0.5,
    //     },
    //     {
    //       y: 60, // 아래로 떨어지는 최종 위치 (폴더 밖으로 충분히)
    //       opacity: 1,
    //       scale: 1,
    //       stagger: 0.1, // 순차적으로 떨어지게
    //       ease: "bounce.out", // 튕기는 효과
    //       duration: 0.8,
    //     },
    //     0.2
    //   ); // 폴더가 열린 후 0.2초 뒤 드롭 시작
    //   // --- 3. 이벤트 핸들러 등록 ---
    //   const listItem = wrapper.closest("li"); // li 요소 찾기
    //   if (listItem) {
    //     listItem.addEventListener("mouseenter", () => tl.play());
    //     listItem.addEventListener("mouseleave", () => tl.reverse());
    //   }
    // },
  },

  mounted() {
    pixelEffect();
    // [추가] 폴더 호버 효과 초기화
    // ref가 DOM에 마운트된 후 실행
    if (this.$refs.folderItem) {
      this.initFolderHoverEffect();
    }
  },

  beforeUnmount() {
    // [추가] 컴포넌트 해제 시 타임라인 및 이벤트 리스너 정리
    if (this.folderTimeline) {
      this.folderTimeline.kill();
    }
    // 이벤트 리스너는 자동으로 정리되지 않으므로, cleanupInterval과 유사하게 명시적 제거 로직이 필요하지만,
    // 현재는 컴포넌트 자체를 언마운트하는 경우가 흔치 않으므로 생략하고, GSAP 타임라인 kill만 합니다.
  },
});
</script>
