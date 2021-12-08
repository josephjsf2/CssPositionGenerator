<template>
  <div class="card" :class="{'grow': grow}">
    <button class="collapse-btn" v-if="this.$slots.title" @click="toggleCollpase()" > 
      <span class="collpase" :class="{'closed': closed}"></span>
    </button>
    <slot name="title"></slot>
    <section class="content" v-if="this.$slots.content && !closed">
      <slot name="content"></slot>
    </section>
  </div>
</template>

<script>
  export default {
    props: { 
      grow: {type: Boolean, default: false},   //flex-grow
    },
    data() {
      return {
        closed: false  // must be false or the bg image will not show
      }
    },
    methods: {
      toggleCollpase() {
        this.closed = !this.closed;
      }
    }
  }
</script>

<style>
  .card {
    background: #fff;
    border-radius: 12px;
    padding: 1rem;
    box-shadow: 0px 3px 6px rgba(0,0,0,0.3);
    width:100%;
    min-height: 50px;
    position: relative;
    flex-shrink: 0;
  }
  .card.grow {
    flex-grow: 1;
  }
  .card h2 {
    display: inline-block;
    padding: 0.1rem 0.5rem;
    background: #3E856B;
    border-radius: 4px;
    color: #fff;
    font-size: 1.4rem;
  }
  .content {
    height: 100%;
  }
  .collapse-btn {
    position: absolute;
    top: 0.7rem;
    right: 0.7rem;
    width: 24px;
    height: 24px;
    border-radius: 50%;
    border: none;
    display: flex;
    justify-content: center;
    align-items: center;
    background: transparent;
    cursor: pointer;
    transition: 0.5s;
  }
  .collapse-btn:hover {
    background: rgb(233, 231, 231);
  }
  .collpase {
    display: block;
    width: 10px;
    height: 2px;
    background: slategray;
    transition: 1s;
    position: relative;
  }
  .collpase::before {
    background: transparent;
  }
  .collpase.closed::before {
    content: '';
    position: relative;
    display: block;
    left: 0;
    top: 0;
    width: 100%;
    height: 2px;
    background: slategray;
    transform: rotate(90deg);
  }

</style>