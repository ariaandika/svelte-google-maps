<script>
import { Loader }from "@googlemaps/js-api-loader"
import { onMount } from "svelte";

const apiKey = "AIzaSyCBzOJlmj8f36DRTwp7wtluj4v6tOVmTME"

/** @type {HTMLElement} */
let mapElem

onMount(async () => {
    const loader = new Loader({ apiKey, version: "weekly", libraries: ["maps","marker"] })

    const pos = {lat: 37.39094933041195, lng: -122.02503913145092}

    if (!navigator.geolocation) { return console.log("No geo ?") }

    navigator.geolocation.getCurrentPosition(
        (position) => {
            pos.lat = position.coords.latitude
            pos.lng = position.coords.longitude

            // infoWindow.setPosition(pos);
            // infoWindow.setContent("Location found.");
            // infoWindow.open(map);
            // map.setCenter(pos);
        },
        (er) => console.error(er),
        { enableHighAccuracy: true }
    );

    const { Map, InfoWindow } = await loader.importLibrary("maps")
    const { AdvancedMarkerElement } = await loader.importLibrary("marker")
    
    const map = new Map(mapElem, {
        center: pos,
        zoom: 14,
        mapId: '4504f8b37365c3d0',
    });

    const infoWindow = new InfoWindow();
    const draggableMarker = new AdvancedMarkerElement({
        map,
        position: pos,
        gmpDraggable: true,
        title: "This marker is draggable.",
    });

    draggableMarker.addListener('dragend', () => {
        const position = draggableMarker.position;
        infoWindow.close();
        infoWindow.setContent(`Pin dropped at: ${position.lat}, ${position.lng}`);
        infoWindow.open(draggableMarker.map, draggableMarker);
    });
})


</script>

<div bind:this={mapElem} class="w-full h-full"></div>

