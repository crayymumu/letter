<template>
  <div class="container">
    <div class="content">
      <div class="envelope" @click="openCard()"></div>
      <div class="letter">
        <div class="body">
          <span class="close" @click.stop="closeCard()">x</span>
          <div class="message">fin.</div>
        </div>
      </div>
      <div class="shadow"></div>
    </div>
  </div>
</template>

<script>
import { gsap } from 'gsap'
import { CSSRulePlugin } from 'gsap/dist/CSSRulePlugin'

export default {
  data() {
    return {
      t1: null,
      t2: null
    }
  },
  created() {
    gsap.registerPlugin(CSSRulePlugin)
  },
  mounted() {
    this.t1 = gsap.timeline({ paused: true });
    this.t2 = gsap.timeline({ paused: true });
    let flap = CSSRulePlugin.getRule(".envelope:before");
    this.t1
      .to(flap, {
        duration: 0.5,
        cssRule: {
          rotateX: 180
        }
      })
      .set(flap, {
        cssRule: {
          zIndex: 10
        }
      })
      .to('.letter', {
        duration: 0.9,
        ease: "back.inOut(1.5)",
        translateY: -200,
        cssRule: {
          rotateX: 180
        }
      })
      .set('.letter', {
        zIndex: 40
      })
      .to('.letter', {
        duration: 0.7,
        ease: "back.out(.4)",
        translateY: -5,
        translateZ: 250,
        height: 500,
        top: -150
      });


    this.t2.to('.shadow', {
      delay: 1.4,
      // width: 450,
      // boxShadow: "-75px 150px 10px 5px #eeeef3",
      ease: "back.out(.2)",
      duration: .7
    });
  },
  methods: {
    openCard(e) {
      this.t1.play();
      this.t2.play();
    },

    closeCard(e) {
      this.t1.reverse();
      this.t2.reverse();
    }
  }
}
</script>

<style lang="scss">
:root {
  --base: #5E5690;
  --base-light: #7873A7;
  --base-dark: #555184;
  --shadow: #4F4C6B;
  --letter: #d8d7e5;
}

* {
  border: 0;
  margin: 0;
  box-sizing: border-box;
}

@mixin center-by-all-means {
  display: flex;
  align-items: center;
  justify-content: center;
}

.container {
  min-height: 100vh;
  @include center-by-all-means;
}

.content {
  position: relative;
  perspective: 1200px;
}

.shadow {
  position: absolute;
  width: 200px;
  height: 1px;
  background: transparent;
  border-radius: 30%;
  box-shadow: 50px 100px 10px 5px #eeeef3;
}

.letter {
  position: absolute;
  top: 10px;
  left: 10px;
  width: 280px;
  height: 160px;
  @include center-by-all-means;
  z-index: 15;
  border-radius: 2px;
  background: var(--letter);
  box-shadow: 0 1px 7px -2px var(--base);

  .body {
    padding: 10px;
    position: relative;
    width: 100%;
    height: 100%;
    @include center-by-all-means;
    color: var(--base);

    .close {
      position: absolute;
      right: 10px;
      top:  0;
      font-size: 30px;
      font-family: 'Manjari', sans-serif;
      cursor: pointer;
      -webkit-user-select: none;
      -moz-user-select: none;
      -ms-user-select: none;
      user-select: none;
    }

    .message {
      font-size: 40px;
      font-weight: 900;
      text-align: center;
      font-family: 'Great Vibes', cursive;
    }
  }
}

.envelope {
  position: relative;
  width: 300px;
  height: 180px;
  background: linear-gradient(#cccbd7 0.5px, var(--base-light) 0.5px) ;
  cursor: pointer;
}

.envelope::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 300px;
  border-top: 115px solid #7873A7;
  border-left: 150px solid transparent;
  border-right: 150px solid transparent;
  box-sizing: border-box;
  z-index: 30;
  transform-origin: top;
}

.envelope::after {
  content: '';
  position: absolute;
  top: 0;
  width: 300px;
  height: 180px;
  z-index: 25;
  background:
  //bottom-right
  linear-gradient(30deg, var(--base-dark) 47%, var(--shadow) 50%, var(--base) 50%) 150px 90px/ 150px 90px no-repeat,

  //top-left
  linear-gradient(31deg, var(--base) 49%, var(--shadow) 50%, transparent 50%) 0px 0px/ 152px 90px no-repeat,

  //bottom-left
  linear-gradient(150deg, var(--base) 50%, var(--shadow) 50%, var(--base-dark) 53%) 0px 90px/ 151px 90px no-repeat,

  //top-right
  linear-gradient(148.7deg, transparent 50%, var(--shadow) 50%, var(--base) 51%) 150px 0px/ 150px 90px no-repeat;
}

</style>
