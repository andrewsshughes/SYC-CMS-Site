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
        <div :class="{ fixed: fixed, hide: !showPreview, remove: removePreview }" class="screen-mock">
          <img src="/images/mac-mock.png" class="mac-mock" />
          <div class="screen">
            <img
              :class="{ down: index > focusIndex, up: index < focusIndex }"
              v-for="(step, index) in steps"
              :key="index"
              :data-step-id="index"
              :src="step.preview"
            />
          </div>
        </div>
      </div>
      <div class="steps">
        <div
          :class="{ focus: index == focusIndex }"
          class="step-card"
          v-for="(step, index) in steps"
          :key="index"
          :data-step-id="index"
          @click.stop.prevent="stepScroll($event, index)"
        >
          <h2>{{ step.title }}</h2>
          <p>{{ step.content }}</p>
        </div>
      </div>
    </section>
    <section id="pricing" :class="{ displayed: !largePreview }">
      <div class="details">
        <div class="price-table">
          <div class="type-toggle">
            <span :class="{ active: filter == 'onetime' }" @click.stop.prevent="filter = 'onetime'"
              >One-Time payments</span
            >
            <span :class="{ active: filter == 'ongoing' }" @click.stop.prevent="filter = 'ongoing'"
              >Ongoing payments</span
            >
          </div>
          <div class="contents">
            <div class="heading-row">
              <span class="title">{{ filter == 'ongoing' ? 'Ongoing Payments' : 'One-Time Payments' }}</span>
              <span class="cost">Cost</span>
            </div>
            <div class="rows"></div>
          </div>
        </div>
      </div>
      <div class="breakdown"></div>
    </section>
    <section id="faqs"></section>
    <section id="footer"></section>
  </main>
</template>

<script>
export default {
  async asyncData({ $content }) {
    const steps = await $content('steps').fetch()
    const costs = await $content('costs').fetch()

    return {
      steps,
      costs,
      fixed: false,
      focusIndex: null,
      showPreview: true,
      removePreview: false,
      largePreview: true,
      filter: 'onetime',
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
    updateFocusedIndex() {
      let steps = document.querySelectorAll('.step-card')
      let scrollT = document.documentElement.scrollTop
      let screenHeight = document.documentElement.clientHeight
      let target = { low: scrollT + Math.floor(screenHeight * 0.3), high: scrollT + Math.floor(screenHeight * 0.7) }

      let found = false
      steps.forEach((step) => {
        if (step.offsetTop > target.low && step.offsetTop < target.high) {
          this.focusIndex = step.getAttribute('data-step-id')
          found = true
        }
      })
      this.focusIndex == !found ? null : this.focusIndex
    },
    stepScroll(evt, index) {
      let step = document.querySelectorAll('.step-card')[index]
      window.scrollTo({
        top: step.offsetTop - Math.floor(document.documentElement.clientHeight / 2) + Math.floor(step.clientHeight / 2),
        left: 0,
        behavior: 'smooth',
      })
    },
    togglePreview() {
      let scrollT = document.documentElement.scrollTop
      let preview = document.querySelector('#how-it-works > .preview')
      let previewBottom = preview.offsetTop + preview.clientHeight
      let marker = previewBottom - Math.floor(document.documentElement.clientHeight * 0.2)
      if (scrollT > marker) {
        this.showPreview = false
      } else {
        this.showPreview = true
      }
    },
    pricingResize() {
      let scrollT = document.documentElement.scrollTop
      let pricingTop = document.querySelector('#pricing').offsetTop
      if (scrollT >= pricingTop) {
        this.largePreview = false
        this.removePreview = true
      } else {
        this.largePreview = true
        this.removePreview = false
      }
    },
  },
  mounted() {
    this.fixScreen()
    this.updateFocusedIndex()
    this.togglePreview()
    this.pricingResize()
    document.onscroll = (evt) => {
      this.fixScreen()
      this.updateFocusedIndex()
      this.togglePreview()
      this.pricingResize()
    }
  },
}
</script>