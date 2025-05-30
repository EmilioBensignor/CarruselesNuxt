<template>
    <div class="min-h-screen bg-gray-100 py-8">
        <div class="container mx-auto px-4">
            <h1 class="text-4xl font-bold text-center mb-12 text-gray-800">
                Carouseles Modernos - Prueba Sin Librerías
            </h1>

            <!-- Carousel 1: Básico con imágenes -->
            <section class="mb-16">
                <h2 class="text-2xl font-semibold mb-6">1. Carousel Básico de Imágenes</h2>
                <div class="relative max-w-4xl mx-auto">
                    <!-- Container del carousel con mouse drag -->
                    <div ref="carousel1" class="carousel-container flex overflow-x-auto touch-pan-x"
                        @scroll="updateIndicators(1)" @mousedown="startDrag" @mousemove="onDrag" @mouseup="stopDrag"
                        @mouseleave="stopDrag" @wheel="onWheel">
                        <div v-for="(image, index) in images" :key="index" class="carousel-item w-full flex-shrink-0">
                            <div
                                class="relative h-64 md:h-96 bg-gradient-to-r from-blue-500 to-purple-600 rounded-lg flex items-center justify-center">
                                <div class="text-center text-white">
                                    <div class="text-6xl mb-4">{{ image.emoji }}</div>
                                    <h3 class="text-2xl font-bold mb-2">{{ image.title }}</h3>
                                    <p class="text-lg opacity-90">{{ image.description }}</p>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Controles -->
                    <button @click="previousSlide(1)"
                        class="absolute left-4 top-1/2 transform -translate-y-1/2 bg-white/80 hover:bg-white rounded-full p-3 shadow-lg transition-all duration-200 z-10">
                        <svg class="w-6 h-6 text-gray-800" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7">
                            </path>
                        </svg>
                    </button>

                    <button @click="nextSlide(1)"
                        class="absolute right-4 top-1/2 transform -translate-y-1/2 bg-white/80 hover:bg-white rounded-full p-3 shadow-lg transition-all duration-200 z-10">
                        <svg class="w-6 h-6 text-gray-800" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7">
                            </path>
                        </svg>
                    </button>

                    <!-- Indicadores -->
                    <div class="absolute bottom-4 left-1/2 transform -translate-x-1/2 flex space-x-2">
                        <button v-for="(_, index) in images" :key="index" @click="goToSlide(1, index)"
                            class="w-3 h-3 rounded-full transition-all duration-300"
                            :class="currentSlides[1] === index ? 'bg-white scale-110' : 'bg-white/50'" />
                    </div>
                </div>
            </section>

            <!-- Carousel 2: Cards de productos -->
            <section class="mb-16">
                <h2 class="text-2xl font-semibold mb-6">2. Carousel de Productos (Multi-item)</h2>
                <div class="relative max-w-6xl mx-auto">
                    <div ref="carousel2" class="carousel-container flex overflow-x-auto gap-4 pb-4 touch-pan-x"
                        @scroll="updateIndicators(2)" @mousedown="startDrag" @mousemove="onDrag" @mouseup="stopDrag"
                        @mouseleave="stopDrag" @wheel="onWheel">
                        <div v-for="(product, index) in products" :key="index"
                            class="carousel-item flex-shrink-0 w-72 bg-white rounded-lg shadow-lg overflow-hidden hover:shadow-xl transition-shadow duration-300">
                            <div
                                class="h-48 bg-gradient-to-br from-gray-200 to-gray-300 flex items-center justify-center">
                                <span class="text-6xl">{{ product.emoji }}</span>
                            </div>
                            <div class="p-6">
                                <h3 class="text-xl font-bold mb-2 text-gray-800">{{ product.name }}</h3>
                                <p class="text-gray-600 mb-4">{{ product.description }}</p>
                                <div class="flex justify-between items-center">
                                    <span class="text-2xl font-bold text-blue-600">${{ product.price }}</span>
                                    <button
                                        class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-lg transition-colors">
                                        Comprar
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Controles para carousel de productos -->
                    <button @click="scrollProducts('left')"
                        class="absolute left-0 top-1/2 transform -translate-y-1/2 -translate-x-4 bg-white hover:bg-gray-50 rounded-full p-3 shadow-lg transition-all duration-200">
                        <svg class="w-6 h-6 text-gray-800" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7">
                            </path>
                        </svg>
                    </button>

                    <button @click="scrollProducts('right')"
                        class="absolute right-0 top-1/2 transform -translate-y-1/2 translate-x-4 bg-white hover:bg-gray-50 rounded-full p-3 shadow-lg transition-all duration-200">
                        <svg class="w-6 h-6 text-gray-800" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7">
                            </path>
                        </svg>
                    </button>
                </div>
            </section>

            <!-- Carousel 3: Testimonios con auto-play infinito -->
            <section class="mb-16">
                <h2 class="text-2xl font-semibold mb-6">3. Carousel de Testimonios (Auto-play Infinito)</h2>
                <div class="relative max-w-3xl mx-auto">
                    <div ref="carousel3" class="carousel-infinite flex overflow-x-auto touch-pan-x"
                        @mouseenter="pauseAutoPlay" @mouseleave="resumeAutoPlay" @mousedown="startDrag"
                        @mousemove="onDrag" @mouseup="stopDrag" @wheel="onWheel">
                        <!-- Items duplicados para loop infinito -->
                        <div v-for="(testimonial, index) in infiniteTestimonials" :key="`testimonial-${index}`"
                            class="carousel-item w-full flex-shrink-0">
                            <div class="bg-white rounded-lg shadow-lg p-8 mx-4">
                                <div class="text-center">
                                    <div
                                        class="w-16 h-16 rounded-full mx-auto mb-4 bg-gradient-to-r from-purple-400 to-pink-400 flex items-center justify-center">
                                        <span class="text-2xl text-white">{{ testimonial.avatar }}</span>
                                    </div>
                                    <blockquote class="text-lg text-gray-700 mb-4 italic">
                                        "{{ testimonial.quote }}"
                                    </blockquote>
                                    <footer class="text-gray-600">
                                        <strong>{{ testimonial.author }}</strong>
                                        <div class="text-sm">{{ testimonial.role }}</div>
                                    </footer>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- Indicadores para testimonios (basados en testimonios originales) -->
                    <div class="flex justify-center mt-6 space-x-2">
                        <button v-for="(_, index) in testimonials" :key="index" @click="goToInfiniteSlide(index)"
                            class="w-3 h-3 rounded-full transition-all duration-300"
                            :class="currentInfiniteSlide === index ? 'bg-purple-500 scale-110' : 'bg-gray-300'" />
                    </div>

                    <!-- Auto-play indicator -->
                    <div class="flex justify-center mt-4">
                        <button @click="toggleAutoPlay"
                            class="flex items-center space-x-2 px-4 py-2 bg-gray-100 hover:bg-gray-200 rounded-lg transition-colors">
                            <svg v-if="isAutoPlaying" class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20">
                                <path fill-rule="evenodd"
                                    d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zM7 8a1 1 0 012 0v4a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v4a1 1 0 102 0V8a1 1 0 00-1-1z"
                                    clip-rule="evenodd" />
                            </svg>
                            <svg v-else class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20">
                                <path fill-rule="evenodd"
                                    d="M10 18a8 8 0 100-16 8 8 0 000 16zM9.555 7.168A1 1 0 008 8v4a1 1 0 001.555.832l3-2a1 1 0 000-1.664l-3-2z"
                                    clip-rule="evenodd" />
                            </svg>
                            <span class="text-sm">{{ isAutoPlaying ? 'Pausar' : 'Reproducir' }}</span>
                        </button>
                    </div>
                </div>
            </section>

            <!-- Información técnica -->
            <section class="bg-white rounded-lg shadow-lg p-8">
                <h2 class="text-2xl font-semibold mb-6">Características Técnicas</h2>
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                    <div class="bg-green-50 p-4 rounded-lg">
                        <h3 class="font-semibold text-green-800 mb-2">✅ CSS Scroll Snap</h3>
                        <p class="text-sm text-green-700">Navegación suave y precisa sin JavaScript adicional</p>
                    </div>
                    <div class="bg-blue-50 p-4 rounded-lg">
                        <h3 class="font-semibold text-blue-800 mb-2">🖱️ Mouse Drag + Snap</h3>
                        <p class="text-sm text-blue-700">Arrastra suavemente y se posiciona en slides específicos</p>
                    </div>
                    <div class="bg-purple-50 p-4 rounded-lg">
                        <h3 class="font-semibold text-purple-800 mb-2">🔄 Loop Infinito + Snap</h3>
                        <p class="text-sm text-purple-700">Auto-play con posicionamiento preciso en slides</p>
                    </div>
                    <div class="bg-yellow-50 p-4 rounded-lg">
                        <h3 class="font-semibold text-yellow-800 mb-2">♿ Accesible</h3>
                        <p class="text-sm text-yellow-700">Compatible con lectores de pantalla y teclado</p>
                    </div>
                    <div class="bg-red-50 p-4 rounded-lg">
                        <h3 class="font-semibold text-red-800 mb-2">📦 Sin dependencias</h3>
                        <p class="text-sm text-red-700">No requiere librerías externas como Swiper</p>
                    </div>
                    <div class="bg-indigo-50 p-4 rounded-lg">
                        <h3 class="font-semibold text-indigo-800 mb-2">🚀 Performance</h3>
                        <p class="text-sm text-indigo-700">Aceleración nativa del navegador</p>
                    </div>
                </div>
            </section>
        </div>
    </div>
