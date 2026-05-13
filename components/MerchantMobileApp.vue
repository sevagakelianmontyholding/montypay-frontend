<!-- MerchantMobileApp.vue -->

<template>
    <section class="relative max-lg:py-10 lg:py-24 bg-[#0F0C22] text-white">
        <img
            src="/pattern-bg-2.webp"
            alt="BG Pattern"
            class="absolute top-0 start-0 object-cover w-full h-full"
        />

        <div class="container relative z-10">
            <h2 class="text-3xl font-bold">
                {{ t('Pages.Online Payments.Section 5.Title') }}
            </h2>

            <h3 class="text-xl font-bold mt-6">
                {{ t('Pages.Online Payments.Section 5.Sub Title 1') }}
            </h3>

            <p class="lg:w-2/3 text-xl mt-6">
                {{ t('Pages.Online Payments.Section 5.Description 1') }}
            </p>

            <div class="flex max-lg:flex-col items-center justify-between gap-10 mt-12">
                
                <!-- LEFT -->
                <div class="w-full">
                    <h3 class="text-xl font-bold">
                        {{ t('Pages.Online Payments.Section 5.Sub Title 2') }}
                    </h3>

                    <div class="flex flex-col gap-7 mt-7">
                        <div
                            v-for="(need, inx) in needs"
                            :key="inx"
                            class="flex max-lg:flex-col gap-5 p-8 bg-[#FFFFFF0D] rounded-xl backdrop-blur-md"
                        >
                            <div>
                                <img :src="need.icon" :alt="need.title" />
                            </div>

                            <div>
                                <h4 class="text-xl font-bold leading-none mb-2">
                                    {{ need.title }}
                                </h4>

                                <p>
                                    {{ need.description }}
                                </p>
                            </div>
                        </div>
                    </div>

                    <p class="text-xl font-normal mt-10">
                        {{ t('Pages.Online Payments.Section 5.Description 2') }}
                    </p>
                </div>

                <!-- RIGHT -->
                <div class="w-full h-full">
                    <ClientOnly>
                        <div
                            ref="containerRef"
                            class="w-full h-full min-h-[500px] relative"
                        >
                            <spline-viewer
                                url="https://prod.spline.design/l4YnsdzfdrnAIkin/scene.splinecode"
                                style="width: 100%; height: 100%; display: block"
                            />
                        </div>
                    </ClientOnly>
                </div>

            </div>
        </div>
    </section>
</template>

<script setup>
const { t } = useI18n()

const needs = [
    {
        icon: '/track-your-sales.svg',
        title: t('Pages.Online Payments.Section 5.Cards 1[0].Title'),
        description: t('Pages.Online Payments.Section 5.Cards 1[0].Description')
    },
    {
        icon: '/manage-transactions.svg',
        title: t('Pages.Online Payments.Section 5.Cards 1[1].Title'),
        description: t('Pages.Online Payments.Section 5.Cards 1[1].Description')
    },
    {
        icon: '/send-payment-links.svg',
        title: t('Pages.Online Payments.Section 5.Cards 1[2].Title'),
        description: t('Pages.Online Payments.Section 5.Cards 1[2].Description')
    },
    {
        icon: '/access-advanced-analytics.svg',
        title: t('Pages.Online Payments.Section 5.Cards 1[3].Title'),
        description: t('Pages.Online Payments.Section 5.Cards 1[3].Description')
    },
    {
        icon: '/stay-connected.svg',
        title: t('Pages.Online Payments.Section 5.Cards 1[4].Title'),
        description: t('Pages.Online Payments.Section 5.Cards 1[4].Description')
    }
]

const containerRef = ref(null)

let resizeObserver = null
let timeout = null

onMounted(async () => {
    // Load spline viewer only on client
    await import('@splinetool/viewer')

    timeout = setTimeout(() => {
        styleCanvas()
    }, 200)

    if (containerRef.value) {
        resizeObserver = new ResizeObserver(() => {
            styleCanvas()
        })

        resizeObserver.observe(containerRef.value)
    }
})

const styleCanvas = () => {
    const viewer = document.querySelector('spline-viewer')

    if (!viewer?.shadowRoot) return

    const canvas = viewer.shadowRoot.querySelector('canvas')

    if (!canvas) return

    canvas.style.width = '100%'
    canvas.style.height = '100%'
    canvas.style.cursor = 'default'
    canvas.style.pointerEvents = 'none'
}

onBeforeUnmount(() => {
    if (resizeObserver) {
        resizeObserver.disconnect()
    }

    if (timeout) {
        clearTimeout(timeout)
    }
})
</script>