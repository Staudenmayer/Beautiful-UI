<template>
  <v-container class="fill-height" fluid @mousemove="updateMousePos">
    <v-responsive class="fill-height"><!--align-center text-center -->
      <div style="height: 50px;"></div>
      <div class="pointer" style="position: relative; text-align: center; color: white;">
        
        <v-img :src="active.img" class="rounded-xl" @load="mainImgageLoading = false" height="72vh" cover style="opacity: 0.7;">
          <v-skeleton-loader v-if="mainImgageLoading" class="rounded-xl" height="72vh" :loading="true"></v-skeleton-loader>
        </v-img> <!--style="opacity: 0.7;"-->
        <span class="text-h4 font-weight-bold" style="position: absolute; bottom: 15vh; left: 15vw;">{{ active.title }}</span>
        <span class="text-h5 font-weight-regular" style="position: absolute; bottom: 5vh; left: 15vw;">{{ active.subtitle }}</span>
        <span class="text-left font-weight-light hidden-sm-and-down" style="position: absolute; bottom: 5vh; right: 15vw; width: 30vw;">
          {{ active.description }}
        </span>
      </div>
      <div class="d-flex flex-row justify-center" style="position: relative;">
        <v-img v-for="(el, idx) of selector" :ref="`carousel-img-${idx}`"
          :src="el.img"
          class="rounded-xl mt-5 ml-2 mr-2"
          :class="{ 'img-micro': !el.active, 'img-mini': el.active, 'shadow': el.shadow }" height="13vh" @load="el.imgLoading = false"
          @click="click(idx, $event)" @mouseenter="enterImg(idx)" @mouseleave="leaveImg(idx)" cover>
          <v-skeleton-loader v-if="el.imgLoading" class="rounded-xl" type="image" :loading="true"></v-skeleton-loader>
        </v-img>
      </div>
      <div class="d-flex flex-row justify-center" style="position: relative;">
        <v-icon v-for="(el, idx) of selector" class="rounded-xl ml-2 mr-2 pointer"
          :class="{ 'img-btn-micro': !el.active, 'img-btn-mini': el.active, show: showBtn, hidden: !showBtn }"
          @mouseenter="enterIco(idx)" @mouseleave="leaveIco(idx)">{{
            el.active ? 'mdi-pause' : '' }}</v-icon>
      </div>
    </v-responsive>
  </v-container>
</template>

<style>
.pointer {
  cursor: pointer;
}

.img-mini,
.img-micro {
  cursor: pointer;
  transition: width 0.5s ease-in-out;
  transition: max-width 0.5s ease-in-out;
}

.img-mini,
.img-btn-mini {
  width: 15vw !important;
  max-width: 15vw !important;
}

.img-micro,
.img-btn-micro {
  width: 10vw !important;
  max-width: 10vw !important;
}

.img-btn-mini,
.img-btn-micro {
  justify-content: start !important;
  bottom: 1.5em;
  left: 0.5em;
}

.img-btn-mini:hover {
  opacity: 1;
  transition: opacity 0.5s;
}

.shadow {
  box-shadow: inset 0 0 5px rgba(0, 0, 0, .075), 0 0 20px rgba(241, 241, 241, 0.6);
}

.show {
  opacity: 1;
  transition: opacity 0.5s;
}

.hidden {
  opacity: 0;
  transition: opacity 0.5s;
}
</style>

<script lang="ts">
interface carouselItem {
  active: boolean
  shadow: boolean
  img: string
  imgLoading: boolean
  title: string
  subtitle: string
  description: string
}
export default {
  data() {
    return {
      mouseX: 0,
      mouseY: 0,
      mainImgageLoading: true,
      selector: [
        {
        active: true,
        shadow: false,
        img: 'https://s3.motionvfx.com/mvfxpublic/templates/mMovements_DVR_homepage.jpg',
        imgLoading: true,
        title: 'mMovements DVR',
        subtitle: 'Diverse Camera Moves Simulations for DaVinci Resolve',
        description: 'Say goodbye to static shots and hello to dynamic, visually stunning sequences -- with mMovements DVR you\'ll have the power to transform ordinary footage into a breathtaking cinematic experience. Enhance your footage with smooth tracking shots, captivating simulations, and awe-inspiring zooms to elevate your narrative'
        },
        {
        active: false,
        shadow: false,
        img: 'https://s3.motionvfx.com/mvfxpublic/templates/mEssentials_homepage.jpg',
        imgLoading: true,
        title: '',
        subtitle: '',
        description: ''
        },
        {
        active: false,
        shadow: false,
        img: 'https://s3.motionvfx.com/mvfxpublic/templates/mBusiness_homepage6.jpg',
        imgLoading: true,
        title: '',
        subtitle: '',
        description: ''
        },
        {
        active: false,
        shadow: false,
        img: 'https://s3.motionvfx.com/mvfxpublic/templates/mStyleVCR_homepage.jpg',
        imgLoading: true,
        title: '',
        subtitle: '',
        description: ''
        },
        {
        active: false,
        shadow: false,
        img: 'https://s3.motionvfx.com/mvfxpublic/slider_home/Homepage_desktop.jpg',
        imgLoading: true,
        title: '',
        subtitle: '',
        description: ''
        },
      ] as Array<carouselItem>,
      active: {} as carouselItem,
      showBtn: false
    }
  },
  mounted(){
    this.active = this.selector[0];
  },
  methods: {
    click(idx: number, event: MouseEvent) {
      for (let i in this.selector) {
        this.selector[i].active = false;
      }
      this.selector[idx].active = !this.selector[idx].active;
      this.active = this.selector[idx];
      let imgElement = event.target as HTMLElement;
      let boundingBox: DOMRect = imgElement.getBoundingClientRect();
      setTimeout(() => {
        if (
          this.mouseX >= boundingBox.left &&
          this.mouseX <= boundingBox.right &&
          this.mouseY >= boundingBox.top &&
          this.mouseY <= boundingBox.bottom
        ) {
          this.leaveImg(idx);
          this.enterImg(idx);
        }
      }, 500)
    },
    enterImg(idx: number) {
      this.selector[idx].shadow = true;
      if (this.selector[idx].active)
        this.showBtn = true;
    },
    leaveImg(idx: number) {
      this.selector[idx].shadow = false;
      if (this.selector[idx].active)
        this.showBtn = false;
    },
    enterIco(idx: number) {
      this.selector[idx].shadow = true;
    },
    leaveIco(idx: number) {
      this.selector[idx].shadow = false;
    },
    updateMousePos(event: MouseEvent) {
      this.mouseX = event.clientX;
      this.mouseY = event.clientY;
    }
  }
}
</script>