</template>

<script setup>
// Datos de ejemplo
const images = ref([
    { emoji: '🏔️', title: 'Montañas', description: 'Paisajes increíbles' },
    { emoji: '🏖️', title: 'Playa', description: 'Relajación total' },
    { emoji: '🌲', title: 'Bosque', description: 'Naturaleza pura' },
    { emoji: '🏜️', title: 'Desierto', description: 'Aventura extrema' },
    { emoji: '🌊', title: 'Océano', description: 'Inmensidad azul' }
]);

const products = ref([
    { emoji: '💻', name: 'MacBook Pro', description: 'Laptop profesional para desarrolladores', price: 2499 },
    { emoji: '📱', name: 'iPhone 15', description: 'El smartphone más avanzado', price: 999 },
    { emoji: '⌚', name: 'Apple Watch', description: 'Tu asistente personal en la muñeca', price: 399 },
    { emoji: '🎧', name: 'AirPods Pro', description: 'Audio inmersivo sin cables', price: 249 },
    { emoji: '🖥️', name: 'iMac', description: 'Computadora todo en uno', price: 1799 },
    { emoji: '📺', name: 'Apple TV', description: 'Entretenimiento en 4K', price: 179 },
    { emoji: '⌨️', name: 'Magic Keyboard', description: 'Teclado inalámbrico premium', price: 99 }
]);

