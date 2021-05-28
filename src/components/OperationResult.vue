<template>
  <div v-if="resultData !== null">
    <h2 class="text-2xl">Respuesta: </h2>
    <vue-json-pretty :data="resultData" :showDoubleQuotes="false" :showLength="true"> </vue-json-pretty>
  </div>
</template>

<script>
import VueJsonPretty from 'vue-json-pretty';
import 'vue-json-pretty/lib/styles.css';

export default {
  components: {
    VueJsonPretty,
  },
  props: ["resultData"],
};
</script>