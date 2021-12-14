<template>
  <div class="container">
    <base-card>
      <template #content>  
        <div v-if="!!bgUrl" class="bg-img">
          <img :width="size.w" :height="size.h" class="img" :src="bgUrl" ref="bgRef">
          <Element v-for="elem in elements" :key="elem.id" :element="elem" :select="elem.select" />
          <div class="marquee-rec" :style="marqueeStyle" ref="marqueeRef"></div>
        </div>
      </template>
    
    </base-card>
    <div v-if="hasDescrp" class="descrp">
      <button class="close-btn" @click="close()">
        <svg xmlns="http://www.w3.org/2000/svg" width="11.414" height="11.414" viewBox="0 0 11.414 11.414">
          <g id="グループ_51" data-name="グループ 51" transform="translate(-1380.793 -1031.793)">
            <line id="線_17" data-name="線 17" x2="10" y2="10" transform="translate(1381.5 1032.5)" fill="none" stroke="#707070" stroke-linecap="round" stroke-width="1"/>
            <line id="線_18" data-name="線 18" x1="10" y2="10" transform="translate(1381.5 1032.5)" fill="none" stroke="#707070" stroke-linecap="round" stroke-width="1"/>
          </g>
        </svg>
      </button>
      <TheDescription />
    </div>
  </div>
</template>

<script>
import Element from '../element/Element.vue';
import TheDescription from '../layout/TheDescription.vue';

export default {
  components: {
    Element,
    TheDescription
  },
  props: [ 'bg-url', 'size' ],
  data() {
    return {
      hasDescrp: true,
      marqueeParams: {left: 0, top: 0, width: 0, height: 0},
      startCoordinates: {x: 0, y: 0},
      currentCoordinates: {x: 0, y: 0}
    }
  },
  created() {
    document.addEventListener('mousedown', this.handlerBlurSelections);
    document.addEventListener('mousedown', this.handleMarqueeSelection);
    
  },
  computed: {
    marqueeStyle() {

      const x = this.marqueeParams.left;
      const y = this.marqueeParams.top;
      const w = this.marqueeParams.width;
      const h = this.marqueeParams.height;
      
      const result =  {
        left: Number.isInteger(x)? x + 'px': x,
        top: Number.isInteger(y)? y + 'px': y,
        width: Math.abs(w) + 'px',
        height: Math.abs(h) + 'px',
        display: (x === 0 && y === 0) || (w === 0 && h === 0) ? 'none': 'block'
      }
      return result;
    }
  },
  methods: {
    close() {
      this.hasDescrp = false;
    },
    // a click on background image should blur all elements
    handlerBlurSelections(event) {
      if(event.target === this.$refs.bgRef) {
        this.deselectAll();
      }
    },

    handleMarqueeSelection(event) {
      if(event.target === this.$refs.bgRef) {
        event.preventDefault();
        document.addEventListener('mousemove', this.handleMouseMove);
        document.addEventListener('mouseup', this.handleMouseUp);
        // start drawing marquee rectangle
        this.setMarqueeStart(event.offsetX, event.offsetY);
      }
    },
    handleMouseMove(event) {
      let w = 0;
      let h = 0;


      //TODO: prevent inccorect marquee when mouse is covering element
      if (event.target !== this.$refs.bgRef) {
        return;
      }
      
      const x = event.offsetX;
      const y = event.offsetY;
      w = x - this.startCoordinates.x;
      h = y - this.startCoordinates.y;

      this.currentCoordinates.x = x;
      this.currentCoordinates.y = y;
    
      // show rectangle marquee
      this.setMarqueeRec(this.startCoordinates.x, this.startCoordinates.y ,w, h);

      // if element is in the marquee
      this.checkInsideElement();
    },
    handleMouseUp() {
      document.removeEventListener('mousemove', this.handleMouseMove);
      document.removeEventListener('mouseup', this.handleMouseUp);
      this.resetMarqueeRec();
    },
    setMarqueeStart(x, y) {
      this.startCoordinates = {x, y};
      this.setMarqueeRec(x, y , 0, 0);
    },
    resetMarqueeRec() {
      this.setMarqueeRec(0, 0, 0, 0);
    }, 
    setMarqueeRec(x, y, w, h) {
      if (w < 0 && h >= 0) {
        x = x + w;
      } else if ( w < 0 && h < 0) {
        x = x + w;
        y = y + h;
      } else if (w >= 0 && h < 0) {
        y = y + h;
      }
      this.marqueeParams = {
        left: x,
        top: y,
        width: Math.abs(w),
        height: Math.abs(h)
      }
    },
    checkInsideElement() {
      this.elements.filter(elem => !elem.select).forEach(elem => {
        if (this.examineInside(elem.position.left, elem.position.top)) {
          this.toggleSelect(elem.id)
        }
      })
    },
    examineInside(x, y) {
      const {left, top, width, height} = this.marqueeParams;
      const xInside = left < x && left + width > x;
      const yInside = top < y && top + height > y;
      return xInside && yInside;
    }
  },
  inject: [ 'elements', 'deselectAll', 'toggleSelect']

}

</script>
<style scoped>
  .container {
    padding: 1rem;
    height: 100vh;
    width: 100%;
    position: relative;
  }
  /* card is global class */
  .container .card {
    height: 100%;
  }

  .bg-img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: scroll;
    box-shadow: 0 0 5px rgba(0,0,0,0.5);
  }

  .descrp {
    position: fixed;
    bottom: 10px;
    right: 10px;
    opacity: 0.9;
  }
  .descrp .close-btn {
    position: absolute;
    right: 0;
    top: 0;
    border: none;
    width: 24px;
    height: 24px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    background: #eee;
  }
  .descrp .close-btn:hover {
    box-shadow: 0 3px 6px rgba(0,0,0,0.3);
  }
  .marquee-rec {
    position: absolute;
    left: 100%;
    top: 100%;
    width: 50px;
    height: 50px;
    border: 1px dashed black;
    background: transparent;
  }
</style>