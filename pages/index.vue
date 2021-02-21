<template>
   <div class="bg-gray-800 min-w-screen min-h-screen text-center overflow-hidden">
        <h1 class="text-5xl text-white">EthicFlow</h1>
        <section class="mt-16">
          <!-- help text -->
          <section v-if="showCamera" class="intro">
            <h1 class="text-white text-2xl">Place your camera on top of a barcode</h1>
            <details>
              <summary class="text-white outline-none focus:outline-none">Or input it manually</summary>
              <span class="text-blue-400 cursor-pointer underline mx-5" @click="showManual = true; showCamera = false;">By product barcode/name</span>
              <span class="text-blue-400 cursor-pointer underline mx-5" @click="showManufacturer = true; showCamera = false;">By product manufacturer</span>
            </details>
          </section>
          <section v-if="showManual">
            <h1 class="text-white text-2xl">Type in the barcode ID or Product Name</h1>
            <h2 class="text-blue-400 cursor-pointer underline" @click="showManual = false; showCamera = true;">Or Scan It</h2>
            <form @submit.prevent="showResults()">
              <input type="text" required v-model="lookUp" class="outline-none h-10 w-96 p-5 m-4 mx-8 focus:ring-4 focus:ring-gray-500 focus:ring-opacity-50 rounded" placeholder="1234567890"><br>
              <button type="submit" class="bg-green-500 focus:ring-4 font-semibold text-white w-32 py-2 focus:ring-gray-500 focus:ring-opacity-50 rounded">Submit</button>
            </form>
          </section>
          <section v-if="showManufacturer">
            <h1 class="text-white text-2xl">Type in the item's Manufacturer</h1>
            <h2 class="text-blue-400 cursor-pointer underline" @click="showManufacturer = false; showCamera = true;">Or Scan It</h2>
            <form @submit.prevent="showResults(true);">
              <input type="text" required v-model="lookUp" class="outline-none h-10 w-96 p-5 m-4 mx-8 focus:ring-4 focus:ring-gray-500 focus:ring-opacity-50 rounded" placeholder="Company Name"><br>
              <button type="submit" class="bg-green-500 focus:ring-4 font-semibold text-white w-32 py-2 focus:ring-gray-500 focus:ring-opacity-50 rounded">Submit</button>
            </form>
          </section>
          <!-- Result -->
          <section v-if="showResult" class="result bg-white md:mx-64 rounded shadow-md mx-5 py-5 text-black">
            <h1 class="text-2xl font-semibold">One Moment...</h1>
            <div class="bg-gray-500 py-4 my-4 rounded animate-pulse w-96 m-auto"></div>
            <div class="bg-gray-500 py-2 my-4 rounded animate-pulse w-96 m-auto"></div>
            <div class="bg-gray-500 py-2 my-4 rounded animate-pulse w-96 m-auto"></div>
            <div class="bg-gray-500 py-2 my-4 rounded animate-pulse w-96 m-auto"></div>
            
          </section>

          <!-- Processing  -->
           <section v-if="processing" class="result md:mx-64 mx-5 py-5 text-black">
            <div class="text-left md:text-center">
              <img src="~assets/float.svg" class="max-w-sm float m-auto" />
              <h1 class="text-4xl font-semibold text-white processing">Searching for Aliases</h1>
              <h1 class="text-4xl font-semibold text-white processing">Looking for Sources</h1>
              <h1 class="text-4xl font-semibold text-white processing">Calculating Ratings</h1>
              <h1 class="text-4xl font-semibold text-gray-500 animate-pulse processing">Finalising</h1>
              <br><br>
              <h2 class="text-white cursor-pointer underline" @click="processing = false; showBtn = true;">Back</h2>
            </div>
          </section>
          <br>
          
          <!-- Camera -->
          <div class="flex justify-center content-center intro">
            <StreamBarcodeReader @decode="scanned" v-if="showCamera"></StreamBarcodeReader>
          </div>
          <!-- Intro Button -->
          <section v-if="showBtn">
            <button ref="camera" @click="btnPress"
              class="z-10 my-32 hover:bg-blue-200 bg-blue-300 h-64 w-64 rounded-full text-white text-3xl shadow-2xl outline-none focus:outline-none focus:border-gray-100 border-solid border-2 border-transparent">Scan
              <br> Barcode
            </button>
            <br>
            <details class="my-28">
              <summary class="text-white outline-none focus:outline-none">Or input it manually</summary>
              <span class="text-blue-400 cursor-pointer underline mx-5"
                @click="showManual = true; showCamera = false; showBtn=false;">By product barcode/name</span>
              <span class="text-blue-400 cursor-pointer underline mx-5"
                @click="showManufacturer = true; showCamera = false; showBtn=false;">By product manufacturer</span>
            </details>
          </section>


        </section>
        

         
     
        </div>
</template>

<script lang="js">
import Vue from 'vue'
import anime from 'animejs/lib/anime.es.js';
import { StreamBarcodeReader } from "vue-barcode-reader";
export default Vue.extend({
  data(){
    return{
      showCamera:false,
      showManual:false,
      showManufacturer:false,
      showBtn:true,
      showResult:false,
      processing:false,
      lookUp:"",
      product:{}
    }
  },
  components:{ StreamBarcodeReader},
  methods:{
    async showResults(isManufacturer=false){
      this.showCamera = false;
      this.showManual = false;
      this.showManufacturer =false;
      let data = {}

      /*Fetch 1st API */
      if(isManufacturer){
        //Skip finding product data, go directly to ratings
        this.processing = true
      }else if(!isNaN(this.lookUp)){
        this.showResult = true;
        //Barcode
         data = await fetch(`https://api.upcitemdb.com/prod/trial/lookup?upc=${this.lookUp}`)

      }else{
        this.showResult = true;
        //Title
         data = await fetch(`https://api.upcitemdb.com/prod/trial/lookup?upc=${this.lookUp}`)
      }
      this.product = data

      this.$nextTick(()=>{
         anime({
          targets:document.querySelectorAll('.processing'),
          translateY:[1000,0],
          easing:"easeInOutSine",
          delay:anime.stagger(2000)
        })

      })
     
      
      


    },
    scanned(v){
      console.log(v)
      this.lookUp = v
      this.showResults()
    },
    btnPress(){
      const mainButton = this.$refs.camera
      const vm = this
            anime({
                targets: mainButton,
                scale:"0",
                complete(){
                  vm.showBtn = false
                  vm.showCamera = true
                  vm.$nextTick(() => {

                    anime({
                      targets: document.querySelectorAll('.intro'),
                      scale: [0, 1],
                      easing:"easeInOutSine",
                      delay: anime.stagger(1000)
                    })

                  })
                  
                }
            })

    }
  },
  
})
</script>

<style>
.float {
    animation: float 1.5s infinite alternate;
    animation-timing-function: ease-in-out;
    animation-iteration-count: infinite;
}
@keyframes float {
    0% {
        transform: translateY(0);
    }
    100% {
        transform: translateY(-20px);
    }
}
@-webkit-keyframes float {
    0% {
        transform: translateY(0);
    }
    100% {
        transform: translateY(-20px);
    }
}
</style>
