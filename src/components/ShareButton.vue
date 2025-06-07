<script setup>
import { onMounted, onUnmounted, ref } from 'vue';

const isVisible = ref(true)
const isCopied = ref(false)
const currentURL = ref('')

async function copyLink() {
    try {
        await navigator.clipboard.writeText(currentURL.value)
        isCopied.value = true
        setTimeout(() => (isCopied.value = false), 2000)
    } catch {
        console.log('Error al copiar el enlace')
    }
}

onMounted(() => {
    currentURL.value = window.location.href

    //Determina si se ha desplazado cierta cantidad de pixeles
    const handleScroll = () => { isVisible.value = window.scrollY > 200 }
    window.addEventListener('scroll', handleScroll)

    handleScroll()

    onUnmounted(() => window.removeEventListener('scroll', handleScroll))
})
</script>

<template>
    <div class="share-floating" :style="{ bottom: isVisible ? '60px' : '20px' }">
        <button class="btn share-btn link-btn" :class="{ 'copied': isCopied }" @click="copyLink" type="button"
            :title="$t('shareButton.btnTitle')">
            <svg v-show="!isCopied" xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                class="bi bi-link-45deg share-icon" viewBox="0 0 16 16">
                <path
                    d="M4.715 6.542 3.343 7.914a3 3 0 1 0 4.243 4.243l1.828-1.829A3 3 0 0 0 8.586 5.5L8 6.086a1 1 0 0 0-.154.199 2 2 0 0 1 .861 3.337L6.88 11.45a2 2 0 1 1-2.83-2.83l.793-.792a4 4 0 0 1-.128-1.287z" />
                <path
                    d="M6.586 4.672A3 3 0 0 0 7.414 9.5l.775-.776a2 2 0 0 1-.896-3.346L9.12 3.55a2 2 0 1 1 2.83 2.83l-.793.792c.112.42.155.855.128 1.287l1.372-1.372a3 3 0 1 0-4.243-4.243z" />
            </svg>
            <svg v-show="isCopied" xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                class="bi bi-check2 share-icon" viewBox="0 0 16 16">
                <path
                    d="M13.854 3.646a.5.5 0 0 1 0 .708l-7 7a.5.5 0 0 1-.708 0l-3.5-3.5a.5.5 0 1 1 .708-.708L6.5 10.293l6.646-6.647a.5.5 0 0 1 .708 0" />
            </svg>
            <Transition name="fade">
                <div v-if="isCopied" class="copy-toast">{{ $t('shareButton.copyToast') }}</div>
            </Transition>
        </button>
    </div>
</template>

<style scoped>
/*Contenedor*/
.share-floating {
    position: fixed;
    right: 30px;
    display: flex;
    align-items: center;
    z-index: 999;
    transition: bottom 0.3s ease;
}

.share-btn {
    position: relative;
    width: 2rem;
    height: 2rem;
    border-radius: 50%;
    padding: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #212529;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.15);
    transition: background-color 0.3s;
}

.share-icon {
    width: 1.2rem;
    height: 1.2rem;
    fill: #E9ECEF;
    transition: fill 0.3s;
}

.share-btn.copied {
    background-color: #28A745 !important;
}

.share-btn.copied .share-icon {
    fill: white !important;
}

.copy-toast {
    position: absolute;
    top: 50%;
    right: 120%;
    background: rgba(33, 37, 41, 0.9);
    color: white;
    padding: 0.5em 1em;
    border-radius: 4px;
    font-size: 0.9rem;
    z-index: 1000;
    white-space: nowrap;
    transform: translateY(-50%);
}

.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}

/*Media queries para una mejor visualización en diferentes dispositivos*/
@media (hover: hover) and (pointer: fine) {
    .share-btn:hover {
        background-color: #E9ECEF;
    }

    .share-btn:hover .share-icon {
        fill: #212529;
    }

}

@media (hover: none) and (pointer: coarse) {
    .share-btn:active {
        background-color: #E9ECEF;
    }

    .share-btn:active .share-icon {
        fill: #212529;
    }
}
</style>