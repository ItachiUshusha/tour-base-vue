<template>
  <TbaseNav/>
  <div class="main">
    <TbaseGallery class="gallery" id="gallery"/>
    <TbaseCalc class="calc" id="calculation"/>
    <TbaseMap class="map" id="homeMap"/>
  </div>
</template>

<script>
  import TbaseNav from '@/components/TbaseNav.vue'
  import TbaseCalc from '@/components/TbaseCalc.vue';
  import TbaseGallery from '@/components/TbaseGallery.vue';
  import TbaseMap from '@/components/TbaseMap.vue';
  export default {
    components:{
      TbaseNav,
      TbaseCalc,
      TbaseGallery,
      TbaseMap
    }
  }

</script>

<style>
.main{
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  flex-direction: column;
  gap: 5vh;
}

.gallery{
  margin-top: 2vh;
}

.map{
    height: 60vh;
    width: 95vw;
    max-width: 1000px;
    max-height: 800px;
}

.calc{
  width:100%;
}
</style>
