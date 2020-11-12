<template class="pepega">
  <nav class="navbar navbar-light bg-light navbar-expand-lg">
    <div class="container-fluid">
      <a class="navbar-brand" href="#"> Amused to Death </a>
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarNav"
        aria-controls="navbarNav"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav ml-auto">
          <li class="nav-item">
            <button class="btn btn-outline-secondary mr-2" type="button" @click="darkMode = !darkMode">Toggle darkmode</button>
          </li>
          <li class="nav-item">
            <button class="btn btn-outline-secondary mr-2" type="button" @click="pepegaTimeformat = !pepegaTimeformat">24H/12H</button>
          </li>
          <li class="nav-item">
            <button class="btn btn-outline-secondary mr-2" type="button" @click="sortedList = !sortedList">Toggle sort</button>
          </li>
          <li class="nav-item" v-for="link in links" :key="link.id">
            <a
              class="nav-link"
              :class="link.disabled ? 'disabled' : ''"
              aria-current="page"
              :href="link.href"
              :aria-disabled="link.disabled"
              :target="link.target"
              >{{ link.name }}</a
            >
          </li>
        </ul>
      </div>
    </div>
  </nav>

  <h1 class="my-4 py-4">Server Time: {{ currentTime }}</h1>
  <div aria-live="polite" aria-atomic="true" style="position: fixed; min-height: 100vh; width: 100%">
    <div
      class="toast d-flex align-items-center"
      id="clipboard"
      role="alert"
      aria-live="assertive"
      aria-atomic="true"
      style="position: absolute; top: 0; right: 0"
    >
      <div class="toast-body">Copied waypoint to clipboard!</div>
      <button type="button" class="btn-close ml-auto mr-2" data-dismiss="toast" aria-label="Close"></button>
    </div>
  </div>
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
          <div class="mr-2">
            <button
              class="btn mr-2"
              v-bind:class="{ 'btn-outline-light': boss.active, 'btn-outline-primary': !boss.active }"
              type="button"
              @click="clipboard(boss.waypoint)"
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
  name: 'App',
  components: {},
  mounted() {
    this.updateTime();
    setInterval(this.updateTime, 1000);
    this.updateRotation();
  },
  data() {
    return {
      darkMode: true,
      pepegaTimeformat: false,
      sortedList: false,
      timeBetweenBossSpawns: 20,
      currentTime: '__:__:__',
      links: [
        {
          id: 1,
          name: 'Source-code',
          href: 'https://github.com/kristoffer-tvera/prepatch',
          disabled: false,
          target: '_blank',
        },
        {
          id: 2,
          name: 'Issues',
          href: 'https://github.com/kristoffer-tvera/prepatch/issues',
          disabled: false,
          target: '_blank',
        },
        {
          id: 3,
          name: 'Donate',
          href: 'https://www.savethechildren.org/',
          disabled: false,
          target: '_blank',
        },
      ],
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
    };
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
    pepegaTimeformat: {
      handler: function () {
        this.updateRotation();
      },
    },
    darkMode: {
      handler: function () {
        document.body.classList.toggle('darkmode');
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
      let date = new Date();
      let hour = (date.getUTCHours() + (1 % 24)).toString().padStart(2, '0');
      let minute = date.getUTCMinutes().toString().padStart(2, '0');
      let seconds = date.getUTCSeconds().toString().padStart(2, '0');

      let suffix = '';

      if (this.pepegaTimeformat) {
        suffix = date.getUTCHours() + (1 % 24) < 12 ? 'AM' : 'PM';
        hour = ((date.getUTCHours() + 1) % 12).toString().padStart(2, '0');
      }

      if (seconds === '00') {
        this.updateRotation();
      }

      this.currentTime = `${hour}:${minute}:${seconds} ${suffix}`;
    },

    updateRotation() {
      let epoch = Date.UTC(2020, 10, 11, 8, 0, 0);
      let nowUtc = new Date().getTime();
      let offsetMilliseconds = nowUtc - epoch;
      let offsetSeconds = Math.floor(offsetMilliseconds / 1000);
      let offsetMinutes = Math.floor(offsetSeconds / 60);
      let offsetHours = Math.floor(offsetMinutes / 60);
      let currentRotation = Math.floor(offsetMinutes / 20) % this.rotation.length;

      let currentMinute = Math.ceil((offsetMinutes % 60) / 20) * 20;
      let currentHour = (offsetHours + 9) % 24;
      for (let i = 0; i < this.rotation.length; i++) {
        let index = (i + currentRotation) % this.rotation.length;

        let minute = currentMinute + i * 20;
        let minuteToString = (minute % 60).toString().padStart(2, '0');

        let hour = (currentHour + Math.floor(minute / 60)) % 24;
        let suffix = '';

        if (this.pepegaTimeformat) {
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

    clipboard(input) {
      navigator.clipboard.writeText(input);

      var el = document.querySelector('#clipboard');
      console.log(el);
      let toast = eval('new bootstrap.Toast(el)');
      toast.show();
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.special-loot {
  max-height: 35px;
}

.darkmode {
  background-color: #232323;
  h1 {
    color: #fff;
  }
  .bg-light {
    background-color: #232323 !important;
    color: #fff;
  }
  .navbar-brand {
    color: #fff;
  }
  .navbar-nav {
    .nav-link {
      color: #fff;
    }
  }
  .list-group-item {
    background-color: #696969;
    color: #fff;
  }
}
</style>


