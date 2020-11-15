<template>
  <h1 class="my-2 py-2 my-lg-4 py-lg-4">Server Time: {{ currentTime }}</h1>
</template>

<script>
export default {
  name: 'Servertime',
  props: ['pepega-time-format'],
  mounted() {
    setInterval(this.updateTime, 1000);
    this.updateTime();
  },
  data() {
    return {
      currentTime: '',
    };
  },
  watch: {
      pepegaTimeFormat: {
      handler: function () {
        this.updateTime();
      },
    },
  },
  methods: {
    updateTime() {
      let date = new Date();
      let hour = (date.getUTCHours() + (1 % 24)).toString().padStart(2, '0');
      let minute = date.getUTCMinutes().toString().padStart(2, '0');
      let seconds = date.getUTCSeconds().toString().padStart(2, '0');

      let suffix = '';

      if (this.pepegaTimeFormat) {
        suffix = date.getUTCHours() + (1 % 24) < 12 ? 'AM' : 'PM';
        hour = ((date.getUTCHours() + 1) % 12).toString().padStart(2, '0');
      }

      this.currentTime = `${hour}:${minute}:${seconds} ${suffix}`;
    },
  },
};
</script>
