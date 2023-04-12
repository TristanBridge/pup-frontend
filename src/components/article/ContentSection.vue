<template>
  <div>
    <TextMedia v-if="isTextMedia" :section="section" :order="order" />
    <Gallery v-else-if="isGallery" :section="section" :order="order" />
    <Video v-else-if="isVideo" :section="section" :order="order" />
    <TextNotes v-else :section="section" :order="order" />
  </div>
</template>

<script>
import TextMedia from "@/components/content/textMedia.vue";
import TextNotes from "@/components/content/textNotes.vue";
import Gallery from "@/components/content/imageGallery.vue";
import Video from "@/components/content/videoNotes.vue";


export default {
  components: {
    TextMedia, TextNotes, Gallery, Video
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
  isGallery() {
    const value = this.section.image_gallery_media !== undefined;
    return value; 
  },
   isVideo() {
    const value = this.section.video_guplayId !== undefined;
    return value; 
  },
  },
};
</script>
