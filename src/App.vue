<template>
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

  <h1 class="my-4 py-4">{{ currentTime }}</h1>
  <div aria-live="polite" aria-atomic="true" style="position: fixed; min-height: 100vh; width: 100%;">
    <div
      class="toast d-flex align-items-center"
      id="clipboard"
      role="alert"
      aria-live="assertive"
      aria-atomic="true"
      style="position: absolute; top: 0; right: 0;"
    >
      <div class="toast-body">
        Copied waypoint to clipboard!
      </div>
      <button type="button" class="btn-close ml-auto mr-2" data-dismiss="toast" aria-label="Close"></button>
    </div>
  </div>
  <div class="container my-4 py-4">
    <div class="list-group">
      <div
        class="list-group-item list-group-item-action"
        v-bind:class="{ active: boss.active }"
        aria-current="{{boss.active}}"
        v-for="boss in rotation"
        :key="boss.id"
      >
        <div class="d-flex w-100 justify-content-between" title="Click to copy waypoint" @click="clipboard(boss.waypoint)">
          <h5 class="mb-1">{{ boss.name }}</h5>
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
    setInterval(this.updateTime, 1000);
    this.updateRotation();

    for (let i = 0; i < this.rotation.length; i++) {
      this.rotation[i].nextSpawn = ((i * 20) % 60).toString();
    }
  },

  data() {
    return {
      currentTime: '',
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
        { id: 0, name: 'The Black Knight', waypoint: '/way 64,8, 22,1', active: false, nextSpawn: '' },
        { id: 1, name: 'Bronjahm', waypoint: '/way 70,7, 38,4', active: false, nextSpawn: '' },
        { id: 2, name: 'Scourgelord Tyrannus', waypoint: '/way 47,2, 66,1', active: false, nextSpawn: '' },
        { id: 3, name: 'Forgemaster Garfrost', waypoint: '/way 58,6, 72,5', active: false, nextSpawn: '' },
        { id: 4, name: 'Marwyn', waypoint: '/way 58,2, 83,4', active: false, nextSpawn: '' },
        { id: 5, name: 'Falric', waypoint: '/way 50,2, 87,9', active: false, nextSpawn: '' },
        { id: 6, name: "The Prophet Tharon'ja", waypoint: '/way 80,1, 61,2', active: false, nextSpawn: '' },
        { id: 7, name: 'Novos the Summoner', waypoint: '/way 77,8, 66,1', active: false, nextSpawn: '' },
        { id: 8, name: 'Trollgore', waypoint: '/way 58,3, 39,4', active: false, nextSpawn: '' },
        { id: 9, name: "Krik'thir the Gatewatcher", waypoint: '/way 67,5, 58,0', active: false, nextSpawn: '' },
        { id: 10, name: 'Prince Taldaram', waypoint: '/way 29,6, 62,2', active: false, nextSpawn: '' },
        { id: 11, name: 'Elder Nadox', waypoint: '/way 44,2, 49,1', active: false, nextSpawn: '' },
        { id: 12, name: 'Noth the Plaguebringer', waypoint: '/way 31,6, 70,5', active: false, nextSpawn: '' },
        { id: 13, name: 'Patchwerk', waypoint: '/way 36,5, 67,4', active: false, nextSpawn: '' },
        { id: 14, name: "Blood Queen Lana'thel", waypoint: '/way 49,7, 32,7', active: false, nextSpawn: '' },
        { id: 15, name: 'Professor Putricide', waypoint: '/way 57,1, 30,3', active: false, nextSpawn: '' },
        { id: 16, name: 'Lady Deathwhisper', waypoint: '/way 51,1, 78,5', active: false, nextSpawn: '' },
        { id: 17, name: 'Skadi the Ruthless', waypoint: '/way 57,8, 56,1', active: false, nextSpawn: '' },
        { id: 18, name: 'Ingvar the Plunderer', waypoint: '/way 52,4, 52,6', active: false, nextSpawn: '' },
      ],
    };
  },
  methods: {
    updateTime() {
      let date = new Date();
      let hour = date
        .getUTCHours()
        .toString()
        .padStart(2, '0');
      let minute = date
        .getUTCMinutes()
        .toString()
        .padStart(2, '0');
      let seconds = date
        .getUTCSeconds()
        .toString()
        .padStart(2, '0');

      if (seconds === '00') {
        this.updateRotation();
      }

      this.currentTime = `${hour}:${minute}:${seconds}`;
    },

    updateRotation() {
      let epoch = Date.UTC(2020, 10, 11, 9, 0, 0);
      let nowUtc = new Date().getTime();
      let offsetMilliseconds = nowUtc - epoch;
      let offsetSeconds = Math.floor(offsetMilliseconds / 1000);
      let offsetMinutes = Math.floor(offsetSeconds / 60);
      let currentRotation = Math.floor(offsetMinutes / 20) % this.rotation.length;

      for (let i = 0; i < this.rotation.length; i++) {
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
</style>
