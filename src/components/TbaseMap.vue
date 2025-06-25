<template>
    <div>
        <div ref="map" class="map-container"></div>
        <a :href="yandexMapsLink" target="_blank" class="open-in-yandex">Открыть в Яндекс.Картах</a>
    </div>
</template>

<script>
export default {
    name: 'YandexMap',
    data() {
        return {
            homeCoordinate: [131.131879, 42.650001],
        };
    },
    computed: {
        yandexMapsLink() {
            const [longitude, latitude] = this.homeCoordinate;
            return `https://yandex.ru/maps/?pt=${longitude},${latitude}&z=15&l=map`;
        },
    },
    methods: {
        async initMap() {
            await ymaps3.ready;

            const { YMap, YMapDefaultSchemeLayer, YMapDefaultFeaturesLayer, YMapMarker } = ymaps3;
            const { YMapDefaultMarker } = await ymaps3.import('@yandex/ymaps3-markers@0.0.1');

            const mapElem = this.$refs.map;

            const map = new YMap(
                mapElem,
                {
                    location: {
                        center: this.homeCoordinate,
                        zoom: 15,
                    },
                }
            );

            map.setMode('vector');

            map.addChild(new YMapDefaultSchemeLayer());
            map.addChild(new YMapDefaultFeaturesLayer());

            const marker = new YMapDefaultMarker({
                coordinates: this.homeCoordinate,
                title: 'Турбаза',
                color: '#1a73e8'
            });

            map.addChild(marker);
        },
    },
    async mounted() {
        await this.initMap();
    },
};
</script>

<style scoped>
.map-container {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-content: center;
    flex-wrap: wrap;
    align-items: center;
    width:100%;
    height:100%;
}
.open-in-yandex {
    display: block;
    margin-top: 10px;
    text-align: center;
    color: blue;
    text-decoration: underline;
    cursor: pointer;
}
</style>