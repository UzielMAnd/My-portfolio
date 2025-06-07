<script setup>
import { computed } from 'vue';
import { useI18n } from 'vue-i18n';

const { locale, messages } = useI18n()
const habilidades = computed(() => messages.value[locale.value].skills.skillsArray)

function sliceArray(items) {
    const half = Math.ceil(items.length / 2)
    return [items.slice(0, half), items.slice(half)]
}
</script>

<template>
    <article class="container carousel-container" id="skills">
        <hr class="section-div">
        <h2>{{ messages[locale].skills.title }}</h2>
        <div id="skillCarousel" class="carousel slide col-10 col-md-8 col-lg-6 mx-auto" data-bs-ride="carousel">
            <div class="carousel-indicators">
                <button v-for="(h, i) in habilidades" :key="i" type="button" data-bs-target="#skillCarousel"
                    :data-bs-slide-to="i" :class="{ active: i === 0 }" :aria-current="i === 0 ? 'true' : undefined"
                    :aria-label="`Slide ${i + 1}`" />
            </div>
            <div class="carousel-inner">
                <div class="carousel-item" :class="{ active: index === 0 }" v-for="(habilidad, index) in habilidades"
                    :key="habilidad.title">
                    <div class="card text-center shadow p-3 mb-5">
                        <div class="card-body">
                            <h3 class="card-title">{{ habilidad.title }}</h3>
                            <div v-if="index === 0" class="row">
                                <div v-for="(col, idx) in sliceArray(habilidad.items)" :key="idx" class="col">
                                    <ul class="list-group list-group-flush">
                                        <li v-for="item in col" :key="item" class="list-group-item">
                                            {{ item }}
                                        </li>
                                    </ul>
                                </div>
                            </div>
                            <ul v-else class="list-group list-group-flush">
                                <li v-for="item in habilidad.items" :key="item" class="list-group-item">
                                    {{ item }}
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
            <button class="carousel-control-prev" type="button" data-bs-target="#skillCarousel" data-bs-slide="prev">
                <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                <span class="visually-hidden">Previous</span>
            </button>
            <button class="carousel-control-next" type="button" data-bs-target="#skillCarousel" data-bs-slide="next">
                <span class="carousel-control-next-icon" aria-hidden="true"></span>
                <span class="visually-hidden">Next</span>
            </button>
        </div>
    </article>
</template>

<style scoped>
h2 {
    padding-bottom: 10px;
}

.card {
    margin: 0 auto;
    width: 66%;
}

#skillCarousel {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 313px;
}

.carousel-container {
    position: relative;
    overflow: hidden;
    padding: 16px 0;
}

.carousel-item {
    position: absolute;
    opacity: 0;
    width: 100%;
    min-height: 100%;
    left: 0;
    top: 0;
    transition: transform 0.6s ease-in-out, opacity 0.6s ease-in-out;
}

.carousel-item.active {
    opacity: 1;
    position: relative;
}

.carousel-control-prev-icon,
.carousel-control-next-icon {
    filter: invert(1);
}

.carousel-control-next,
.carousel-control-prev:hover {
    background-color: transparent;
}

.carousel-indicators [data-bs-target] {
    background-color: #727272;
    border: none;
}

.carousel-indicators .active {
    background-color: #161616;
}

/*Media query para una mejor visualización en diferentes dispositivos*/
@media (max-width: 576px) {
    #skillCarousel {
        padding: 0;
        height: 450px;
        width: 95%;
    }

    h2 {
        margin-bottom: 1rem;
    }

    article.carousel-container {
        padding: 16px;
    }
}
</style>