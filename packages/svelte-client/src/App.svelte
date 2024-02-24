<script>
import { Loader }from "@googlemaps/js-api-loader"
import { onMount } from "svelte";

const apiKey = "AIzaSyCBzOJlmj8f36DRTwp7wtluj4v6tOVmTME"

/** @type {HTMLElement} */
let mapElem

onMount(async () => {
    const loader = new Loader({ apiKey, version: "weekly", libraries: ["maps","marker"] })

    const { Map, InfoWindow } = await loader.importLibrary("maps")
    const { AdvancedMarkerElement } = await loader.importLibrary("marker")
    
    const map = new Map(mapElem, {
        center: {lat: 37.39094933041195, lng: -122.02503913145092},
        zoom: 14,
        mapId: '4504f8b37365c3d0',
    });

    const infoWindow = new InfoWindow();
    const draggableMarker = new AdvancedMarkerElement({
        map,
        position: {lat: 37.39094933041195, lng: -122.02503913145092},
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

<!-- <iframe -->
<!--     title="oof" -->
<!--     width="600" -->
<!--     height="450" -->
<!--     style="border:0" -->
<!--     loading="lazy" -->
<!--     allowfullscreen -->
<!--     referrerpolicy="no-referrer-when-downgrade" -->
<!--     src="https://www.google.com/maps/embed/v1/place?key={apikey}&q=Space+Needle,Seattle+WA"> -->
<!-- </iframe> -->




