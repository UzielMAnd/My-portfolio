<script setup>
import { useI18n } from 'vue-i18n'

const { t } = useI18n()

const contacts = [
    { href: 'https://linkedin.com/in/uziel-moreno-andrade', icon: new URL('../assets/linkedin.svg', import.meta.url).href, alt: 'LinkedIn' },
    { href: 'mailto:uzielmoreno18@gmail.com', icon: new URL('../assets/mail.svg', import.meta.url).href, alt: 'Email' }
]

const githubAccounts = [
    { href: 'https://github.com/UzielMo4-18', label: t('footer.menu.school') },
    { href: 'https://github.com/UzielMAnd', label: t('footer.menu.personal') }
]
</script>

<template>
    <footer class="text-center">
        <div class="container py-4">
            <h2 id="contact">{{ t('footer.title') }}</h2>
            <div class="d-flex gap-3 mb-4 justify-content-center">
                <a v-for="({ href, icon, alt }, index) in contacts" :key="index" :href="href" class="footer-icon"
                    target="_blank" rel="noopener noreferrer">
                    <img :src="icon" :alt="alt" class="svg-img" width="26" height="26" />
                </a>
                <div class="dropdown">
                    <a class="dropdown-toggle d-flex align-items-center" id="accounts-GH" role="button"
                        data-bs-toggle="dropdown" aria-expanded="false" aria-label="Github accounts">
                        <svg height="24px" viewBox="0 0 496 512">
                            <path fill="#ffffff"
                                d="M165.9 397.4c0 2-2.3 3.6-5.2 3.6-3.3 .3-5.6-1.3-5.6-3.6 0-2 2.3-3.6 5.2-3.6 3-.3 5.6 1.3 5.6 3.6zm-31.1-4.5c-.7 2 1.3 4.3 4.3 4.9 2.6 1 5.6 0 6.2-2s-1.3-4.3-4.3-5.2c-2.6-.7-5.5 .3-6.2 2.3zm44.2-1.7c-2.9 .7-4.9 2.6-4.6 4.9 .3 2 2.9 3.3 5.9 2.6 2.9-.7 4.9-2.6 4.6-4.6-.3-1.9-3-3.2-5.9-2.9zM244.8 8C106.1 8 0 113.3 0 252c0 110.9 69.8 205.8 169.5 239.2 12.8 2.3 17.3-5.6 17.3-12.1 0-6.2-.3-40.4-.3-61.4 0 0-70 15-84.7-29.8 0 0-11.4-29.1-27.8-36.6 0 0-22.9-15.7 1.6-15.4 0 0 24.9 2 38.6 25.8 21.9 38.6 58.6 27.5 72.9 20.9 2.3-16 8.8-27.1 16-33.7-55.9-6.2-112.3-14.3-112.3-110.5 0-27.5 7.6-41.3 23.6-58.9-2.6-6.5-11.1-33.3 2.6-67.9 20.9-6.5 69 27 69 27 20-5.6 41.5-8.5 62.8-8.5s42.8 2.9 62.8 8.5c0 0 48.1-33.6 69-27 13.7 34.7 5.2 61.4 2.6 67.9 16 17.7 25.8 31.5 25.8 58.9 0 96.5-58.9 104.2-114.8 110.5 9.2 7.9 17 22.9 17 46.4 0 33.7-.3 75.4-.3 83.6 0 6.5 4.6 14.4 17.3 12.1C428.2 457.8 496 362.9 496 252 496 113.3 383.5 8 244.8 8zM97.2 352.9c-1.3 1-1 3.3 .7 5.2 1.6 1.6 3.9 2.3 5.2 1 1.3-1 1-3.3-.7-5.2-1.6-1.6-3.9-2.3-5.2-1zm-10.8-8.1c-.7 1.3 .3 2.9 2.3 3.9 1.6 1 3.6 .7 4.3-.7 .7-1.3-.3-2.9-2.3-3.9-2-.6-3.6-.3-4.3 .7zm32.4 35.6c-1.6 1.3-1 4.3 1.3 6.2 2.3 2.3 5.2 2.6 6.5 1 1.3-1.3 .7-4.3-1.3-6.2-2.2-2.3-5.2-2.6-6.5-1zm-11.4-14.7c-1.6 1-1.6 3.6 0 5.9 1.6 2.3 4.3 3.3 5.6 2.3 1.6-1.3 1.6-3.9 0-6.2-1.4-2.3-4-3.3-5.6-2z" />
                        </svg>
                    </a>
                    <ul class="dropdown-menu dropdown-menu-end" aria-labelledby="githubMenu">
                        <li v-for="(acct, index) in githubAccounts" :key="index">
                            <a class="dropdown-item" :href="acct.href" target="_blank" rel="noopener noreferrer">
                                {{ acct.label }}
                            </a>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
        <div class="text-center p-3">© 2025 Uziel Moreno Andrade</div>
    </footer>
</template>

<style scoped>
h2 {
    font-size: 1.5rem;
    font-weight: 500;
    color: white;
}

a.dropdown-toggle {
    font-weight: 500;
    color: white;
    text-decoration: inherit;
    transition: background-color 0.2s ease;
}

.dropdown-menu {
    background-color: #343a40;
    padding: 0;
    border: none;
    border-radius: 0.25rem;
    overflow: hidden;
}

.dropdown-item {
    color: white;
    padding: 0.5rem 1rem;
    transition: background-color 0.2s ease, color 0.2s ease;
}

.dropdown-item:hover {
    background-color: #9AC3EB;
    color: black;
}

.footer-icon {
    color: white;
    background-color: transparent;
}

.footer-buttons {
    justify-content: center;
    align-items: center;
}

.svg-img {
    filter: invert(1);
}

/*Media queries para una mejor visualización en diferentes dispositivos*/
@media (hover: none) and (pointer: coarse) {
    a.dropdown-toggle:active {
        color: #9AC3EB;
        transition: backgorund-color 0.2s ease;
    }
}

@media (hover: hover) and (pointer: fine) {
    a.dropdown-toggle:hover {
        color: #9AC3EB;
    }
}
</style>