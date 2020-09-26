<template>
  <div>
    <h2 class="text-xl">Obtener ventas del día</h2>
    <label for="printOnPos">
      <input type="checkbox" v-model="printOnPos" id="printOnPos"> Imprimir en el POS (no devuelve la información)
    </label>
      <hr class="m-2">
    <button class="bg-green-600 hover:bg-green-700 px-5 py-2 shadow rounded text-white" @click="getDetails">
      <span v-if="!waiting">Obtener ventas del día</span>
      <span v-if="waiting">Obteniendo...</span>
    </button>
    <pre>{{ response }}</pre>
  </div>
</template>
<script>
    import POS from "transbank-pos-sdk-web"

    export default {
        data() {
            return {
                printOnPos: false,
                waiting: false,
                response: null,
            }
        },
        methods: {
            getDetails() {
                if (this.waiting) return;

                this.waiting = true;
                this.response = null;
                POS.getDetails(this.printOnPos).then((response) => {
                    this.response = response
                }).finally(() => {
                    this.waiting = false;
                })

            },
        },
    }
</script>
