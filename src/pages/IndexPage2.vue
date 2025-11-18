<template>
  <q-page class="mainpage02-container">
    <div id="pixel-container" class="pixel-container"></div>
    <div class="content-wrap">
      <div class="wrapper">
        <div class="title-wrap">
          <div class="sub-text-wrapper">
            <span ref="subText" class="sub-text">사용자 중심의 퍼블리셔</span>
          </div>
          <div class="focus-keyword">
            <span ref="dynamicWord" class="dynamic-word font_900"
              >Accessible</span
            >
          </div>
          <span class="font_neodgm_pro static-text text-r">Web Publisher.</span>
        </div>
      </div>
    </div>
  </q-page>
</template>

<!-- <script>
import { defineComponent, onMounted } from "vue";
import gsap from "gsap";
import pixelEffect from "src/assets/js/stacking-pixels.js";

export default defineComponent({
  name: "IndexPage",

  data() {
    return {
      words: [
        "Performant",
        "Responsive",
        "Semantic",
        "Interactive",
        "Accessible",
      ],
      currentIndex: 0,
      scrambleChars: "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz",
      intervalTime: 4000,
    };
  },

  methods: {
    animateWordScramble(newWord) {
      const el = this.$refs.dynamicWord;
      if (!el) return; // 요소가 없을 경우 방어 코드

      const oldWord = el.textContent;
      const maxLength = Math.max(oldWord.length, newWord.length);
      const chars = this.scrambleChars;

      let obj = { progress: 0 };

      gsap.fromTo(
        obj,
        { progress: 0 },
        {
          progress: 1,
          duration: 1,
          ease: "power2.inOut",
          onUpdate: () => {
            let displayWord = "";

            for (let i = 0; i < maxLength; i++) {
              const oldChar = oldWord[i] || "";
              const newChar = newWord[i] || "";
              if (Math.random() < obj.progress) {
                displayWord += newChar;
              } else if (Math.random() > 0.5) {
                displayWord += chars[Math.floor(Math.random() * chars.length)];
              } else {
                displayWord += oldChar;
              }
            }

            el.textContent = displayWord;
          },
        }
      );
    },

    startWordCycle() {
      const el = this.$refs.dynamicWord;
      if (!el) return;

      el.textContent = this.words[0];

      setInterval(() => {
        this.currentIndex = (this.currentIndex + 1) % this.words.length;
        const nextWord = this.words[this.currentIndex];
        this.animateWordScramble(nextWord);
      }, this.intervalTime);
    },
  },

  mounted() {
    // DOM 렌더링 완료 후 애니메이션 시작
    this.startWordCycle();

    pixelEffect();
    this.startWordCycle();
  },
});
</script> -->

<!-- <script>
import { defineComponent, onMounted } from "vue";
import gsap from "gsap";
import pixelEffect from "src/assets/js/stacking-pixels.js";