const testimonials = ref([
    {
        avatar: '👨‍💻',
        quote: 'Este enfoque me permitió reducir el bundle size de mi app en un 40%',
        author: 'Carlos Ruiz',
        role: 'Frontend Developer'
    },
    {
        avatar: '👩‍🎨',
        quote: 'La personalización es increíble y mantiene la accesibilidad',
        author: 'Ana García',
        role: 'UX Designer'
    },
    {
        avatar: '👨‍🚀',
        quote: 'Performance nativo del navegador, no más librerías pesadas',
        author: 'Miguel Torres',
        role: 'Tech Lead'
    },
    {
        avatar: '👩‍💼',
        quote: 'Fácil de mantener y actualizar en proyectos grandes',
        author: 'Laura Vega',
        role: 'Project Manager'
    }
]);

// Estado reactivo
const currentSlides = ref({ 1: 0, 2: 0, 3: 0 });
const currentInfiniteSlide = ref(0); // Para el carousel infinito
const carousel1 = ref(null);
const carousel2 = ref(null);
const carousel3 = ref(null);
const isAutoPlaying = ref(true);
let autoPlayInterval = null;

// Estados para mouse drag
const isDragging = ref(false);
const startX = ref(0);
const scrollLeft = ref(0);

// Crear array infinito para testimonios (duplicamos items)
const infiniteTestimonials = computed(() => {
    // Duplicamos: [...prev, ...original, ...next]
    return [...testimonials.value, ...testimonials.value, ...testimonials.value];
});

