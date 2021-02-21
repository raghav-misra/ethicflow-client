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
                        <span class="cursor-pointer mx-3 hover:text-blue-400">about</span>
                    </nav>
                </div>
            </header>

            <main class="
                flex-2 flex
                items-center justify-center
            ">
                <div style="min-height: 76.666vh;padding-right:30px;padding-left:40px;">
                    <!-- ABOUT -->
                    <br>
                    <h1>ABOUT US</h1>
                    <p1>
                        In our modern life, almost everything we buy comes off of a store shelf. In the age of globalization, our products can come anywhere, from a few miles away to to the other side of the world! The products are often also built with items from many other places.<br>
                        <img style="border-radius: 28px; padding-top: 37px; padding-bottom: 37px" src="https://cdn.discordapp.com/attachments/798425786629488660/812730488464605234/Lifewire_Where_Is_The_Iphone_Made_1999503-f2ec579b52e14f88bb4021b0c0a5d954.png" alt="Map of companies and their owners">
                        Every step in the creating of a product has an impact outside of the product. Although these changes are slight, when; you multiply them by the billions, it can have a great impact on the world. Whenever you buy anything, your money supports the practices of the manufacturer making it. But when was the last time you thought about this? Not only, is the data hard to come by, but it can often be inconvenient to find. When buying product you like think about its specs, taste, or price. And NOT about how it was... chances are, most of the things you buy help fuel some of the worlds biggest environment and social problems, from labor abuse to environmental destruction, companies make billions upon billions and still contribute to these issues, because of the demand by consumers. However, we have little impact, until now. <br>
                        Because we can denounce them for the issues they create, as long as we can stop buying their products and supporting more ethical brands, we can change the world. But this isn,t a simple task, many websites show ethics, but they are incomplete, and it takes a lot of time to look up each company. If you wanted to buy Doritos, how would you know to look up their owner, PesiCo's practices instead?<br>
                        We solve these issues, as we provide a globalized way to lookup company data and view their practices. Not only do we show you the ethics of a product, but it as simple as scanning a barcode. No extra research needed! Together we can change the world. Even though we might have a small impact, together, with the simplicity and fun of this application, we can promote ethical brands, and end unethical practices.
                    </p1>
                    <br><br>
                    <p1>Data from <a style="color: rgb(75, 219, 135)" href="https://www.ethical.org.au/3.4.2/">Ethical</a>, <a style="color: rgb(75, 219, 135)" href="https://en.wikipedia.org/">Wiki</a></p1>
                </div>
            </main>
        </section>
    </div>
</template>

<script>
export default {
    methods: {
        goHome() {
            this.$router.push("/");
        }
    }
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