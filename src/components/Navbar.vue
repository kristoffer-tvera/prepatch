<template>
  <nav class="navbar navbar-light bg-light navbar-expand-lg">
    <div class="container-fluid">
      <a class="navbar-brand" href="https://amusedtodeath.eu"> Amused to Death </a>
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
            <button class="btn btn-outline-secondary mr-2 my-2 my-lg-0" type="button" @click="$emit('toggle-darkmode')">Toggle darkmode</button>
          </li>
          <li class="nav-item">
            <button class="btn btn-outline-secondary mr-2 my-2 my-lg-0" type="button" @click="$emit('toggle-pepega-time-format')">24H/12H</button>
          </li>
          <li class="nav-item">
            <button class="btn btn-outline-secondary mr-2 my-2 my-lg-0" type="button" @click="$emit('toggle-sorted-list')">Toggle sort</button>
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
</template>

<script>
export default {
  name: 'Navbar',
  props: [],
  data() {
    return {
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
    };
  },
  emits: ['toggle-darkmode', 'toggle-pepega-time-format', 'toggle-sorted-list'],
};
</script>
