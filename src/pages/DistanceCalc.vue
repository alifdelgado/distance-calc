<template>
  <main>
    <origin-destination-form/>
    <route-list/>
    <directions-map-view/>
  </main>
</template>

<script>
import RouteList from "@/components/RouteList";
import DirectionsMapView from "@/components/DirectionsMapView";
import OriginDestinationForm from "@/components/OriginDestinationForm";
export default {
  components: {
    RouteList,
    DirectionsMapView,
    OriginDestinationForm,
  }
}
</script>

<style>

</style>
