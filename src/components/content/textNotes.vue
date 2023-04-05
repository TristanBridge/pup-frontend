<template>
  <section :id="section.label" class="article-section">
  this is a text article {{ section.text_text }}
  </section>
</template>

<script>
import { apiUrl } from "@/assets/api";

export default {
  props: ["section"],
  methods: {
    apiUrl
  }
};
</script>

<style lang="scss" scoped>
.article-section {
  margin: 1.5rem 0;
  scroll-margin-top: 1rem;

  .content-ear {
    position: absolute;
    left: 0%;
    width: 8%;
    text-align: right;
    font-size: 1.25rem;
    font-weight: 100;
  }

  .content-media {
    margin: 0 0 1rem;
    font-size: 0.9rem;
    text-align: right;
    position: relative;
    // font-family: Signika, sans-serif;

    .media-item {
      margin-bottom: 0.5rem;

      .media-visual {
        max-width: 100%;
      }
    }

    .gallery {
      display: flex;
      flex-wrap: wrap;
      margin-right: -15px;
      flex-direction: row-reverse;

      .media-item {
        width: calc(50% - 15px);
        margin: 0 15px 15px 0;

        .media-visual {
          display: block;
        }
      }
    }
  }

  .content-text {
    text-align: justify;

    p:first-child {
      margin-top: 0;
    }
    p:last-child {
      margin-bottom: 0;
    }
  }

  @media screen and (min-width: 1000px) {
    display: flex;

    .content-media {
      width: 47.5%;
      margin-right: 5%;

      &::after {
        position: absolute;
        top: 0.25rem;
        left: 103%;
        content: " ";
        display: block;
        width: 0.75rem;
        height: 0.75rem;
        background-image: url("../../assets/chevrons.png");
        background-size: cover;
        transform: scaleX(-1);
      }
    }

    .content-text {
      width: 47.5%;
    }

    // Every other section is laid out in reverse.
    &:nth-child(2n) {
      flex-direction: row-reverse;

      .content-media {
        margin-left: 5%;
        margin-right: 0;
        text-align: left;

        .gallery {
          flex-direction: row;
        }

        &::after {
          left: auto;
          right: 103%;
          transform: none;
        }
      }
    }
  }

  @media screen and (min-width: 1200px) {
    .content-media {
      width: 60%;
    }
    .content-text {
      width: 35%;
    }
  }
}
</style>
