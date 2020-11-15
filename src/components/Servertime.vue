<template>
  <h1 class="my-2 py-2 my-lg-4 py-lg-4">{{ region }} Server Time: {{ currentTime }}</h1>
</template>

<script>
export default {
  name: 'Servertime',
  props: ['pepega-time-format', 'european'],
  mounted() {
    setInterval(this.updateTime, 1000);
    this.updateTime();
  },
  data() {
    return {
      currentTime: '',
      timeOffset: this.european ? 1 : 16,
      region: this.european ? 'EU' : 'NA',
    };
  },
  watch: {
    pepegaTimeFormat: {
      handler: function () {
        this.updateTime();
      },
    },
    european: {
      handler: function () {
        this.timeOffset = this.european ? 1 : 16;
        this.region = this.european ? 'EU' : 'NA';
        this.updateTime();
      },
    },
  },
  methods: {
    updateTime() {
      let date = new Date();
      let hour = ((date.getUTCHours() + this.timeOffset) % 24).toString().padStart(2, '0');
      let minute = date.getUTCMinutes().toString().padStart(2, '0');
      let seconds = date.getUTCSeconds().toString().padStart(2, '0');

      let suffix = '';

      if (this.pepegaTimeFormat) {
        suffix = (date.getUTCHours() + this.timeOffset) % 24 < 12 ? 'AM' : 'PM';
        hour = ((date.getUTCHours() + this.timeOffset) % 12).toString().padStart(2, '0');
      }

      this.currentTime = `${hour}:${minute}:${seconds} ${suffix}`;
    },
  },
};
</script>
