<script>
	import { onMount } from 'svelte';
    import {Map, setWorkerUrl} from 'maplibre-gl';
    import { WarpedMapLayer } from '@allmaps/maplibre'
    import 'maplibre-gl/dist/maplibre-gl.css';
    import workerUrl from 'maplibre-gl/dist/maplibre-gl-worker.mjs?worker&url';

    setWorkerUrl(workerUrl);

	onMount(() => {
		let map = new Map({
			container: 'map',
			style: 'https://basemaps.cartocdn.com/gl/voyager-gl-style/style.json',
			center: [-73.9337, 40.8011],
			zoom: 11.5,
			maxPitch: 0
		});

		const annotationUrl = 'https://annotations.allmaps.org/images/d180902cb93d5bf2';
		const warpedMapLayer = new WarpedMapLayer();

		map.on('load', () => {
			map.addLayer(warpedMapLayer);
			warpedMapLayer.addGeoreferenceAnnotationByUrl(annotationUrl);
		});
		
		return () => {
			map?.remove();
		};
	});
</script>

<div id="map" class="w-full h-lvh"></div>