// Métodos para mouse drag mejorado
const startDrag = (e) => {
    if (e.target.closest('button')) return; // No drag en botones

    const carousel = e.currentTarget;
    isDragging.value = true;
    startX.value = e.pageX - carousel.offsetLeft;
    scrollLeft.value = carousel.scrollLeft;

    // Deshabilitar scroll smooth y snap durante drag
    carousel.classList.add('carousel-dragging');
    carousel.style.scrollSnapType = 'none';
    carousel.style.scrollBehavior = 'auto';
};

const onDrag = (e) => {
    if (!isDragging.value) return;
    e.preventDefault();

    const carousel = e.currentTarget;
    const x = e.pageX - carousel.offsetLeft;
    const walk = (x - startX.value) * 1.5; // Sensibilidad ajustada
    carousel.scrollLeft = scrollLeft.value - walk;
};

const stopDrag = (e) => {
    if (!isDragging.value) return;

    isDragging.value = false;
    const carousel = e.currentTarget;

    // Restaurar scroll behavior y snap después de drag
    setTimeout(() => {
        carousel.classList.remove('carousel-dragging');

        // Determinar qué tipo de carousel es y aplicar estilos correspondientes
        if (carousel === carousel3.value) {
            // Carousel infinito: activar snap
            carousel.style.scrollSnapType = 'x mandatory';
            carousel.style.scrollBehavior = 'smooth';

            // Forzar snap al slide más cercano
            const slideWidth = carousel.offsetWidth;
            const currentScroll = carousel.scrollLeft;
            const nearestSlide = Math.round(currentScroll / slideWidth);
            carousel.scrollTo({
                left: nearestSlide * slideWidth,
                behavior: 'smooth'
            });
        } else {
            // Carouseles 1 y 2: mantener snap activado
            carousel.style.scrollSnapType = 'x mandatory';
            carousel.style.scrollBehavior = 'smooth';
        }
    }, 100);
};

// Mouse wheel horizontal scroll mejorado
const onWheel = (e) => {
    e.preventDefault();
    const carousel = e.currentTarget;

    // Para carouseles con snap, hacer scroll suave por slides
    if (carousel === carousel1.value || carousel === carousel3.value) {
        const slideWidth = carousel.offsetWidth;
        const currentScroll = carousel.scrollLeft;
        const direction = e.deltaY > 0 ? 1 : -1;
        const currentSlide = Math.round(currentScroll / slideWidth);
        const nextSlide = Math.max(0, currentSlide + direction);

        carousel.scrollTo({
            left: nextSlide * slideWidth,
            behavior: 'smooth'
        });
    } else {
        // Para carousel de productos, scroll libre
        carousel.scrollLeft += e.deltaY;
    }
};

// Métodos de navegación
const goToSlide = (carouselId, index) => {
    const carousel = carouselId === 1 ? carousel1.value :
        carouselId === 2 ? carousel2.value : carousel3.value;

    if (!carousel) return;

    const slideWidth = carousel.offsetWidth;
    carousel.scrollTo({
        left: index * slideWidth,
        behavior: 'smooth'
    });

    currentSlides.value[carouselId] = index;
};

const nextSlide = (carouselId) => {
    const totalSlides = carouselId === 1 ? images.value.length :
        carouselId === 2 ? products.value.length : testimonials.value.length;
    const current = currentSlides.value[carouselId];
    const next = current < totalSlides - 1 ? current + 1 : 0;
    goToSlide(carouselId, next);
};

const previousSlide = (carouselId) => {
    const totalSlides = carouselId === 1 ? images.value.length :
        carouselId === 2 ? products.value.length : testimonials.value.length;
    const current = currentSlides.value[carouselId];
    const prev = current > 0 ? current - 1 : totalSlides - 1;
    goToSlide(carouselId, prev);
};

const scrollProducts = (direction) => {
    const carousel = carousel2.value;
    if (!carousel) return;

    const cardWidth = 288 + 16; // w-72 + gap
    const scrollAmount = direction === 'left' ? -cardWidth * 2 : cardWidth * 2;

    carousel.scrollBy({
        left: scrollAmount,
        behavior: 'smooth'
    });
};

