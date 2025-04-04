<template>
    <div>
        <my-carousel 
            :imgs="images"
        />
    </div>
</template>

<script>
    export default {
        data() {
            return{
                images: [
                    require("@/components/media/base_imgs/Shkota_island.jpg"),
                    require("@/components/media/base_imgs/Tokareva_s_cat.jpg"),
                    require("@/components/media/base_imgs/Yacht_bay.jpg")
                ]
            }
        }
    }
</script>

<style scoped>

</style>