export default defineComponent({
  name: "IndexPage",

  data() {
    return {
      words: [
        "Performant",
        "Responsive",
        "Semantic",
        "Interactive",
        "Accessible",
      ],
      currentIndex: 0,
      // scrambleChars는 타이핑 효과에서 사용되지 않습니다.
      // scrambleChars: "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz",
      intervalTime: 4000,
      wordCycleInterval: null, // setInterval ID 저장을 위한 변수 추가
    };
  },

  methods: {
    animateWordScramble(newWord) {
      const el = this.$refs.dynamicWord;
      if (!el) return;

      const oldWord = el.textContent;

      if (el.gsapTL) el.gsapTL.kill();

      const tl = gsap.timeline({
        defaults: { ease: "none" },
        // 💡 1. 애니메이션 시작 시 'is-typing' 클래스 추가 (CSS 커서 비활성화)
        onStart: () => el.classList.add("is-typing"),
        // 💡 2. 애니메이션 완료 시 'is-typing' 클래스 제거 (CSS 커서 활성화)
        onComplete: () => {
          el.textContent = newWord;
          el.classList.remove("is-typing");
        },
      });
      el.gsapTL = tl;

      // 1. 🛑 Exit / 삭제 효과 (Backspace)
      let deleteObj = { progress: 0 };
      tl.to(deleteObj, {
        progress: 1,
        duration: oldWord.length * 0.05,
        onUpdate: () => {
          const charsToKeep = Math.round(
            oldWord.length * (1 - deleteObj.progress)
          );
          // 💡 JS 커서 제거: 텍스트만 표시
          el.textContent = oldWord.substring(0, charsToKeep);
        },
        onComplete: () => {
          el.textContent = "";
        },
      });

      // 2. 🟩 Enter / 타이핑 효과 (Typewriter)
      let typeObj = { progress: 0 };
      tl.to(
        typeObj,
        {
          progress: 1,
          duration: newWord.length * 0.08,
          delay: 0.1,
          onUpdate: () => {
            const charsToShow = Math.round(newWord.length * typeObj.progress);
            // 💡 JS 커서 제거: 텍스트만 표시
            el.textContent = newWord.substring(0, charsToShow);

            // 주석 처리되었던 JS 커서 로직:
            /*
            if (charsToShow < newWord.length) {
              el.textContent = newWord.substring(0, charsToShow) + "|";
            } else {
              el.textContent = newWord;
            }
            */
          },
        },
        "+=0"
      );
    },

    startWordCycle() {
      const el = this.$refs.dynamicWord;
      if (!el) return;

      // 초기 단어를 배열의 마지막 단어('Accessible')로 설정하여 사이클을 시작하는 것이 더 자연스럽습니다.
      el.textContent = "Accessible";
      this.currentIndex = this.words.length - 1;

      // 기존 setInterval을 중복 실행 방지 및 정리
      if (this.wordCycleInterval) clearInterval(this.wordCycleInterval);

      this.wordCycleInterval = setInterval(() => {
        this.currentIndex = (this.currentIndex + 1) % this.words.length;
        const nextWord = this.words[this.currentIndex];
        this.animateWordScramble(nextWord);
      }, this.intervalTime);
    },

    // 💡 인터벌 정리 메서드
    cleanupInterval() {
      if (this.wordCycleInterval) {
        clearInterval(this.wordCycleInterval);
        this.wordCycleInterval = null;
      }
    },
  },

  mounted() {
    // 폰트 로드 대기 로직 (안정성)
    document.fonts.ready
      .then(() => {
        this.startWordCycle();
      })
      .catch((e) => {
        console.error("Font loading error:", e);
        this.startWordCycle();
      });

    // 픽셀 효과는 그대로 유지
    pixelEffect();

    // ⚠️ 주의: mounted에서 this.startWordCycle()을 두 번 호출할 필요는 없습니다.
  },
  onUpdate: () => {
    const charsToShow = Math.round(newWord.length * typeObj.progress);
    // 💡 타이핑 중일 때는 여전히 JS가 커서를 제어
    el.textContent = newWord.substring(0, charsToShow) + "|";
  },
  onComplete: () => {
    // 💡 최종 단어만 남기고 JS 커서 제거
    el.textContent = newWord;

    // CSS ::after가 이 시점부터 깜빡이는 커서를 표시합니다.
  },
  beforeUnmount() {
    // 💡 컴포넌트 해체 시 인터벌 정리
    this.cleanupInterval();
  },
});
</script> -->

<script>
import { defineComponent, onMounted, onBeforeUnmount } from "vue";
import gsap from "gsap";
import pixelEffect from "src/assets/js/stacking-pixels.js";

