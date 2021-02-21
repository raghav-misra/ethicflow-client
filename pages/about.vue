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
                        <span class="cursor-pointer mx-3 hover:text-blue-400" @click="goHome">start</span>
                        <span class="cursor-pointer mx-3 hover:text-blue-400">about</span>
                    </nav>
                </div>
            </header>

            <main class="
                flex-2 flex
                items-center justify-center
            ">
                <div style="min-height: 76.666vh;">
                </div>
            </main>
        </section>
    </div>
</template>

<script>
export default {
    methods: {
        goHome() {
            location.href = "/";
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