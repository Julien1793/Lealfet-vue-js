<template>
    <div>
        <l-map
        :center="center"
        :zoom="zoom"
        :options="{attributionControl: false}"
        class="map"
        ref="map"
        @update:center="centerUpdated"
        @update:zoom="zoomUpdated"
        >
        <LControlAttribution 
        position="bottomright" 
        prefix="Source :"
        >
        </LControlAttribution>
        <l-tile-layer
        :url="url"
        :attribution="attribution"
        :visible="visible"
        :name="name"
        layer-type="base"
        ></l-tile-layer>
        <l-control-layers></l-control-layers>
        <LWMSTileLayer
        v-for="layer in layers"
        :key="layer.name"
        :base-url="layer.baseUrl"
        :layers="layer.layers"
        :visible="layer.visible"
        :name="layer.name"
        :layer-type="layer.lcontrol"
        :attribution="layer.attribution"
        ></LWMSTileLayer>
        <!-- layer-type="overlay" for multiple layer in the map or "base" for single choice -->
        </l-map>
    </div>
</template>
<script>
import { LMap, LWMSTileLayer,LTileLayer, LControlLayers,LControlAttribution } from 'vue2-leaflet';

export default{
    components:{
        LMap,
        LTileLayer,
        LWMSTileLayer,
        LControlLayers,
        LControlAttribution
    },
    data() {
        return {
            center:[48.5532698,1.930517],
            zoom:20,
            name:'OSM',
            url:'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
            attribution:'&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
            visible:'true',

            layers: [
                {
                    name: "ORTHO 20cm",
                    baseUrl: "https://wxs.ign.fr/ortho/geoportail/r/wms",
                    visible: false,
                    format: "image/png",
                    layers: "HR.ORTHOIMAGERY.ORTHOPHOTOS",
                    transparent: true, 
                    attribution: '<a target="_blank" href="https://geoservices.ign.fr/cgu-licences">IGN : Ortho 20cm</a>&#174;',
                    lcontrol:"overlay"
                },
                {
                    name: "ORTHO 1950-1965",
                    baseUrl: "https://wxs.ign.fr/orthohisto/geoportail/r/wms",
                    visible: false,
                    format: "image/png",
                    layers: "ORTHOIMAGERY.ORTHOPHOTOS.1950-1965",
                    transparent: true, 
                    attribution: '<a target="_blank" href="https://geoservices.ign.fr/cgu-licences">IGN : Ortho 1950-1965</a>&#174;',
                    lcontrol:"overlay"
                }
                ]
        };
    },
    methods:{
        centerUpdated(center){
            this.center = center;
        },
        zoomUpdated(zoom){
            this.zoom = zoom;
        },
    },
};

</script>
<style scoped>
.map{
    position: absolute;
    width: 100%;
    height: 100%;
    overflow: hidden;
}
</style>