export default defineComponent({
  name: "IndexPage",

  data() {
    return {
      // subText와 dynamicWord의 매칭되는 단어들을 쌍으로 관리합니다.
      wordPairs: [
        { sub: "강력한 성능의 퍼블리셔", main: "Performant" },
        { sub: "유연한 반응형 퍼블리셔", main: "Responsive" },
        { sub: "구조적인 시맨틱 퍼블리셔", main: "Semantic" },
        { sub: "생동감 넘치는 퍼블리셔", main: "Interactive" },
        { sub: "모두를 위한 퍼블리셔", main: "Accessible" },
      ],
      currentIndex: 0,
      intervalTime: 4000,
      wordCycleInterval: null,
    };
  },

  methods: {
    animateWordTransition(newSubText, newMainWord) {
      const subEl = this.$refs.subText;
      const mainEl = this.$refs.dynamicWord;
      if (!subEl || !mainEl) return;

      const oldMain = mainEl.textContent;

      if (subEl.gsapTL) subEl.gsapTL.kill();

      const tl = gsap.timeline({
        defaults: { ease: "none" },
        onComplete: () => {
          // 최종 텍스트 설정
          subEl.textContent = newSubText;
          mainEl.textContent = newMainWord;
          // Sub Text 3D 속성 초기화
          gsap.set(subEl, { rotateX: 0, y: 0, opacity: 1 });
        },
      });
      subEl.gsapTL = tl;

      // dynamicWord (Typewriter/Delete 효과) 설정
      const deleteDuration = oldMain.length * 0.04;
      const typeDuration = newMainWord.length * 0.08;

      // subText (3D Vertical Flip 효과) 설정
      const flipDuration = 0.3;
      const textHeight = "1.2em";

      // ----------------------------------------------------
      // A. dynamicWord 타이핑/삭제 효과
      // ----------------------------------------------------

      // 1. dynamicWord 🛑 Exit / 삭제 효과
      let deleteObj = { progress: 0 };
      tl.to(deleteObj, {
        progress: 1,
        duration: deleteDuration,
        onUpdate: () => {
          const charsToKeepMain = Math.round(
            oldMain.length * (1 - deleteObj.progress)
          );
          mainEl.textContent = oldMain.substring(0, charsToKeepMain);
        },
        onComplete: () => {
          mainEl.textContent = "";
        },
      });

      // 2. dynamicWord 🟩 Enter / 타이핑 효과
      let typeObj = { progress: 0 };
      tl.to(
        typeObj,
        {
          progress: 1,
          duration: typeDuration,
          delay: 0.1,
          onUpdate: () => {
            const charsToShowMain = Math.round(
              newMainWord.length * typeObj.progress
            );
            mainEl.textContent = newMainWord.substring(0, charsToShowMain);
          },
        },
        "+=0" // 삭제 효과 끝난 직후 시작
      );

      // ----------------------------------------------------
      // B. subText 3D Vertical Flip 효과 (동기화)
      // ----------------------------------------------------

      // 1. subText 🛑 Exit / 회전하며 사라짐 (dynamicWord 삭제 시작 시점과 동시 시작)
      tl.to(
        subEl,
        {
          duration: flipDuration,
          rotateX: 90,
          y: `-${textHeight}`,
          opacity: 0,
          ease: "power2.inOut",
        },
        0
      ); // <-- 핵심: 타임라인 시작 시점 (0초)에 동시 시작

      // 2. subText 🟩 Enter / 새 텍스트 준비 및 회전하며 등장
      // dynamicWord의 타이핑 시작 시점(deleteDuration + 0.1) 근처에 배치
      tl.set(
        subEl,
        {
          textContent: newSubText,
          rotateX: -90,
          y: textHeight,
          opacity: 0,
        },
        deleteDuration + 0.1
      ).to(
        subEl,
        {
          duration: flipDuration,
          rotateX: 0,
          y: 0,
          opacity: 1,
          ease: "power2.inOut",
        },
        `-=${flipDuration * 0.5}`
      ); // 직전 set이 끝나는 시점에 겹쳐서 시작
    },

    startWordCycle() {
      const initialIndex = this.wordPairs.length - 1;
      this.currentIndex = initialIndex;

      if (this.$refs.subText) {
        this.$refs.subText.textContent = this.wordPairs[initialIndex].sub;
      }
      if (this.$refs.dynamicWord) {
        this.$refs.dynamicWord.textContent = this.wordPairs[initialIndex].main;
      }

      if (this.wordCycleInterval) clearInterval(this.wordCycleInterval);

      this.wordCycleInterval = setInterval(() => {
        this.currentIndex = (this.currentIndex + 1) % this.wordPairs.length;
        const nextPair = this.wordPairs[this.currentIndex];
        this.animateWordTransition(nextPair.sub, nextPair.main);
      }, this.intervalTime);
    },

    cleanupInterval() {
      if (this.wordCycleInterval) {
        clearInterval(this.wordCycleInterval);
        this.wordCycleInterval = null;
      }
    },
  },

  mounted() {
    document.fonts.ready
      .then(() => {
        this.startWordCycle();
      })
      .catch((e) => {
        console.error("Font loading error:", e);
        this.startWordCycle();
      });

    pixelEffect();
  },

  beforeUnmount() {
    this.cleanupInterval();
  },
});
</script>
