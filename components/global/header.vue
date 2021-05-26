<template>
  <div class="header">
    <div class="animatedBG"></div>
    <div class="container">
      <div class="top-bar">
        <div class="logo" @click.stop.prevent="navTo()">
          <img src="/images/plus-logo.png" />
        </div>
        <nav>
          <span @click.stop.prevent="navTo($event)" data-target="how-it-works">How it works</span>
          <span @click.stop.prevent="navTo($event)" data-target="pricing">Pricing</span>
          <span @click.stop.prevent="navTo($event)" data-target="faqs">FAQs</span>
          <span class="cta">Get in touch</span>
        </nav>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    navTo: function (e) {
      if (e == undefined) {
        window.scrollTo({
          top: 0,
          left: 0,
          behavior: 'smooth',
        })
      } else {
        let target = e.target
        let id = `#${target.getAttribute('data-target')}`
        let targetHeight = document.querySelector(id).offsetTop
        window.scrollTo({
          top: targetHeight,
          left: 0,
          behavior: 'smooth',
        })
      }
    },
  },
}
</script>