<template class="pepega">
  <Navbar
    v-on:toggle-darkmode="darkMode = !darkMode"
    v-on:toggle-pepega-time-format="pepegaTimeFormat = !pepegaTimeFormat"
    v-on:toggle-sorted-list="sortedList = !sortedList"
    v-on:toggle-european="european = !european"
  />

  <Servertime :pepega-time-format="pepegaTimeFormat" :european="european" />

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

  <Bosslist :pepega-time-format="pepegaTimeFormat" :sorted-list="sortedList" v-on:clipboard="clipboard" :european="european"/>
</template>

<script>
import Navbar from './components/Navbar';
import Servertime from './components/Servertime';
import Bosslist from './components/Bosslist';

export default {
  name: 'App',
  components: { Navbar, Servertime, Bosslist },
  mounted() {
    if (this.darkMode) {
      document.body.classList.toggle('darkmode');
    }
  },
  data() {
    return {
      darkMode: this.getFromLocalStorageOrFallback('darkMode', false),
      pepegaTimeFormat: this.getFromLocalStorageOrFallback('pepegaTimeFormat', false),
      sortedList: this.getFromLocalStorageOrFallback('sortedList', false),
      european: this.getFromLocalStorageOrFallback('european', true) 
    };
  },
  watch: {
    pepegaTimeFormat: {
      handler: function () {
        this.serializeSettings();
      },
    },
    darkMode: {
      handler: function () {
        this.serializeSettings();
        document.body.classList.toggle('darkmode');
      },
    },
    sortedList: {
      handler: function () {
        this.serializeSettings();
      },
    },
    european: {
      handler: function () {
        this.serializeSettings();
      },
    },
  },
  methods: {
    getFromLocalStorageOrFallback(key, fallback) {
      let fromLocalStorage = localStorage.getItem(key);
      if (fromLocalStorage) {
        return JSON.parse(fromLocalStorage);
      }
      return fallback;
    },

    serializeSettings() {
      localStorage.setItem('darkMode', JSON.stringify(this.darkMode));
      localStorage.setItem('pepegaTimeFormat', JSON.stringify(this.pepegaTimeFormat));
      localStorage.setItem('sortedList', JSON.stringify(this.sortedList));
      localStorage.setItem('european', JSON.stringify(this.european));
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
  .navbar-toggler {
    background-color: #444;
  }

  .list-group-item {
    background-color: #444;
    color: #fff;

    &.active {
      background-color: #1d1b1b;
      border-color: #fff;
    }
  }

  .btn-outline-primary {
    color: #fff;
    border-color: #fff;

    &:hover {
      background-color: #000;
    }
  }
}
</style>
