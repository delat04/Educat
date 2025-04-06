<template>
  <div class="gallery-container">
    <div class="scroll-container" :class="{ 'is-loading': isLoading }">
      <!-- Loading overlay -->
      <div v-if="isLoading" class="loading-overlay">
        <div class="loading-spinner"></div>
      </div>

      <!-- Navigation dots -->
      <div class="nav-dots">
        <button
            v-for="(_, index) in images"
            :key="index"
            :class="{ active: currentImageIndex === index }"
            @click="scrollToImage(index)"
            class="nav-dot"
        ></button>
      </div>

      <!-- Images -->
      <div
          v-for="(image, index) in images"
          :key="index"
          class="image-wrapper"
          :class="{ 'is-visible': visibleImages[index] }"
          :style="{
          backgroundImage: `url(${image})`,
          '--delay': `${index * 0.2}s`
        }"
          ref="imageRefs"
      >
        <!-- Parallax overlay -->
        <div class="parallax-overlay"></div>

        <!-- Image counter -->
        <div class="image-counter">
          <span class="current">{{ index + 1 }}</span>
          <span class="total">/ {{ images.length }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, onUnmounted, computed } from "vue";
import gsap from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { ScrollToPlugin } from "gsap/ScrollToPlugin";

gsap.registerPlugin(ScrollTrigger, ScrollToPlugin);

export default {
  name: 'EnhancedScrollGallery',

  props: {
    images: {
      type: Array,
      required: true,
      validator: (value) => value.every(item => typeof item === 'string')
    },
    options: {
      type: Object,
      default: () => ({
        autoplay: false,
        autoplaySpeed: 5000,
        pauseOnHover: true
      })
    }
  },

  setup(props) {
    const imageRefs = ref([]);
    const isLoading = ref(true);
    const visibleImages = ref({});
    const currentImageIndex = ref(0);
    const autoplayInterval = ref(null);

    // Preload images
    const preloadImages = async () => {
      const loadPromises = props.images.map(src => {
        return new Promise((resolve, reject) => {
          const img = new Image();
          img.onload = resolve;
          img.onerror = reject;
          img.src = src;
        });
      });

      try {
        await Promise.all(loadPromises);
        isLoading.value = false;
      } catch (error) {
        console.error('Error loading images:', error);
        isLoading.value = false;
      }
    };

    // Enhanced animation setup
    const setupAnimations = () => {
      imageRefs.value.forEach((image, index) => {
        const timeline = gsap.timeline({
          scrollTrigger: {
            trigger: image,
            start: "top bottom",
            end: "bottom top",
            scrub: 1,
            onEnter: () => {
              visibleImages.value[index] = true;
              currentImageIndex.value = index;
            },
            onLeave: () => {
              visibleImages.value[index] = false;
            },
            onEnterBack: () => {
              visibleImages.value[index] = true;
              currentImageIndex.value = index;
            },
            onLeaveBack: () => {
              visibleImages.value[index] = false;
            }
          }
        });

        // Enhanced animation sequence
        timeline
            .fromTo(image, {
              y: "7%",
              scale: 1.1,
              opacity: 1,
              rotateX: "2deg"
            }, {
              y: "10%",
              scale: 0.8,
              opacity: 1,
              rotateX: "20deg",
              duration: 0.9,
              ease: "power3.out"
            })
            .fromTo(image.querySelector('.parallax-overlay'), {
              opacity: 1,
              scale: 1.1
            }, {
              opacity: 0.3,
              scale: 1,
              duration: 0.8
            }, "-=1")
            .to(image, {
              y: "-30%",
              scale: 0.7,
              opacity: 0,
              rotateX: "20deg",
              duration: 1,
              ease: "power2.in"
            });
      });
    };

    // Smooth scroll to specific image
    const scrollToImage = (index) => {
      const targetImage = imageRefs.value[index];
      if (targetImage) {
        gsap.to(window, {
          duration: 1,
          scrollTo: {
            y: targetImage,
            offsetY: 50
          },
          ease: "power2.inOut"
        });
      }
    };

    // Autoplay functionality
    const startAutoplay = () => {
      if (props.options.autoplay && !autoplayInterval.value) {
        autoplayInterval.value = setInterval(() => {
          const nextIndex = (currentImageIndex.value + 1) % props.images.length;
          scrollToImage(nextIndex);
        }, props.options.autoplaySpeed);
      }
    };

    const stopAutoplay = () => {
      if (autoplayInterval.value) {
        clearInterval(autoplayInterval.value);
        autoplayInterval.value = null;
      }
    };

    // Lifecycle hooks
    onMounted(async () => {
      await preloadImages();
      setupAnimations();
      if (props.options.autoplay) startAutoplay();
    });

    onUnmounted(() => {
      stopAutoplay();
      ScrollTrigger.getAll().forEach(trigger => trigger.kill());
    });

    return {
      imageRefs,
      isLoading,
      visibleImages,
      currentImageIndex,
      scrollToImage,
      startAutoplay,
      stopAutoplay
    };
  }
};
</script>

<style scoped>
.gallery-container {
  position: relative;
  width: 100%;
  overflow: hidden;
}

.scroll-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 100px 0;
  min-height: 100vh;
}

.image-wrapper {
  width: 90vw;
  height: min(900px, 60vw);
  background-size: cover;
  background-position: center;
  border-radius: 20px;
  position: relative;
  opacity: 0;
  transform-style: preserve-3d;
  perspective: 1000px;
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
  transition: transform 0.3s ease;
  overflow: hidden;
}

.image-wrapper:hover {
  transform: scale(1.02);
}

.parallax-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
      45deg,
      rgba(0, 0, 0, 0.4) 0%,
      rgba(0, 0, 0, 0) 100%
  );
  opacity: 0;
}

.image-counter {
  position: absolute;
  bottom: 20px;
  right: 20px;
  color: white;
  font-size: 1.2rem;
  font-weight: 600;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

.nav-dots {
  position: fixed;
  right: 20px;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  flex-direction: column;
  gap: 10px;
  z-index: 10;
}

.nav-dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  border: none;
  cursor: pointer;
  transition: all 0.3s ease;
}

.nav-dot.active {
  background: white;
  transform: scale(1.2);
}

.loading-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.loading-spinner {
  width: 50px;
  height: 50px;
  border: 3px solid transparent;
  border-top-color: white;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

@media (max-width: 768px) {
  .scroll-container {
    gap: 10vh;
    padding: 50px 0;
  }

  .nav-dots {
    right: 10px;
  }

  .image-counter {
    bottom: 10px;
    right: 10px;
    font-size: 1rem;
  }
}
</style>