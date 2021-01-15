<template>
<section class="route-list-view">
  <div class="route-list-view-header">
    <h3 class="ui header">Route List</h3>
    <select @change="sortRoute($event)">
      <option selected disabled>Sort</option>
      <optgroup label="Distance">
        <option value="distance-asc">short</option>
        <option value="distance-desc">long</option>
      </optgroup>
      <optgroup label="Duration">
        <option value="duration-asc">fast</option>
        <option value="duration-desc">slow</option>
      </optgroup>
    </select>
    <button type="button" class="ui button show-all" @click="showAllRoutesButtonPressed()">Show All</button>
  </div>
  <div class="item" v-for="route in routes" :key="route.id" @click="routeItemPressed(route)" :style="{'background-color': route.color}">
    <div><i class="marker alternate icon"></i><span v-text="route.origin.address"></span></div>
    <div><i class="flag checkered icon"></i><span v-text="route.destination.address"></span></div>
    <div class="ui label small" v-text="route.distance.text"></div>
    <div class="ui label small" v-text="route.duration.text"></div>
  </div>
</section>
</template>

<script>
import firebase from 'firebase/app';
import "firebase/firestore";
import { EventBus } from "@/EventBus";
export default {
  data() {
    return {
      routes: [],
    };
  },
  created() {
    const db = firebase.firestore();
    db.collection("routes").onSnapshot(snap => {
      this.pushRoutes(snap);
    });
  },
  methods: {
    sortRoute(e){
      const sort = e.target.value.split('-');
      const sortName = sort[0];
      const sortOrder = sort[1];
      const db = firebase.firestore();
      db.collection("routes")
        .orderBy(sortName + ".value", sortOrder)
        .get()
        .then(snap => {
          this.pushRoutes(snap);
        });
    },
    routeItemPressed(route){
      EventBus.$emit("routes-data", [route]);
    },
    showAllRoutesButtonPressed(){
      EventBus.$emit("routes-data", this.routes);
    },
    pushRoutes(snap) {
      this.routes = [];
      snap.forEach(doc => {
        let route = doc.data();
        route.id = doc.id;
        this.routes.push(route);
      });
    }
  },
}
</script>

<style>
.route-list-view{
  position: relative;
  z-index: 1;
  max-width: 250px;
  margin: 10px;
  background-color: #fff;
}

.route-list-view-header {
  padding: 10px;
}

.item{
  padding: 10px;
  cursor: pointer;
}

.item:hover {
  background-color: rgba(0,0,0,0.1);
}

.show-all {
  padding: 4px 10px;
}
</style>
