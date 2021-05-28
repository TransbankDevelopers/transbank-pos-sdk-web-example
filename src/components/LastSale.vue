<template>
  <div>
    <h2 class="text-xl">Recuperar última venta</h2>
    <button class="bg-green-600 hover:bg-green-700 px-5 py-2 shadow rounded text-white" @click="getLastSale">
      <span v-if="!waiting">Recuperar última venta</span>
      <span v-if="waiting">Obteniendo última venta...</span>
    </button>
    <pre>{{ lastSale }}</pre>
  </div>
</template>
<script>
    import POS from "transbank-pos-sdk-web"

    export default {
        data() {
            return {
                waiting: false,
                lastSale: null,
            }
        },
        methods: {
            getLastSale() {
                if (this.waiting) return;

                this.waiting = true;
                this.lastSale = null;
                POS.getLastSale().then((response) => {
                    this.$emit('onLastSaleResponse', response)
                    // this.lastSale = response
                }).finally(() => {
                    this.waiting = false;
                })

            },
        },
    }
</script>
