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
                <div style="min-height: 76.666vh;">
                    <!-- ABOUT -->
                    <br>
                    <h1>ABOUT US</h1>
                    <p1 style="padding: 30px">
                        But I must explain to you how all this mistaken idea of denouncing pleasure and praising pain was born and I will give you a complete account of the system, and expound the actual teachings of the great explorer of the truth, the master-builder of human happiness. No one rejects, dislikes, or avoids pleasure itself, because it is pleasure, but because those who do not know how to pursue pleasure rationally encounter consequences that are extremely painful. Nor again is there anyone who loves or pursues or desires to obtain pain of itself, because it is pain, but because occasionally circumstances occur in which toil and pain can procure him some great pleasure. To take a trivial example, which of us ever undertakes laborious physical exercise, except to obtain some advantage from it? But who has any right to find fault with a man who chooses to enjoy a pleasure that has no annoying consequences, or one who avoids a pain that produces no resultant pleasure?
                    </p1>
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