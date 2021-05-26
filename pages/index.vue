<template>
  <main>
    <section id="cliff">
      <div class="bg-wrap">
        <div class="bg-fill"></div>
      </div>
      <div class="contents">
        <h1>Let us take care of you,<br />then you can take care of your clients.</h1>
        <div class="button-set">
          <div class="btn large secondary">Find out more</div>
          <div class="btn large">Get in touch</div>
        </div>
      </div>
    </section>
    <section id="how-it-works">
      <div class="preview">
        <div :class="{ fixed: fixed }" class="screen-mock">
          <img src="/images/mac-mock.png" class="mac-mock" />
          <img v-for="(step, index) in steps" :key="index" :data-step-id="index" :src="step.preview" />
        </div>
      </div>
      <div class="steps">
        <div class="step-card" v-for="(step, index) in steps" :key="index" :data-step-id="index">
          <h2>{{ step.title }}</h2>
          <p>{{ step.content }}</p>
        </div>
      </div>
    </section>
    <section id="cta"></section>
    <section id="pricing"></section>
    <section id="faqs"></section>
    <section id="footer"></section>
  </main>
</template>

<script>
export default {
  async asyncData({ $content }) {
    const steps = await $content('steps').fetch()

    return {
      steps,
      fixed: false,
    }
  },
  methods: {
    fixScreen: function () {
      let scrollT = document.documentElement.scrollTop
      let contents = document.querySelector('#cliff > .contents')
      if (scrollT > contents.offsetTop) {
        this.fixed = true
      } else {
        this.fixed = false
      }
    },
  },
  mounted() {
    this.fixScreen()
    document.onscroll = (evt) => {
      this.fixScreen()
    }
  },
}
</script>