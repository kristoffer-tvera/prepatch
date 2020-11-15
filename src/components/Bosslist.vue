<template>
  <div class="container my-4 py-4">
    <div class="list-group">
      <div
        class="list-group-item list-group-item-action"
        v-bind:class="{ active: boss.active }"
        :aria-current="boss.active"
        v-for="boss in sortedRotation"
        :key="boss.id"
        :data-sort-order="boss.sortOrder"
      >
        <div class="d-flex w-100 justify-content-between" title="Click to copy waypoint">
          <h5 class="mb-1">{{ boss.name }}</h5>
          <div class="ml-auto"></div>
          <div v-if="boss.id == 17" class="mr-2">
            <img src="@/assets/mount.jpg" alt="mount pic" class="special-loot" />
          </div>
          <div v-if="boss.id == 1" class="mr-2">
            <img src="@/assets/bag.jpg" alt="mount pic" class="special-loot" />
          </div>
          <div class="mr-2 d-none d-md-block">
            <button
              class="btn mr-2"
              v-bind:class="{ 'btn-outline-light': boss.active, 'btn-outline-primary': !boss.active }"
              type="button"
              @click="$emit('clipboard', boss.waypoint)"
            >
              Copy waypoint
            </button>
          </div>
          <small>{{ boss.nextSpawn }}</small>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Bosslist',
  props: ['pepega-time-format', 'sorted-list', 'european'],
  data() {
    return {
      rotation: [
        { id: 0, name: 'The Black Knight', waypoint: '/way 64,8, 22,1', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 1, name: 'Bronjahm', waypoint: '/way 70,7, 38,4', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 2, name: 'Scourgelord Tyrannus', waypoint: '/way 47,2, 66,1', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 3, name: 'Forgemaster Garfrost', waypoint: '/way 58,6, 72,5', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 4, name: 'Marwyn', waypoint: '/way 58,2, 83,4', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 5, name: 'Falric', waypoint: '/way 50,2, 87,9', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 6, name: "The Prophet Tharon'ja", waypoint: '/way 80,1, 61,2', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 7, name: 'Novos the Summoner', waypoint: '/way 77,8, 66,1', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 8, name: 'Trollgore', waypoint: '/way 58,3, 39,4', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 9, name: "Krik'thir the Gatewatcher", waypoint: '/way 67,5, 58,0', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 10, name: 'Prince Taldaram', waypoint: '/way 29,6, 62,2', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 11, name: 'Elder Nadox', waypoint: '/way 44,2, 49,1', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 12, name: 'Noth the Plaguebringer', waypoint: '/way 31,6, 70,5', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 13, name: 'Patchwerk', waypoint: '/way 36,5, 67,4', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 14, name: "Blood Queen Lana'thel", waypoint: '/way 49,7, 32,7', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 15, name: 'Professor Putricide', waypoint: '/way 57,1, 30,3', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 16, name: 'Lady Deathwhisper', waypoint: '/way 51,1, 78,5', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 17, name: 'Skadi the Ruthless', waypoint: '/way 57,8, 56,1', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 18, name: 'Ingvar the Plunderer', waypoint: '/way 52,4, 52,6', active: false, nextSpawn: '', sortOrder: -1 },
        { id: 19, name: 'Prince Keleseth', waypoint: '/way 54,0, 44,7', active: false, nextSpawn: '', sortOrder: -1 },
      ],
      timeBetweenBossSpawns: 20,
      timeOffset: this.european ? 9 : 0,
      bossOffset: this.european ? 0 : 16,
    };
  },
  mounted() {
    setInterval(this.updateTime, 1000);
    this.updateRotation();
  },
  computed: {
    sortedRotation: function () {
      if (this.sortedList) {
        let copy = this.rotation.slice();
        return copy.sort(this.compare);
      } else {
        return this.rotation;
      }
    },
  },
  watch: {
    pepegaTimeFormat: {
      handler: function () {
        this.updateRotation();
      },
    },
    european: {
      handler: function () {
          if(this.european){
              this.timeOffset = 9;
              this.bossOffset = 0;
          } else {
              this.timeOffset = 0;
              this.bossOffset = 16;
          }
        this.updateRotation();
      },
    },
  },
  methods: {
    compare(a, b) {
      if (a.sortOrder < b.sortOrder) return -1;
      if (a.sortOrder > b.sortOrder) return 1;
      return 0;
    },

    updateTime() {
      if (new Date().getSeconds === 0) {
        this.updateRotation();
      }
    },

    updateRotation() {
        let epoch = Date.UTC(2020, 10, 11, 8, 0, 0);
      let nowUtc = new Date().getTime();
      let offsetMilliseconds = nowUtc - epoch;
      let offsetSeconds = Math.floor(offsetMilliseconds / 1000);
      let offsetMinutes = Math.floor(offsetSeconds / 60);
      let offsetHours = Math.floor(offsetMinutes / 60);
      let currentRotation = (Math.floor(offsetMinutes / this.timeBetweenBossSpawns) + this.bossOffset) % this.rotation.length;

      let currentMinute = Math.ceil((offsetMinutes % 60) / this.timeBetweenBossSpawns) * this.timeBetweenBossSpawns;
      let currentHour = (offsetHours + this.timeOffset) % 24;
      for (let i = 0; i < this.rotation.length; i++) {
        let index = (i + currentRotation) % this.rotation.length;

        let minute = currentMinute + i * this.timeBetweenBossSpawns;
        let minuteToString = (minute % 60).toString().padStart(2, '0');

        let hour = (currentHour + Math.floor(minute / 60)) % 24;
        let suffix = '';

        if (this.pepegaTimeFormat) {
          suffix = hour < 12 ? 'AM' : 'PM';
          hour = hour % 12;
        }

        let hourToString = hour.toString().padStart(2, '0');

        this.rotation[index].nextSpawn = `${hourToString}:${minuteToString} ${suffix}`;

        this.rotation[index].sortOrder = i;
        this.rotation[i].active = false;
      }
      this.rotation[currentRotation].active = true;
    },
  },
  emits: ['clipboard'],
};
</script>
