<template>
    <div>
        <button @click="clickBtn">Change rectangle style</button>
        <l-map @click="drawCircle($event)" :zoom="zoom" :center="center" style="height: 500px; width: 100%">
            <l-tile-layer :url="url" :attribution="attribution" />

            <l-marker v-for="item in points" :key="item.id" :lat-lng="item.point"></l-marker>

            <l-circle :lat-lng="circle.center" :radius="circle.radius" />
        </l-map>
    </div>
</template>

<script>
import { latLng } from "leaflet";
import { LMap, LTileLayer, LCircle, LRectangle, LPolygon, LPolyline, LMarker, LPopup } from "vue2-leaflet";
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
    },
    data() {
        return {
            points: json,
            zoom: 11,
            center: [47.31322, -1.319482],
            circle: {
                center: latLng(47.41322, -1.0482),
                radius: 8000,
            },
            icon: L.icon({
                iconUrl: "/images/baseball-marker.png",
                iconSize: [32, 37],
                iconAnchor: [16, 37],
            }),
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
            console.log(event);
            this.circle.center = event.latlng;
        },
    },
};
</script>
