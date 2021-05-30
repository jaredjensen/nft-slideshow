<template>
  <div class="slideshow">
    <vueper-slides autoplay fixed-height="100%">
      <vueper-slide v-for="(nft, i) in nfts" :key="i">
        <template v-slot:content>
          <div class="slide-body">
            <div class="preview">
              <div v-if="nft.data.img">
                <img class="preview" :src="`https://ipfs.io/ipfs/${nft.data.img}`" />
              </div>
              <div v-else-if="nft.data.video">
                <video class="preview" autoplay muted loop>
                  <source :src="`https://ipfs.io/ipfs/${nft.data.video}`" />
                </video>
              </div>
            </div>
            <div class="info">
              <h1>{{ nft.name }}</h1>
              <label>ID</label>
              <span>{{ nft.asset_id }}</span>
              <label>Mint</label>
              <span>{{ nft.template_mint }}</span>
            </div>
          </div>
        </template>
      </vueper-slide>
    </vueper-slides>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { VueperSlides, VueperSlide } from 'vueperslides';
import 'vueperslides/dist/vueperslides.css';

interface NFT {
  asset_id: string;
  content: string;
  data: {
    img?: string;
    video?: string;
  }
  name: string;
  template_mint: string;
}

export default defineComponent({
  components: { VueperSlides, VueperSlide },
  name: 'Slideshow',
  props: {
    account: String,
  },
  data: () => ({
    error: undefined,
    isLoading: true,
    nfts: [] as NFT[],
  }),
  created() {
    this.getNfts();
  },
  methods: {
    async getNfts() {
      this.error = undefined;
      this.isLoading = true;
      try {
        const url = `https://wax.api.atomicassets.io/atomicmarket/v1/assets?owner=${this.account}&page=1&limit=1000&order=desc&sort=asset_id`;
        const res = await fetch(url);
        if (!res.ok) {
          throw new Error(`Failed to fetch assets, received status ${res.status}`);
        }
        const body = await res.json();
        this.nfts = body.data.map((x: any) => ({ content: '', ...x }));
      } catch (error) {
        this.error = error;
      } finally {
        this.isLoading = false;
      }
    },
  },
});
</script>

<style>
  .slide-body {
    display: flex;
    flex-direction: row;
  }
  .slide-body .info,
  .slide-body .preview {
    padding-top: 50px;
    position: relative;
    top: 0;
    vertical-align: top;
  }
  .slide-body .info {
    color: #fff;
    flex-basis: 60%;
    font-size: 20px;
    padding-left: 5rem;
    text-align: left;
  }
  .slide-body .info h1 {
    margin-top: 0;
    padding-top: 0;
  }
  .slide-body .info label {
    color: #aaa;
    display: block;
    font-size: 0.8rem;
  }
  .slide-body .info span {
    display: block;
    padding-bottom: 2rem;
  }
  .slide-body .preview {
    flex-basis: 40%;
    text-align: right;
  }
  .slide-body .preview img,
  .slide-body .preview video {
    margin-top: 0;
    max-width: 80%;
    padding-top: 0;
  }
</style>
