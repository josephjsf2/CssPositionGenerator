<template>
  <div class="container" >

    <div class="card-wrap">
      <button @click="hide()" class="close-btn"><span></span></button>
      <base-card>
        <template v-if="this.$slots.title" #title>
          <slot name="title"></slot>
        </template>
        <template v-if="this.$slots.content" #content>
          <slot name="content"></slot>
        </template>
      </base-card>
    </div>
    
  </div>
</template>

<script>
export default {
  methods: {
    hide() {
      this.$emit('modalHide');
    }
  },
}

</script>

<style scoped>
  .container {
    position: fixed;
    left: 0;
    top: 0;
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 999;
    background: rgba(0,0,0,0.3);
  }
  .container .card-wrap {
    width: 50%;
    position: relative;
  }
  .close-btn {
    position: absolute;
    top: -2rem;
    right: -2rem;
    bottom: 1rem;
    width: 24px;
    height: 24px;
    border-radius: 50%;
    border: none;
    display: flex;
    align-items: center;
    justify-content: center;
    background: #fff;
    cursor: pointer;
    z-index: 2;
  }
  .close-btn:hover {
    box-shadow: 0 3px 3px rgba(0,0,0,0.3);
  }
  .close-btn span {
    display: block;
    width: 12px;
    height: 2px;
    background: transparent
  }
  .close-btn span::before,
  .close-btn span::after {
    content: '';
    width: 12px;
    height: 2px;
    position: absolute;
    top: 50%;
    background: gray;
    display: block;
    transform: rotate(45deg);
  }
  .close-btn span::after {
    transform: rotate(-45deg)
  }
</style>