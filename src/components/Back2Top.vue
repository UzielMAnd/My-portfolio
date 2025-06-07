<script setup>
import { onMounted, onUnmounted, ref } from "vue"

const isVisible = ref(false);

//Función para controlar la visualización del botón
const handleScroll = () => {
    if (window.scrollY > 200) isVisible.value = true
    else isVisible.value = false
};

function scrollToTop() {
    window.scrollTo({ top: 0 })
}

onMounted(() => window.addEventListener("scroll", handleScroll));
onUnmounted(() => window.removeEventListener("scroll", handleScroll));
</script>

<template>
    <Transition>
        <div v-if="isVisible">
            <button class="floating-btn" @click="scrollToTop" :title="$t('back2TopButton')">
                <svg xmlns="http://www.w3.org/2000/svg" class="bi bi-arrow-up floating-btn-icon" viewBox="0 0 16 16">
                    <path fill-rule="evenodd"
                        d="M8 15a.5.5 0 0 0 .5-.5V2.707l3.146 3.147a.5.5 0 0 0 .708-.708l-4-4a.5.5 0 0 0-.708 0l-4 4a.5.5 0 1 0 .708.708L7.5 2.707V14.5a.5.5 0 0 0 .5.5" />
                </svg>
            </button>
        </div>
    </Transition>
</template>

<style scoped>
.floating-btn {
    all: unset;
    position: fixed;
    bottom: 20px;
    right: 30px;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #212529;
    cursor: pointer;
    height: 2rem;
    width: 2rem;
    border-radius: 50%;
    transition: background-color 0.3s;
}

.floating-btn-icon {
    width: 100%;
    height: 75%;
    fill: #E9ECEF;
    display: block;
    transition: fill 0.3s;
}

.v-enter-active,
.v-leave-active {
    transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
    opacity: 0;
}

/*Media queries para una mejor visualización en diferentes dispositivos*/
@media (hover: none) and (pointer: coarse) {
    .floating-btn:active {
        background-color: #E9ECEF;
    }

    .floating-btn:active .floating-btn-icon {
        fill: #212529;
    }
}

@media (hover: hover) and (pointer: fine) {
    .floating-btn:hover {
        background-color: #E9ECEF;
    }

    .floating-btn:hover .floating-btn-icon {
        fill: #212529;
    }
}
</style>