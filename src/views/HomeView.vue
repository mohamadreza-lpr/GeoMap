<template>
    <div>
        <l-map @click="drawCircle($event)" ref="myMap" :zoom="zoom" :center="center" style="height: 500px; width: 100%">
            <l-tile-layer :url="url" :attribution="attribution" />

            <l-marker ref="myMarker" v-for="item in points" :key="item.id" :lat-lng="item.point" :icon="item.isInside ? redIcon : icon">
                <!-- <l-icon :icon-size="dynamicSize" :icon-anchor="dynamicAnchor" icon-url="src/assets/blueIcon.png"> </l-icon> -->
            </l-marker>

            <l-circle v-if="circleShow" ref="myCircle" :lat-lng="circle.center" :radius="circle.radius" />
        </l-map>
    </div>
</template>

<script>
import { latLng, LatLng } from "leaflet";
import { LMap, LTileLayer, LCircle, LRectangle, LPolygon, LPolyline, LMarker, LPopup, LIcon } from "vue2-leaflet";
import json from "./Data.json";

export default {
    name: "GeometryTest",
    components: {
        LMap,
        LTileLayer,
        LCircle,
        LRectangle,
        LPolygon,
        LPolyline,
        LMarker,
        LPopup,
        LIcon,
    },
    data() {
        return {
            circleShow: false,
            points: json,
            zoom: 11,
            center: [47.31322, -1.319482],
            circle: {
                center: latLng(47.41322, -1.0482),
                radius: 8000,
            },
            icon: L.icon({
                iconUrl: require("../assets/blueIcon.png"),
                iconSize: [32, 37],
                iconAnchor: [16, 37],
            }),
            redIcon: L.icon({
                iconUrl: require("../assets/redIcon.png"),
                iconSize: [32, 37],
                iconAnchor: [16, 37],
            }),
            isInside: false,
            iconSize: 64,
            url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
            attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
        };
    },
    methods: {
        clickBtn() {
            this.rectangle.style.weight++;
            this.rectangle.style.color = this.rectangle.style.weight % 2 === 0 ? "blue" : "green";
        },
        drawCircle(event) {
            this.circleShow = true;
            this.circle.center = event.latlng;
            this.points = this.points;
            for (var point of this.points) {
                let d = this.$refs.myMap.mapObject.options.crs.distance(latLng(point.point[0], point.point[1]), this.circle.center);
                let isInside = d < this.circle.radius;
                point.isInside = isInside;
            }
        },
    },
};
</script>
