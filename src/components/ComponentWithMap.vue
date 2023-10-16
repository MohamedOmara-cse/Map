<template>
	<div>
		<GMapMap
			:center="center"
			:zoom="10"
			ref="map"
			:click="true"
			map-type-id="terrain"
			style="width: 100vw; height: 20rem"
			@dragend="MapDragend"
		>
			<GMapCluster :zoomOnClick="true">
				<GMapMarker
					v-for="(m, index) in markers"
					:key="index"
					:position="m"
					:draggable="true"
					@dragend="onMarkerDragend(index, $event)"
				></GMapMarker>
			</GMapCluster>
		</GMapMap>
		<div>
			<input v-model="newLat" type="number" placeholder="Latitude" />
			<input v-model="newLng" type="number" placeholder="Longitude" />
			<button @click="addMarker">Add Marker</button>
		</div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			center: {},
			markers: [],
			newLat: "",
			newLng: "",
		};
	},
	methods: {
		MapDragend(e) {
			const location = {
				lat: this.$refs.map.$mapObject.center.lat(),
				lng: this.$refs.map.$mapObject.center.lng(),
			};
			console.log(location);
		},
		onMarkerDragend(index, event) {
			const location = {
				lat: event.latLng.lat(),
				lng: event.latLng.lng(),
			};
			console.log(location);
			this.newLat = location.lat;
			this.newLng = location.lng;
		},
		addMarker() {
			if (this.newLat && this.newLng) {
				this.markers = [
					{
						lat: parseFloat(this.newLat),
						lng: parseFloat(this.newLng),
					},
				];
				this.center = {
					lat: parseFloat(this.newLat),
					lng: parseFloat(this.newLng),
				};
			}
		},
	},
	mounted() {
		if ("geolocation" in navigator) {
			navigator.geolocation.getCurrentPosition((position) => {
				this.center = {
					lat: position.coords.latitude,
					lng: position.coords.longitude,
				};
				this.markers.push(this.center);
				this.newLat = this.center.lat;
				this.newLng = this.center.lng;
			});
		}
	},
};
</script>

<style>
body {
	margin: 0;
}
</style>
