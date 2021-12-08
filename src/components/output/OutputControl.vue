<template>
  <base-card>
    <template #title>
      <h2>Output</h2>
    </template>
    <template #content>
      <div class="container">
        <base-button @click="show()">Preview</base-button>
        <!-- <base-button :solid="true" :primary="true">Output CSS</base-button> -->
      </div>
    </template>
  </base-card>


  <base-modal  v-if="showModal" v-on:modal-hide="hide()">
    <template #title><h2>Preview</h2></template>
    <template #content>
      <div class="content">
        <textarea class="textarea" id="w3review" name="w3review" rows="4" cols="50" v-model="outputCss" disabled ref="textarea">
        </textarea>
        <button @click="copy()" class="copy-btn"><svg xmlns="http://www.w3.org/2000/svg" width="22" height="30" viewBox="0 0 22 30">
  <g id="グループ_9" data-name="グループ 9" transform="translate(-708 -281)">
    <g id="グループ_8" data-name="グループ 8">
      <g id="長方形_22" data-name="長方形 22" transform="translate(712 281)" fill="#707070" stroke="#707070" stroke-width="1">
        <rect width="18" height="25" rx="7" stroke="none"/>
        <rect x="0.5" y="0.5" width="17" height="24" rx="6.5" fill="none"/>
      </g>
      <g id="長方形_21" data-name="長方形 21" transform="translate(708 286)" fill="#fff" stroke="#707070" stroke-width="1">
        <rect width="18" height="25" rx="7" stroke="none"/>
        <rect x="0.5" y="0.5" width="17" height="24" rx="6.5" fill="none"/>
      </g>
    </g>
    <text id="A" transform="translate(710 305)" fill="#707070" font-size="20" font-family="UDDigiKyokashoNP-R, UD Digi Kyokasho NP-R"><tspan x="0" y="0">A</tspan></text>
  </g>
</svg></button>
      </div>
    </template>
  </base-modal>

</template>

<script>
export default {
  data() {
    return {
      showModal: false,
      cssTemplage: 
`
.$className$ { 
  top: $top$; 
  left: $left$; 
  transform: rotate($deg$deg);
}
`,
      outputCss: ''
    }
  },
  methods: {
    show() {
      this.showModal = true;
      this.outputCss = this.makeCss();
    },
    hide() {
      this.showModal = false;
    },
    makeCss() {
      let result = '';
      console.log(this.elements)
      this.elements.forEach(elem => {
        let temp = '';
        temp = this.cssTemplage.replace('$className$', elem.class.trim().replace(' ', '.')).replace('$top$', elem.position.top).replace('$left$', elem.position.left)
        if (elem.rotation.deg === 0) {
          temp = temp.replace(`transform: rotate($deg$deg);
`, '').replace('  }', "}");
        } else {
          temp = temp.replace('$deg$', elem.rotation.deg);
        }

        // prevent duplicate class
        if (!result.includes(temp)) {
          result = result + temp;
        }
      });
      return result;
    },
    copy() {
      const textContent = this.$refs.textarea.value;
      if (navigator.clipboard) {
        navigator.clipboard.writeText(textContent).then(() => {
          this.showAlert('Copy to clipboard successfully!')
        });
        
      }
    }
  },
  inject: [ 'elements', 'showAlert' ]
}
</script>

<style scoped>
  .container {
    display: flex;
    gap: 0.5rem;
    margin-top: 0.5rem;
  }
  .content {
    position: relative;
  }
  .textarea {
    margin-top: 1rem;
    width: 100%;
    min-height: 300px;
  }
  .copy-btn {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: #fff;
    border: none;
    position: absolute;
    top: 2rem;
    right: 1rem;
    box-shadow: 0 3px 6px rgba(0,0,0,0.1);
    transition: 0.3s;
  }
  .copy-btn:hover {
    box-shadow: 0 3px 6px rgba(0,0,0,0.4);
  }
</style>