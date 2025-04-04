<template>
    <div class="carousel">
        <div 
            class="carousel-content"
            :style="{ transform: `translateX(-${currentIndex * 100}%)` }"
            >
            <div 
                v-for="(img, index) in imgs" 
                :key="index" 
                class="carousel-item" 
                >
                <img :src="img" alt="">
            </div>
        </div>
        <button class="carousel-control next" @click="nextSlide"></button>
        <button class="carousel-control prev" @click="prevSlide"></button>
        <div class="pagination">
          <span
            v-for="(img, index) in imgs"
            :class="{'active': currentIndex === index}"
            :key="index"
            @click="goToSlide(index)">
          </span>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'my-carousel',
        data(){
            return{
                currentIndex: 0,
                interval: null
            }
        },
        props: {
            imgs: {
                type: Array,
                default: () => []
            }
        },
        methods: {
            prevSlide() {
                this.currentIndex = (this.currentIndex - 1 + this.imgs.length) % this.imgs.length;
            },
            nextSlide() {
                this.currentIndex = (this.currentIndex + 1) % this.imgs.length;
            },
            goToSlide(index){
              this.currentIndex = index;
            },
            startAutoPlay() {
              this.interval = setInterval(this.nextSlide, 8000);
            },
            stopAutoPlay(){
              clearInterval(this.interval)
            }
        },
        mounted(){
          this.startAutoPlay()
        },
}
</script>

<style scoped>
.carousel {
  position: relative;
  height: 60vh;
  width: 95vw;
  max-width: 1000px;
  max-height: 800px;
  overflow: hidden;
}

.carousel-content{
  display: flex;
  transition: transform 0.5s ease;
  height: 100%;
}

.carousel-item {
  min-width: 100%;
}

.carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background-color: rgba(250, 250, 250, .5);
  color: white;
  border: none;
  padding: 10px;
  cursor: pointer;
  border-radius: 50%;
}

.carousel-control.prev {
  left: 10px;
  background-image: url('../media/carousel_imgs/Arrow_curve_back_point_left-512.png');
}

.carousel-control.next {
  right: 10px;
  background-image: url('../media/carousel_imgs/Arrow_curve_forward_point_right-512.png');
}

.pagination {
  position: absolute;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 5px;
}

.pagination span {
  width: 10px;
  height: 10px;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 50%;
  cursor: pointer;
}

.pagination span.active {
  background-color: white;
}
</style>