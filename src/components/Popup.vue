<template>
  
</template>

<script>

import { mapMutations } from 'vuex';
import L from 'leaflet';

const events = [
  'add',
  'remove',
  'popupopen',
  'popupclose',
  'tooltipopen',
  'tooltipclose'
];

const props = {
  content: {
    type: String,
    custom: true,
    default: '',
  },
  latlng: {
    type: Array,
  },
};

export default {
  props,
  methods: {
    ...mapMutations([
      'openPopup',
    ]),
  },
  mounted() {
    if (this.latlng) {
      var popup = L.popup()
        .setLatLng(this.latlng)
        .setContent(this.content);

      this.$nextTick(function () {
        this.openPopup(popup)
        events.forEach((event) => {
          popup.on({ event, func: (ev) => { this.$emit(event, ev) } })
        })
      });
    } else {
      this.$nextTick(function () {
        this.$parent.$marker.bindPopup(this.content).openPopup();
        //TODO: popup
      })
    }
  }
}
</script>
