<template>
  <div class="showcase-container">
    <div class="showcase-slider">
      <div
        v-for="(slide, index) in showcaseSlides"
        :key="index"
        class="showcase-slide"
        v-show="currentSlide === index"
      >
        <div class="split-view" :class="{ 'single-image': !slide.sketchImage }">
          <div class="sketch-artwork" v-if="slide.sketchImage">
            <div class="image-container">
              <img :src="slide.sketchImage" alt="Sketch" />
            </div>
          </div>
          <div class="final-artwork">
            <div class="image-container">
              <img :src="slide.finalImage" alt="Final Artwork" />
            </div>
          </div>
          <a href="#" class="follow-btn">Follow <i class="follow-arrow">→</i></a>
          <div class="designer-credit" v-if="slide.designer">
            <div class="name">{{ slide.designer.name }}</div>
            <div class="title">{{ slide.designer.title }}</div>
          </div>
        </div>
        <div class="slide-description" v-if="!slide.designer && (slide.title || slide.description)">
          <h3 v-if="slide.title">{{ slide.title }}</h3>
          <p v-if="slide.description">{{ slide.description }}</p>
        </div>
      </div>
    </div>
    <div class="showcase-dots">
      <span
        v-for="(_, index) in showcaseSlides"
        :key="index"
        :class="{ active: currentSlide === index }"
        @click="switchSlide(index)"
      ></span>
    </div>
  </div>
</template>

<script setup lang="ts">
  // Define types for the showcase slides
  interface Designer {
    name: string
    title: string
  }

  interface ShowcaseSlide {
    finalImage: string
    sketchImage: string
    designer?: Designer
    title?: string
    description?: string
  }

  // 展示图片配置 - 使用分屏对比效果
  const showcaseSlides = ref<ShowcaseSlide[]>([
    {
      finalImage: 'https://images.unsplash.com/photo-1567899378494-47b22a2ae96a', // Porsche render
      sketchImage: '', // Car sketch
      designer: {
        name: 'Eric Stoddard',
        title: 'CARDESIGN.ACADEMY Founder and President'
      }
    },
    {
      finalImage: 'https://images.unsplash.com/photo-1580274455191-1c62238fa333', // Audi concept
      sketchImage: '', // Empty sketch to demonstrate single image view
      designer: {
        name: 'David Carson',
        title: 'Senior Automotive Designer'
      }
    },
    {
      finalImage: 'https://images.unsplash.com/photo-1503736334956-4c8f8e92946d', // Ferrari
      sketchImage: '', // Car sketches
      designer: {
        name: 'Maria Chen',
        title: 'Industrial Design Director'
      }
    },
    {
      finalImage: 'https://images.unsplash.com/photo-1552519507-da3b142c6e3d', // Corvette
      sketchImage: '', // Car design sketch
      designer: {
        name: 'James Rodriguez',
        title: 'Concept Vehicle Designer'
      }
    },
    {
      finalImage: 'https://images.unsplash.com/photo-1618843479313-40f8afb4b4d8', // Racing car
      sketchImage: '', // No sketch for this one
      designer: {
        name: 'Akos Szaz',
        title: 'Car Designer'
      }
    }
  ])

  const currentSlide = ref(0)

  let slideInterval: ReturnType<typeof setInterval> | null = null

  // 手动切换
  const switchSlide = (index: number) => {
    // 停止当前计时器
    if (slideInterval) {
      clearInterval(slideInterval)
      slideInterval = null
    }

    // 先移除动画类
    const dots = document.querySelectorAll('.showcase-dots span')
    dots.forEach((dot) => {
      dot.classList.remove('active')
    })

    // 稍微延迟后再设置新的幻灯片和动画类，以确保动画能重新触发
    setTimeout(() => {
      // 设置新的幻灯片索引
      currentSlide.value = index

      // 添加动画类到当前激活的点
      if (dots[index]) {
        dots[index].classList.add('active')
      }

      // 重新开始轮播
      startSlideShow()
    }, 50)
  }

  // 开始自动轮播
  const startSlideShow = () => {
    // 确保不会创建多个计时器
    if (slideInterval) {
      clearInterval(slideInterval)
    }

    // 创建新计时器
    slideInterval = setInterval(() => {
      // 先移除动画类
      const dots = document.querySelectorAll('.showcase-dots span')
      dots.forEach((dot) => {
        dot.classList.remove('active')
      })

      // 移动到下一个幻灯片
      const nextSlide = (currentSlide.value + 1) % showcaseSlides.value.length

      // 设置新的幻灯片索引
      currentSlide.value = nextSlide

      // 添加动画类到当前激活的点
      setTimeout(() => {
        if (dots[nextSlide]) {
          dots[nextSlide].classList.add('active')
        }
      }, 50)
    }, 5000)
  }

  // 组件挂载时开始轮播
  onMounted(() => {
    startSlideShow()
  })

  // 组件卸载时清除计时器
  onUnmounted(() => {
    if (slideInterval) {
      clearInterval(slideInterval)
      slideInterval = null
    }
  })
</script>

<style lang="scss" scoped>
  @use '@/views/login/index.scss' as login;
</style>
