<template>
  <div>
    <h2 class="text-xl">Cerrar día</h2>
    <button class="bg-green-600 hover:bg-green-700 px-5 py-2 shadow rounded text-white" @click="closeDay">
      <span v-if="!waiting">Cerrar día</span>
      <span v-if="waiting">Cerrando día...</span>
    </button>
    <pre>{{ response }}</pre>
  </div>
</template>
<script>
    import POS from "transbank-pos-sdk-web"

    export default {
        data() {
            return {
                waiting: false,
                response: null,
            }
        },
        methods: {
            closeDay() {
                if (this.waiting) return;

                this.waiting = true;
                this.response = null;
                POS.closeDay().then((response) => {
                    this.$emit('onCloseDayResponse', response)
                    // this.response = response
                }).finally(() => {
                    this.waiting = false;
                })

            },
        },
    }
</script>
