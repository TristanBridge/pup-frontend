<template>
  <section :id="'section-' + order" class="article-section">
    <div class="content-container">
      <div class="order">
        <a @click="copyLinkToClipboard" :href="'#section-' + order">{{ order }}</a>
      </div>
      <div class="text-content">
        {{ section.text_text }}
      </div>
      <div class="text-caption">
        {{ section.text_caption }}
      </div>
    </div>
    <div v-if="copied" class="copied-tooltip">Copied!</div>
  </section>
</template>

<script>
import { apiUrl } from "@/assets/api";

export default {
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
      console.log('ho')
      setTimeout(() => {
        this.copied = false;
      }, 2000);
    },
  }
}
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
  padding-right: 1rem; // Add some space between the order number and the content
  a {
    color: inherit;
    text-decoration: none;
    &:hover {
      text-decoration: underline;
    }
  }
}

.text-content,
.text-caption {
  flex: 1;
  width: 50%;
}

.text-caption {
  padding-left: 1rem; // Optional, to add some space between the text and caption
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
  top: 30px;
  z-index: 10;
}
</style>
