<template>
  <nav class="nav__bar">
    <div class="nav-header">
      <h4 class="nav-title">Турбаза в Андреевке</h4>
      <div class="nav-menu" :class="{ 'active': isMenuOpen }">
        <div class="nav-elem" v-for="elem in navElem" :key="elem.name" @click="scrollTo(elem.name)">
          <span class="nav-icon" v-html="elem.icon"></span>
          <span class="nav-text">{{ elem.title }}</span>
        </div>
      </div>
      <div class="burger-menu" @click="toggleMenu" :class="{ 'active': isMenuOpen }">
        <span></span>
        <span></span>
        <span></span>
      </div>
    </div>
  </nav>
</template>

<script>
import { handleError } from 'vue';

export default {
  data() {
    return {
      isMenuOpen: false,
      navElem: [
        {name: 'about', title: "О нас", icon: '&hearts;' },
        {name: 'calculation', title: "Заказать комнату", icon: '&#128719;' },
        {name: 'price', title: "Цены", icon: '&#128178;' },
        {name: 'homeMap', title: "Где нас найти", icon: '&#128205;' }
      ]
    }
  },
  created() {
    window.addEventListener('scroll',  this.handleScroll)
  },
  unmounted() {
    window.removeEventListener('scroll', this.handleScroll);
  },
  methods: {
    scrollTo(elem) {
      const element = document.getElementById(elem)
      if(element) {
        element.scrollIntoView({behavior: 'smooth'})
        this.isMenuOpen = false
      }
    },
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen
    },
    handleScroll() {
      this.isMenuOpen = false
    }
  }
}
</script>

<style scoped>
.nav__bar {
  border-bottom: 2px solid rgba(11, 65, 182, 0.619);
  width: 100%;
  background: linear-gradient(90deg, #f8f9fa 0%, #ffffff 100%);
}

.nav-header {
  display: flex;
  justify-content: space-between; /* Распределяет пространство между элементами */
  align-items: center;
  padding: 1vh 2vw;
  max-width: 1200px;
  margin: 0 auto;
  position: relative;
}

.nav-title {
  cursor: default;
  font-size: calc(1.2rem + 0.5vw);
  color: #0b41b6;
  text-shadow: 1px 1px 2px rgba(0,0,0,0.1);
  white-space: nowrap;
  margin-right: auto; /* Прижимает к левому краю */
}

.nav-menu {
  display: flex;
  justify-content: flex-end;
  flex-grow: 1;
  margin-left: 20px;
}

.nav-elem {
  display: flex;
  align-items: center;
  font-size: calc(0.8rem + 0.5vw);
  padding: 0.8vh 1.5vw;
  cursor: pointer;
  margin: 0 0.3vw;
  border-radius: 25px;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
  white-space: nowrap;
}

.burger-menu {
  display: none;
  flex-direction: column;
  justify-content: space-between;
  width: 30px;
  height: 21px;
  cursor: pointer;
  z-index: 100;
  margin-left: 20px;
}

.burger-menu span {
    display: block;
    height: 3px;
    width: 100%;
    background: #0b41b6;
    border-radius: 3px;
    transition: all 0.3s ease;
  }

.burger-menu.active span:nth-child(1) {
  transform: translateY(9px) rotate(45deg);
}

.burger-menu.active span:nth-child(2) {
  opacity: 0;
}

.burger-menu.active span:nth-child(3) {
  transform: translateY(-9px) rotate(-45deg);
}

/* Мобильная версия */
@media (max-width: 768px) {
  .nav-header {
      padding: 1vh 4vw;
  }
  
  .nav-title {
      font-size: calc(1.2rem + 1vw);
  }
  
  .burger-menu {
      display: flex;
  }
  
  .nav-menu {
      position: fixed;
      top: 80px;
      left: 0;
      right: 0;
      width: 100%;
      background: rgba(255, 255, 255, 0.98);
      flex-direction: column;
      align-items: stretch;
      max-height: 0;
      overflow: hidden;
      box-shadow: 0 4px 6px rgba(0,0,0,0.1);
      transition: max-height 0.3s ease, opacity 0.2s ease;
      opacity: 0;
      padding: 0;
      margin: 0;
      z-index: 100;
  }
  
  .nav-menu.active {
      max-height: 100vh;
      opacity: 1;
      padding: 2vh 0;
  }
  
  .nav-elem {
      width: auto;
      margin: 0 10px;
      padding: 12px 0;
      text-align: center;
      justify-content: center;
      font-size: calc(1rem + 0.5vw);
  }
}
</style>