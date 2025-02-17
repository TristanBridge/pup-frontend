<template>
  <section v-if="venue">
    <div class="top">
      <video id="videoBG" poster="/poster.jpg" autoplay muted loop playsinline>
        <source src="/introvideocompressed.mp4" type="video/mp4" />
      </video>

      <div class="container">
        <div class="header">
          <img src="/biccs.png" alt="BICCS" title="Logo" />
          <div class="title">
            {{ venue.title }}
          </div>
          <div class="info-menu">
            <div
              v-for="page in venue.pages"
              :key="page.id"
              class="info-menu-item"
              :class="{ active: tab === page.id }"
              @click="toggleTab(page.id)"
            >
              {{ page.heading }}
            </div>
          </div>
        </div>
      </div>
    </div>

    <TransitionExpand>
      <div v-if="tab" class="info">
        <div class="container">
          <TransitionExpand v-for="page in venue.pages" :key="page.id">
            <div v-if="tab === page.id">
              <!--<h2>{{ page.heading }}</h2>-->
              <div class="menu-item" v-html="parseMarkdown(page.body)" />
            </div>
          </TransitionExpand>
        </div>
      </div>
    </TransitionExpand>

    <div class="main">
      <!-- <div v-if="venue.journals.length" class="journal-menu">
        <div class="container">
         Past editions: 
          <router-link
            v-for="journal in venue.journals"
            :key="journal.id"
            :to="`/${journal.identifier}`"
            class="journal-menu-item"
          >
            {{ journal.identifier }}
          </router-link>
        </div>
      </div>-->

      <div v-if="venue.introduction || venue.files.length" class="intro">
        <div class="container" style="margin-top:30px;">
          <h2>BICCS 2023</h2>
          <div
            class="introduction"
            style=""
            v-html="parseMarkdown(venue.introduction)"
          />
          <Downloads :downloads="venue.files" class="venue-downloads" />
        </div>
      </div>

      <div v-if="venue.images.length" class="gallery">
        <CaptionedImage
          v-for="image in venue.images"
          :key="image.id"
          :src="imageUrl(image.image)"
          :caption="image.caption"
        />
      </div>

      <footer v-if="venue.footer" class="venue-footer">
        <div class="container">
          <h2>{{ venue.footer.heading }}</h2>
          <div v-html="venue.footer.body" />
        </div>
      </footer>
    </div>
  </section>
</template>

<script>
import TransitionExpand from "@/components/TransitionExpand";
import { apiUrl, getImageAtLeast, getVenue } from "@/assets/api";
import { parseMarkdown } from "@/assets/markdown";
import Downloads from "@/components/article/Downloads.vue";
import CaptionedImage from "@/components/CaptionedImage.vue";

export default {
  components: { TransitionExpand, Downloads, CaptionedImage },
  data() {
    return {
      venue: null,
      tab: ""
    };
  },
  created() {
    this.load();
  },
  activated() {
    this.load();
  },
  methods: {
    parseMarkdown,
    async load() {
      this.loadVenue();
      this.$store.commit("setHeader", null);
    },
    async loadVenue() {
      this.venue = await getVenue();
    },
    toggleTab(name) {
      this.tab = this.tab === name ? "" : name;
    },
    gotoJournal(identifier) {
      if (!identifier) {
        throw new Error("Journal has no identifier");
      }
      this.$router.push(`/${identifier}`);
    },
    imageUrl(image) {
      return apiUrl(getImageAtLeast(image, 1000).url);
    }
  },
  metaInfo() {
    return {
      meta: [
        {
          property: "og:title",
          content:
            "Biennial International Conference for the Craft Sciences (BICCS)"
        },
        { property: "og:type", content: "website" },
        {
          property: "og:url",
          content: location.origin + location.pathname
        }
      ]
    };
  }
};
</script>

<style lang="scss" scoped>


.menu-item {
  font-size: 22px;
  //columns:2;
  //:first-child {margin-top: 0;}
  //  column-gap:40px;
  margin-bottom: 2rem;
  text-align: left;
  max-width: 1100px;

  :deep(p) {
    line-height: 1.2;
    padding: 0px;
    margin-top: 0px;
    margin-bottom: 8px;
  }

  :deep(h1) {
    font-size: 30px;
    margin-bottom: 10px;
    margin-top: 30px;
  }

  

  :deep(a) {
    color: lightblue;
  }

  :deep(h3) {
    a {
      color: white;
    }
    a:link {
      background-image: url(/linkbuttonwhite.png);
      background-size: 19px;
      background-repeat: no-repeat;
      padding-left: 30px;
      background-position: 0px 0px;
      color: white;
      cursor: pointer;
    }
    a:hover {
      color: lightblue;
    }
  }
}

