<template>
  <section :id="'section-' + order" class="article-section">
    <div class="content-container">
      <div class="order">
        <a @click="copyLinkToClipboard">{{ order }}</a>
        <div v-if="copied" class="copied-tooltip">Copied!</div>
      </div>
      <div class="VisualModule">
        <MainVideo :guplay-id="section.video_guplayId" />
      </div>
    </div>
  </section>
</template>

<script>
import { apiUrl } from "@/assets/api";
import MainVideo from "@/components/article/MainVideo.vue";

export default {
  components: {
    MainVideo, 
  },
  props: ["section", "order"],
  data() {
    return {
      copied: false,
    };
  },
  methods: {
    apiUrl,
    copyLinkToClipboard() {
      this.copied = true;
      const link = `${window.location.origin}${window.location.pathname}#section-${this.order}`;
      navigator.clipboard.writeText(link);
      setTimeout(() => {
        this.copied = false;
      }, 2000);
    },
  },
};
</script>

<style lang="scss" scoped>
.article-section {
  margin: 1.5rem 0;
  scroll-margin-top: 1rem;
}

.content-container {
  display: flex;
}

.order {
  cursor: pointer;
  padding-right: 1rem;
  position: relative;
  a {
    color: inherit;
    text-decoration: none;
    &:hover {
      text-decoration: underline;
    }
  }
}
.copied-tooltip {
  position: absolute;
  background-color: #ffffff;
  border: 1px solid #000000;
  padding: 5px;
  border-radius: 3px;
  font-size: 0.9rem;
  left: 50%;
  transform: translateX(-50%);
  bottom: calc(100% + 5px);
  z-index: 10;
}
</style>
