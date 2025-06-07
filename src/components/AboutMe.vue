<script setup>
import { computed } from 'vue';
import { useI18n } from 'vue-i18n';

const { locale } = useI18n()

const cvPath = computed(() => {
    return locale.value === 'es' ? '/CV_ES.pdf' : '/CV_EN.pdf'
})
</script>

<template>
    <article class="container" id="about_me" aria-labelledby="about-title">
        <h1>{{ $t('aboutMe.title') }}</h1>
        <h2 id="about-title">{{ $t('aboutMe.subtitle') }}</h2>
        <p v-html="$t('aboutMe.description')"></p>
        <div class="btn-container">
            <a :href="cvPath" class="btn cv-btn" download>
                <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                    class="bi bi-file-earmark-arrow-down-fill" viewBox="0 0 16 16">
                    <path
                        d="M9.293 0H4a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V4.707A1 1 0 0 0 13.707 4L10 .293A1 1 0 0 0 9.293 0M9.5 3.5v-2l3 3h-2a1 1 0 0 1-1-1m-1 4v3.793l1.146-1.147a.5.5 0 0 1 .708.708l-2 2a.5.5 0 0 1-.708 0l-2-2a.5.5 0 0 1 .708-.708L7.5 11.293V7.5a.5.5 0 0 1 1 0" />
                </svg>
                {{ $t('aboutMe.downloadButton') }}
            </a>
        </div>
    </article>
</template>

<style scoped>
article {
    margin: 64px auto;
    scroll-margin-top: 56px;
    padding: 16px 0;
    box-sizing: border-box;
}

.btn-container {
    display: flex;
    justify-content: center;
    margin-top: 1rem;
}

.cv-btn {
    display: flex;
    align-items: center;
    gap: 0.4em;
    padding: 0.6em 1em;
    background-color: #6C757D;
    color: #ffffff;
    transition: background-color 0.3s, color 0.3s;
}

.cv-btn svg {
    width: 1em;
    height: 1em;
}

/*Media queries para una mejor visualización en diferentes dispositivos*/
@media (max-width: 600px) {
    article {
        padding: 16px;
    }
}

@media (hover: none) and (pointer: coarse) {
    .cv-btn:active {
        background-color: #9AC3EB;
        color: black;
        transition: background-color 0.1s ease, color 0.1s ease;
    }
}

@media (hover: hover) and (pointer: fine) {
    .cv-btn:hover {
        background-color: #9AC3EB;
        color: black;
    }
}
</style>