<template>
  <div>
    <TextMedia :section="section" />
    <TextNotes :section="section" />
  </div>
</template>

<script>
import TextMedia from "@/components/content/textMedia.vue";
import TextNotes from "@/components/content/textNotes.vue";


export default {
  components: {
    TextMedia, TextNotes
  },
  props: {
    section: {
      type: Object,
      required: true,
    },
  },
};
</script>
