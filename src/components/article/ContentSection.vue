<template>
  <div>
    <TextMedia v-if="isTextMedia" :section="section" :order="order" />
    <TextNotes v-else :section="section" :order="order" />
  </div>
</template>

<script>
import TextMedia from "@/components/content/textMedia.vue";
import TextNotes from "@/components/content/textNotes.vue";


export default {
  components: {
    TextMedia, TextNotes,
  },
  props: {
    section: {
      type: Object,
      required: true,
    },
    order: {
      type: Number,
      required: true,
    },
  },
  computed: {
  isTextMedia() {
    const value = this.section.media_media !== undefined;
    return value;
  },
  },
};
</script>
