<template>
  <div class="steamMapBox">
    <div class="title">
      <i class="fas fa-circle-notch"></i>설비별 위치
    </div>

    <div class="steams">
      <img src="@/assets/sewangMap.png" alt="">

      <div class="spot-box" v-if="store.state.steam.mapkey === 1">
        <div class="spot"  v-for="(spot,i) in store.state.steam.steamMapbox" :key="i" :class="[{selected : spot.select},{warn : spot.status === '이상'},{err : spot.status === '고장'}, `spot${spot.key}`]"  @click="clickTrap(spot.id)">
        <a-tooltip :title="spot.title" trigger="hover" v-if="!spot.select">
          <i class="fas fa-map-marker-alt"></i>
        </a-tooltip>
        <a-tooltip :title="spot.title" :visible="spot.select" v-else>
          <i class="fas fa-map-marker-alt"></i>
        </a-tooltip>
        </div>
      </div>

      <div class="spot-box" v-else>
        <div class="spot"  v-for="(spot,i) in store.state.steam.elecMapbox" :key="i" :class="[{selected : spot.select},{warn : spot.status === '이상'},{err : spot.status === '고장'}, `spot${spot.key}`]"  @click="clickElec(spot.id)">
        <a-tooltip :title="spot.title" trigger="hover" v-if="!spot.select">
          <i class="fas fa-map-marker-alt"></i>
        </a-tooltip>
        <a-tooltip :title="spot.title" :visible="spot.select" v-else>
          <i class="fas fa-map-marker-alt"></i>
        </a-tooltip>
        </div>
      </div>
    </div>
      <div class="resetBtn" @click="resetView">
        <i class="fas fa-undo reset"></i>
        원래대로
      </div>
  </div>
</template>

<script>
import { useStore } from "vuex";

export default {
emits: ["click-steam", "click-elec"],
setup(props, { emit }) {
    const store = useStore();

    const clickTrap = (value) => {
      store.commit("steam/steamTapSelect", value);
      store.commit("steam/steamTrapMapSelect", value);
      store.commit("steam/steamTrapAlign");
      emit("click-steam", value);
    };
    const clickElec = (value) => {
      store.commit("steam/elecTapSelect", value);
      store.commit("steam/elecTrapMapSelect", value);
      store.commit("steam/elecTrapAlign");
      emit("click-elec", value);
    };

    const resetView = () => {
      emit("click-steam", 100);
      store.commit("steam/resetSelect");
    };

    return{
      store,
      clickTrap,
      clickElec,
      resetView
    };
    }

};
</script>

<style>

</style>
