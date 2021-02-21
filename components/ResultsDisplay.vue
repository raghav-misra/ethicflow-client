<template>
    <div v-if="data"
        class="p-3 overflow-y-auto flex-1 text-left"
        style="max-height: calc(100vh - 125px); width: 100%;"
    >
        <h1 class="text-2xl">{{ data.name }}</h1>
        <p>Rating: <b :style="`color: ${ratingColor}`">{{data.rating}}</b></p>
        <hr class="my-1">

        <details
            v-if="data.pros"
            class="my-3"
        >
            <summary><b>Positive Impacts</b></summary>
            <div v-for="(info, i) in data.pros" :key="i">
                <b>{{ info[0] }}</b>
                <small>{{ info[1] }}</small>
            </div>
        </details>

        <details
            v-if="data.cons"
            class="my-3"
        >
            <summary><b>Negative Impacts</b></summary>
            <div v-for="(info, i) in data.cons" class="my-3" :key="i">
                <b>{{ info[0] }}:</b>
                <small>{{ info[1] }}</small>
            </div>
        </details>

        <details
            v-if="data.pros"
            class="my-3"
        >
            <summary><b>General Information</b></summary>
            <div v-for="(info, i) in data.info" :key="i">
                <b>{{ info[0] }}</b>
                <small>{{ info[1] }}</small>
            </div>
        </details>
    </div>
</template>

<script>
export default {
    props: { data: Object },
    computed: {
        ratingColor() {
            switch (this.data.rating) {
                case "A":
                    return "green";
                case "B":
                    return "cornflowerblue";
                case "C":
                    return "yellow";
                case "D":
                    return "orange";
                case "F":
                    return "red";
            }
        }
    }
}
</script>

<style scoped>
</style>