const updateIndicators = (carouselId) => {
    const carousel = carouselId === 1 ? carousel1.value :
        carouselId === 2 ? carousel2.value : carousel3.value;

    if (!carousel) return;

    const slideWidth = carousel.offsetWidth;
    const newSlide = Math.round(carousel.scrollLeft / slideWidth);
    currentSlides.value[carouselId] = newSlide;
};

// Auto-play infinito para testimonios
const startAutoPlay = () => {
    if (autoPlayInterval) clearInterval(autoPlayInterval);
    autoPlayInterval = setInterval(() => {
        if (isAutoPlaying.value && carousel3.value) {
            moveToNextInfiniteSlide();
        }
    }, 4000);
};

const moveToNextInfiniteSlide = () => {
    const carousel = carousel3.value;
    if (!carousel) return;

    const slideWidth = carousel.offsetWidth;
    const currentScroll = carousel.scrollLeft;
    const originalLength = testimonials.value.length;

    // Calcular próximo slide
    const nextScrollPosition = currentScroll + slideWidth;

    // Scroll suave al siguiente
    carousel.scrollTo({
        left: nextScrollPosition,
        behavior: 'smooth'
    });

    // Actualizar indicador (basado en slide original)
    currentInfiniteSlide.value = (currentInfiniteSlide.value + 1) % originalLength;

    // Verificar si necesitamos "saltar" para mantener loop infinito
    setTimeout(() => {
        const newScroll = carousel.scrollLeft;
        const slidePosition = Math.round(newScroll / slideWidth);

        // Si estamos en las copias del final, saltar al inicio real
        if (slidePosition >= originalLength * 2) {
            carousel.style.scrollBehavior = 'auto';
            carousel.scrollLeft = slideWidth * originalLength; // Posición equivalente en el set original
            setTimeout(() => {
                carousel.style.scrollBehavior = 'smooth';
            }, 50);
        }
        // Si estamos antes del primer set, saltar al final del set original
        else if (slidePosition < originalLength) {
            carousel.style.scrollBehavior = 'auto';
            carousel.scrollLeft = slideWidth * (originalLength + currentInfiniteSlide.value);
            setTimeout(() => {
                carousel.style.scrollBehavior = 'smooth';
            }, 50);
        }
    }, 600); // Esperar a que termine la animación
};

const goToInfiniteSlide = (index) => {
    const carousel = carousel3.value;
    if (!carousel) return;

    const slideWidth = carousel.offsetWidth;
    const originalLength = testimonials.value.length;

    // Ir al slide en el set del medio (set original) con snap
    const targetPosition = slideWidth * (originalLength + index);

    carousel.style.scrollSnapType = 'x mandatory';
    carousel.scrollTo({
        left: targetPosition,
        behavior: 'smooth'
    });

    currentInfiniteSlide.value = index;
};

const pauseAutoPlay = () => {
    isAutoPlaying.value = false;
};

const resumeAutoPlay = () => {
    isAutoPlaying.value = true;
};

const toggleAutoPlay = () => {
    isAutoPlaying.value = !isAutoPlaying.value;
};

// Lifecycle
onMounted(() => {
    startAutoPlay();

    // Inicializar carousel infinito en la posición del medio
    nextTick(() => {
        if (carousel3.value) {
            const slideWidth = carousel3.value.offsetWidth;
            const originalLength = testimonials.value.length;
            // Posicionar en el set del medio sin animación
            carousel3.value.style.scrollSnapType = 'none';
            carousel3.value.style.scrollBehavior = 'auto';
            carousel3.value.scrollLeft = slideWidth * originalLength;
            setTimeout(() => {
                carousel3.value.style.scrollSnapType = 'x mandatory';
                carousel3.value.style.scrollBehavior = 'smooth';
            }, 100);
        }
    });
});

onUnmounted(() => {
    if (autoPlayInterval) {
        clearInterval(autoPlayInterval);
    }
});

// Meta tags
useHead({
    title: 'Carouseles Modernos - Nuxt + Tailwind',
    meta: [
        { name: 'description', content: 'Prueba de carouseles modernos sin librerías usando CSS Scroll Snap' }
    ]
});
</script>