<script setup>
import { computed, onBeforeUnmount, onMounted, reactive } from 'vue'
import { useI18n } from 'vue-i18n'

const { locale, messages } = useI18n()
const proyectos = computed(() => messages.value[locale.value].projects.items)
const hoverIdx = reactive({})
const touchIdx = reactive({})

const isTouchDevice = ('ontouchstart' in window) || (navigator.maxTouchPoints > 0) || (navigator.msMaxTouchPoints > 0)

const resolveImg = (path) => {
    const cleanPath = path.replace(/^\/+/, '')
    return `${import.meta.env.BASE_URL || '/'}${cleanPath}`
}

//Función exclusiva de dispositivos móviles, usada para mostrar nombres y colores de íconos
const handleTechTap = (projIdx, techName) => {
    if (touchIdx[projIdx] === techName) {
        delete touchIdx[projIdx]
        console.log('tap off', projIdx, techName, touchIdx)
    }
    else {
        touchIdx[projIdx] = techName
        console.log('tap on', projIdx, techName, touchIdx)
    }
}

//Similar a la función anterior, usada para ocultar nombres y colores de íconos al tocar en otros lados
const handleOutsideTap = (e) => {
    if (!e.target.closest('.tech-wrapper')) Object.keys(touchIdx).forEach(key => delete touchIdx[key])
}

onMounted(() => document.addEventListener('click', handleOutsideTap))
onBeforeUnmount(() => document.removeEventListener('click', handleOutsideTap))
</script>

<template>
    <article class="container" id="projects">
        <hr class="section-div">
        <h2>{{ messages[locale].projects.title }}</h2>
        <div v-for="(proyecto, index) in proyectos" :key="proyecto.id" :id="proyecto.id"
            :class="['project', { 'inverted-project': index % 2 !== 0 }]">
            <div>
                <img :src="resolveImg(proyecto.image)" :alt="proyecto.imageAlt" class="project-img" loading="lazy"
                    width="896" height="834" style="width: 100%;height: auto;">
            </div>
            <div>
                <h3>{{ proyecto.title }}</h3>
                <p>{{ proyecto.description }}</p>
                <div class="small-screen-container">
                    <a v-if="proyecto.link" :href="proyecto.link" class="btn btn-app-test" target="_blank"
                        rel="noopener noreferrer">{{ messages[locale].projects.button }}</a>
                </div>
            </div>
            <div>
                <h4>{{ messages[locale].projects.subtitle }}</h4>
                <div class="technologies">
                    <div v-for="tech in proyecto.technologies" class="tech-wrapper" :key="tech.name"
                        @touchend.prevent="handleTechTap(index, tech.name)"
                        @click="isTouchDevice && handleTechTap(index, tech.name)"
                        @mouseover="!isTouchDevice && (hoverIdx[index] = tech.name)"
                        @mouseleave="!isTouchDevice && delete hoverIdx[index]"
                        :style="{ cursor: isTouchDevice ? 'pointer' : 'default' }">
                        <img :src="tech.icon" :alt="tech.name" :title="!isTouchDevice ? tech.name : ''"
                            class="tech-icon" :style="{
                                filter: ((!isTouchDevice && hoverIdx[index] === tech.name) ||
                                    (isTouchDevice && touchIdx[index] === tech.name))
                                    ? `drop-shadow(0px 0px 6px ${tech.color})` : 'none'
                            }">
                        <span v-if="isTouchDevice"
                            :class="{ 'tech-name': true, 'tech-name-visible': touchIdx[index] === tech.name }">
                            {{ tech.name }}
                        </span>
                    </div>
                </div>
            </div>
        </div>
    </article>
</template>

<style scoped>
h4 {
    text-align: center;
    font-size: 1.05rem;
}

article {
    margin: 64px auto;
    padding: 16px 0;
    box-sizing: border-box;
}

/*Clase general de cada proyecto*/
.project {
    margin: 0 auto 64px;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    gap: 30px;
    padding: 2rem;
}

.project>div {
    flex: 1 1 450px;

}

.inverted-project {
    flex-direction: row-reverse;
}

.project-img {
    width: 100%;
    height: auto;
    object-fit: contain;
}

/*Contenedor de tecnologías*/
.technologies {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap: 50px;
    padding: 10px;
}

.technologies img {
    width: 80px;
    height: 80px;
    object-fit: contain;
}

/*Contiene los íconos y nombres de las tecnologías*/
.tech-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    flex: 0 0 100px;
    min-height: 100px;
    max-width: 100px;
    box-sizing: border-box;
}

.tech-icon {
    width: 80px;
    height: 80px;
    object-fit: contain;
    transition: filter 0.3s ease-in-out;
}

.tech-name {
    margin-top: 4px;
    font-size: 0.75rem;
    color: #212529;
    text-align: center;
    opacity: 0;
    transform: translateY(5px);
    transition: opacity 0.3s ease, transform 0.3s ease;
    white-space: normal;
    overflow-wrap: break-word;
    word-break: break-word;
    width: 100%;
}

.tech-name-visible {
    opacity: 1;
    transform: translateY(0);
}

.btn-app-test {
    background-color: #6C757D;
    color: #ffffff;
    transition: background-color 0.3s, color 0.3s;
}

/*Media queries para una mejor visualización en diferentes dispositivos*/
@media (max-width: 600px) {
    article {
        padding: 16px;
    }
}

@media (max-width: 1440px) {
    .small-screen-container {
        display: flex;
        justify-content: center;
        margin-top: 1rem;
    }
}

@media (hover: none) and (pointer: coarse) {
    .tech-wrapper {
        flex: 0 0 100px;
        min-height: 100px;
    }

    .tech-name {
        white-space: normal;
        overflow-wrap: break-word;
        max-width: 80px;
        word-break: break-word;
    }

    .btn-app-test:active {
        background-color: #9AC3EB;
        color: #000000;
    }
}

@media (hover: hover) and (pointer: fine) {
    .btn-app-test:hover {
        background-color: #9AC3EB;
        color: #000000;
    }
}
</style>