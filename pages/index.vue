<template>
    <div class="bg-gray-800">
        <section class="section">
            <header class="
                flex-1 flex
                items-end justify-center
            ">
                <div>
                    <h1 class="title text-5xl text-blue-400">EthicFlow</h1>
                    <nav>
                        <span
                            class="cursor-pointer mx-3 hover:text-blue-400"
                            @click="goHome"
                        >start</span>
                        <span
                            class="cursor-pointer mx-3 hover:text-blue-400"
                            @click="goAbout"
                        >about</span>
                    </nav>
                </div>
            </header>

            <main class="
                flex-1 flex
                items-center justify-center
            "><div style="min-height: 40vh; display: flex; flex-direction: column;">
                <button
                    v-if="currentPage === 'landing'"
                    ref="camera"
                    @click="clickCta"
                    title="Scan a barcode"
                    class="
                    my-auto
                    cta-button
                    h-64 w-64
                    bg-gray-800 hover:bg-blue-400 
                    rounded-full
                    hover:shadow-2xl
                    transform hover:scale-110
                    border-blue-400 border-solid 
                    outline-none
                    border-2 border-transparent
                    fas fa-camera text-5xl"
                ></button>

                <div
                    v-if="currentPage === 'scanner'"
                    class="intro"
                >
                    <StreamBarcodeReader @decode="scanned"></StreamBarcodeReader>
                </div>

                <div v-if="currentPage === 'manufacturer-entry'"
                    class="flex flex-col flex-1 justify-center items-center"
                >
                    <p class="text-lg">
                        Find a manufacturer:
                    </p>
                    <input
                        type="text"
                        required
                        v-model="manualEntries.manufacturer"
                        class="
                            outline-none h-10 w-72 p-5 m-4 mx-8 
                            focus:ring-4 focus:ring-gray-500 
                            focus:ring-opacity-50 rounded text-center"
                        placeholder="Company Name">
                </div>

                <div v-if="currentPage === 'barcode-entry'"
                    class="flex flex-col flex-1 justify-center items-center"
                >
                    <p class="text-lg">
                        Search by barcode:
                    </p>
                    <input
                        type="text"
                        required
                        v-model="manualEntries.barcode"
                        class="
                            outline-none h-10 w-72 p-5 m-4 mx-8 
                            focus:ring-4 focus:ring-gray-500 
                            focus:ring-opacity-50 rounded text-center"
                        placeholder="Company Name">
                </div>

                <section
                    v-if="currentPage === 'loading'"
                    class="result md:mx-64 mx-5 py-5 text-black"
                >
                    <div class="text-left md:text-center">
                        <img
                            src="@/assets/float.svg"
                            class="max-w-sm float m-auto"
                        />
                        <h1 class="text-4xl font-semibold text-white processing">Searching for Aliases</h1>
                        <h1 class="text-4xl font-semibold text-white processing">Looking for Sources</h1>
                        <h1 class="text-4xl font-semibold text-white processing">Calculating Ratings</h1>
                        <h1 class="text-4xl font-semibold text-gray-500 animate-pulse processing">Finalising</h1>
                        <br><br>
                        <h2
                            class="text-white cursor-pointer underline"
                            @click="processing = false; showBtn = true;"
                        >Back</h2>
                    </div>
                </section>
            </div></main>

            <footer class="
                flex-1 flex
                items-start justify-center
            ">
                <div v-if="currentPage.endsWith('-entry')">
                    <button
                        @click="showResults"
                        class="
                            bg-green-500 focus:ring-4 font-semibold 
                            text-white w-32 py-2 focus:ring-gray-500 
                            focus:ring-opacity-50 rounded"
                    >
                        Submit
                    </button>
                </div>
                <div v-else>
                    <h3 class="
                        text-2xl
                    "><b>Or,</b> input it manually</h3>

                    <span
                        class="text-blue-400 cursor-pointer"
                        @click="currentPage = 'barcode-entry'"
                    >by barcode</span>
                    <br>
                    <span
                        class="text-blue-400 cursor-pointer"
                        @click="currentPage = 'manufacturer-entry'"
                    >by manufacturer</span>
                </div>
            </footer>
        </section>
    </div>
</template>

<script>
import anime from "animejs/lib/anime.es.js";
import { StreamBarcodeReader } from "vue-barcode-reader";
import axios from "axios";

export default {
    data() {
        return {
            currentPage: "landing", // landing, scanner, manufacturer-entry, barcode-entry, results, loading
            scannedBarcode: NaN,
            manualEntries: {
                manufacturer: "",
                barcode: "",
            }
        };
    },

    methods: {
        goHome() {
            location.reload();
        },

        goAbout() {
            this.$router.push("/about");
        },

        async showResults(isManufacturer = false) {
            this.currentPage = "loading";
            let data = {};

            // Manufacturer:
            if (this.manualEntries.manufacturer.length > 0) {
            }

            // Barcode (manual entry):
            else if (
                this.manualEntries.barcode.length > 0 &&
                !isNaN(Number(this.manualEntries.barcode.trim()))
            ) {
                const response = await axios.post(`${process.env.API_URL}/data/by_barcode/`, {
                    number: Number(this.manualEntries.barcode.trim())
                });
            }

            // Barcode (scanned):
            else if (!isNaN(this.scannedBarcode)) {
                const response = await axios.post(`${process.env.API_URL}/data/by_barcode/`, {
                    number: this.scannedBarcode
                });
            }

            /* Fetch 1st API */
            if (isManufacturer) {
                // Skip finding product data, go directly to ratings
                this.processing = true
            }
            else if (!isNaN(this.lookUp)) {
                this.showResult = true;
                //Barcode
                data = await fetch(`https://api.upcitemdb.com/prod/trial/lookup?upc=${this.lookUp}`);

            }
            else {
                this.showResult = true;
                //Title
                data = await fetch(`https://api.upcitemdb.com/prod/trial/lookup?upc=${this.lookUp}`);
            }

            this.product = data;

            this.$nextTick(() => anime({
                targets: document.querySelectorAll('.processing'),
                translateY: [1000, 0],
                easing: "easeInOutSine",
                delay: anime.stagger(2000)
            }))
        },

        scanned(barcodeValue) {
            console.log("Found barcode", barcodeValue);
            this.scannedBarcode = barcodeValue;
            alert(`Scanned barcode ${barcodeValue}`);
            this.showResults();
        },

        clickCta() {
            this.$refs.camera.style = "transition: none;";
            anime({
                targets: this.$refs.camera,
                scale: "0",
                complete: () => {
                    this.currentPage = "scanner";
                    this.$nextTick(() => anime({
                        targets: document.querySelectorAll(".intro"),
                        scale: [0, 1],
                        easing: "easeInOutSine",
                    }))
                }
            });
        }
    },

    components: { StreamBarcodeReader },
};
</script>

<style scoped>
.section {
    min-width: 100%;
    min-height: 100vh;
    text-align: center;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    background: #2d3748;
}

.title {
    font-family: "Montserrat Alternates", sans-serif;
}

.ripple-button {
    animation: ripple 1s linear infinite;
}

.cta-button,
.cta-button::before {
    color: #63b3ed;
    transition: ease-in-out 500ms;
}

.cta-button:hover::before,
.cta-button:focus::before {
    color: white !important;
}

@keyframes ripple {
    from {
        opacity: 1;
        transform: scale3d(0.75, 0.75, 1);
    }
    to {
        opacity: 0;
        transform: scale3d(1.5, 1.5, 1);
    }
}
</style>