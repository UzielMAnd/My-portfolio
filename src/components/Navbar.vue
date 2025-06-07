<script setup>
import { useI18n } from 'vue-i18n'
import { onMounted, onBeforeUnmount, ref } from 'vue'

const { locale, t } = useI18n()

//Referencias a elementos del template
const navbarRef = ref(null)
const collapseRef = ref(null)

//Estados reactivos
const ignoreScroll = ref(false)
const lastScrollTop = ref(window.pageYOffset)
const navbarHeight = ref(0)
const hideThreshold = 10

let bsCollapse = null

function showNavbar() {
    if (!navbarRef.value) return
    navbarRef.value.style.top = '0'
}

//Función para desplazarse a una sección
const scrollTo = (id) => {
    const el = document.getElementById(id)
    if (!el) return
    window.scrollTo({ top: el.offsetTop - navbarHeight.value, behavior: 'smooth' })
}

function toggleLanguage() {
    ignoreScroll.value = true
    locale.value = locale.value === 'es' ? 'en' : 'es'
    showNavbar()

    setTimeout(() => {
        ignoreScroll.value = false
        showNavbar()
    }, 300)
}

//Función para cerrar menú al desplazarse en dispositivos móviles
function collapseMenu() {
    if (collapseRef.value?.classList.contains('show') && bsCollapse) {
        bsCollapse.hide()
    }
}

//Determina si la navbar se oculta o se muestra
function updateNavbarPos() {
    if (!navbarRef.value) return

    const curr = window.pageYOffset
    const diff = curr - lastScrollTop.value

    if (curr <= navbarHeight.value) showNavbar()
    else {
        if (diff > hideThreshold) navbarRef.value.style.top = `-${navbarHeight.value}px`
        else if (diff < -hideThreshold) showNavbar()
    }
    lastScrollTop.value = curr
}

//Handler de scroll
function onScroll() {
    if (ignoreScroll.value) {
        showNavbar()
        return
    }
    collapseMenu()
    updateNavbarPos()
}

onMounted(() => {
    if (navbarRef.value) {
        navbarHeight.value = navbarRef.value.offsetHeight
        lastScrollTop.value = window.pageYOffset
    }

    if (collapseRef.value) {
        bsCollapse = new bootstrap.Collapse(collapseRef.value, { toggle: false })
    }
    window.addEventListener('scroll', onScroll)
})

onBeforeUnmount(() => window.removeEventListener('scroll', onScroll))
</script>

<template>
    <nav ref="navbarRef" class="navbar navbar-expand-lg navbar-dark bg-dark" id="contentNavbar">
        <div class="container-fluid">
            <span class="navbar-brand mb-0 h1">{{ t('navbar.portfolio') }}</span>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
                data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false"
                aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div ref="collapseRef" class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav me-auto mb-2 mb-lg-0">
                    <li class="nav-item">
                        <a class="nav-link" href="#about_me" @click.prevent="scrollTo('about_me')">
                            {{ t('navbar.about') }}</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#skills" @click.prevent="scrollTo('skills')">
                            {{ t('navbar.skills') }}</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#projects" @click.prevent="scrollTo('projects')">
                            {{ t('navbar.projects') }}</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#contact" @click.prevent="scrollTo('contact')">
                            {{ t('navbar.contact') }}</a>
                    </li>
                </ul>
                <button @click="toggleLanguage" class="lang-btn">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                        class="bi bi-globe" viewBox="0 0 16 16">
                        <path
                            d="M0 8a8 8 0 1 1 16 0A8 8 0 0 1 0 8m7.5-6.923c-.67.204-1.335.82-1.887 1.855A8 8 0 0 0 5.145 4H7.5zM4.09 4a9.3 9.3 0 0 1 .64-1.539 7 7 0 0 1 .597-.933A7.03 7.03 0 0 0 2.255 4zm-.582 3.5c.03-.877.138-1.718.312-2.5H1.674a7 7 0 0 0-.656 2.5zM4.847 5a12.5 12.5 0 0 0-.338 2.5H7.5V5zM8.5 5v2.5h2.99a12.5 12.5 0 0 0-.337-2.5zM4.51 8.5a12.5 12.5 0 0 0 .337 2.5H7.5V8.5zm3.99 0V11h2.653c.187-.765.306-1.608.338-2.5zM5.145 12q.208.58.468 1.068c.552 1.035 1.218 1.65 1.887 1.855V12zm.182 2.472a7 7 0 0 1-.597-.933A9.3 9.3 0 0 1 4.09 12H2.255a7 7 0 0 0 3.072 2.472M3.82 11a13.7 13.7 0 0 1-.312-2.5h-2.49c.062.89.291 1.733.656 2.5zm6.853 3.472A7 7 0 0 0 13.745 12H11.91a9.3 9.3 0 0 1-.64 1.539 7 7 0 0 1-.597.933M8.5 12v2.923c.67-.204 1.335-.82 1.887-1.855q.26-.487.468-1.068zm3.68-1h2.146c.365-.767.594-1.61.656-2.5h-2.49a13.7 13.7 0 0 1-.312 2.5m2.802-3.5a7 7 0 0 0-.656-2.5H12.18c.174.782.282 1.623.312 2.5zM11.27 2.461c.247.464.462.98.64 1.539h1.835a7 7 0 0 0-3.072-2.472c.218.284.418.598.597.933M10.855 4a8 8 0 0 0-.468-1.068C9.835 1.897 9.17 1.282 8.5 1.077V4z" />
                    </svg>
                    {{ t('language') }}
                </button>
            </div>
        </div>
    </nav>
</template>

<style scoped>
.lang-btn {
    display: flex;
    align-items: center;
    gap: 0.4em;
    padding: 0.6em 1em;
    transition: background-color 0.2s ease, color 0.2s ease;
}

.lang-btn svg {
    width: 1em;
    height: 1em;
}

.navbar {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 999;
    transition: top 0.3s ease-in-out;
}

/*Media queries para una mejor visualización en diferentes dispositivos*/
@media (hover: none) and (pointer: coarse) {

    .lang-btn:active,
    .navbar-toggler:active {
        background-color: #9AC3EB;
        color: black;
        transition: background-color 0.1s ease, color 0.1s ease;
    }
}

@media (hover: hover) and (pointer: fine) {

    .lang-btn:hover,
    .navbar-toggler:hover {
        background-color: #9AC3EB;
        color: black;
    }
}
</style>
