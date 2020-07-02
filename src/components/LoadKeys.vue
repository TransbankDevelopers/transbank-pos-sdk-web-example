<template>
  <div>
    <h2 class="text-xl">Carga de llaves</h2>
    <button class="bg-green-600 hover:bg-green-700 px-5 py-2 shadow rounded text-white" @click="getLastSale">
      <span v-if="waiting">Cargando llaves...</span>
      <span v-if="!waiting">Carga de llaves</span>
    </button>
    <pre>{{ result }}</pre>
  </div>
</template>
<script>
    import POS from "transbank-pos-sdk-web"

    export default {
        data() {
            return {
                waiting: false,
                result: null,
            }
        },
        methods: {
            getLastSale() {
                if (this.waiting) return
                this.waiting = true
                this.result = null
                POS.getKeys().then((response) => {
                    this.result = response
                }).finally(() => {
                    this.waiting = false
                })

            },
        },
    }
</script>
