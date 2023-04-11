<template>
  <div>
    <TextMedia :section="section" :order="order" />
    <TextNotes :section="section" :order="order" />
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
};
</script>