.introduction {
  letter-spacing: 1px;
  font-size: 20px;
  font-weight: 300;
  columns: 2;
  column-gap: 40px;
  text-align: left;

  :first-child {
    margin-top: 0;
  }
  margin-bottom: 2rem;
}

.intro {
  :deep(h2) {
    font-family: "Teko", sans-serif;
    font-size: 4rem;
    margin-block: 1rem;
    margin-top:40px;
    margin-bottom:30px;
  }

  :deep(h3) {
    margin-block-end: 0.5em;
    margin-block-start: 0em;
    line-height: 1.2;

    a:link {
      color: black;
      background-image: url(/linkbutton.png);
      background-size: 19px;
      background-repeat: no-repeat;
      padding-left: 30px;
      background-position: 0px 0px;
    }
    a:visited {
      color: black;
    }
    a:hover {
      color: #159cea;
    }
  }
}

@media screen and (max-width: 900px) {
  .introduction {
    font-size: 30px;
    columns: 1;
  }

  .intro {
  :deep(h2) {
  
    font-size: 5rem;
    color:black;
    margin-block: 1rem;
  }
  }

  .menu-item {
    font-size: 32px;

    :deep(h1) {
      font-size: 52px;
      line-height: 1;
      margin-top: 50px;
    }

    :deep(h3) a:link {
      background-image: url(/linkbuttonwhite.png);
      background-size: 34px;
      background-repeat: no-repeat;
      padding-left: 55px;
      background-position: 0px 0px;
    }
  }

  .intro :deep(h3) a:link {
    background-image: url(/linkbutton.png);
    background-size: 28px;
    background-repeat: no-repeat;
    padding-left: 45px;
    background-position: 0px 0px;
  }
}

.top {
  width: 100%;
  background-color: rgba(255, 255, 255, 0.7);
  opacity: 0.99;
  transform: translateZ(0);
  overflow: hidden;
}

#videoBG {
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: cover;
  opacity: 0.2;
  transform: translateZ(0);
  overflow: hidden;
  z-index: -1;
}

.header {
  padding: 50px 0px 20px 0px;

  img {
    display: block;
    width: 200px;
  }
}

.title {
  font-family: "Teko", sans-serif;
  font-weight: 100;
  font-size: 200px;
  line-height: 0.75;
  margin-top: 60px;
  max-width: 1100px;
  margin-left: -0.05em;

  @media screen and (max-width: 1200px) {
    font-size: 18vw;
  }
}



.info-menu {
  margin-top: 20px;
  font-family: "Teko", sans-serif;
  line-height: 0.8;

  font-size: 40px;
  margin-left: -10px;
}

.info-menu-item {
  cursor: pointer;
  display: inline-block;
  margin-right: 20px;
  border-radius: 5px;
  padding: 10px 10px 5px 10px;
  &:hover {
    background-color: rgba(255, 255, 255, 0.5);
  }
  &.active {
    font-weight: 300;
    color: #009eb8;
  }
}

.info {
  background-color: rgba(70, 70, 70, 1);
  box-shadow: inset 0 10px 20px 0px rgba(0, 0, 0, 0.2),
    inset 0 6px 40px 0 rgba(0, 0, 0, 0.19);
  color: white;
  z-index: 1;
  width: 100%;
  font-size: 1.25rem;
  text-align: justify;

   :deep(h1) {

    margin-block: 1rem;
    margin-top:30px;
  margin-bottom:00px;
  }

   :deep(h2) {

    font-size: 2.5rem;

    margin-top:60px;
  margin-bottom:20px;
  }

  :deep(h6) {
    font-family: "Teko", sans-serif;
    font-size: 1.5rem;
    margin-block: 1rem;
    margin-top:30px;
  margin-bottom:0px;
  }

   :deep(p) {
  
  margin-bottom:20px;
  font-size:20px;
  max-width:800px;
  }
}




.journal-menu {
  margin-left: -0.05em;
  padding: 0.5rem 0 1rem;
  border-bottom: thin solid #f4f4f4;
  font-family: "Teko", sans-serif;
  font-weight: 400;
  font-size: 2.5rem;
  line-height: 0.75;

  .journal-menu-item {
    color: #888;
    &:hover {
      color: inherit;
    }
  }
}

.venue-footer {
  padding: 0 0 1rem;
  background-color: rgba(70, 70, 70, 1);
  box-shadow: inset 0 10px 20px 0px rgba(0, 0, 0, 0.2),
    inset 0 6px 40px 0 rgba(0, 0, 0, 0.19);
  color: white;
}


@media screen and (max-width: 600px) {
  
  .container{
      width: 80%;
  }

  
}
</style>
