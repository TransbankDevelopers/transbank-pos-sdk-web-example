<template>
  <div>
    <h2 class="text-xl">Anulaciones</h2>
    <div class="flex items-center">
      <label for="operationId">
        <input type="number" class="border shadow rounded border-gray-600 p-1 px-2" v-model="operationId" placeholder="ID de operación" id="operationId">
      </label>
      <button class="ml-2 bg-green-600 hover:bg-green-700 px-5 py-2 shadow rounded text-white" @click="getDetails">
        <span v-if="!waiting">Anular</span>
        <span v-if="waiting">Anulando...</span>
      </button>
    </div>
    <pre>{{ response }}</pre>
  </div>
</template>
<script>
    import POS from "transbank-pos-sdk-web"

    export default {
        data() {
            return {
                operationId: null,
                waiting: false,
                response: null,
            }
        },
        methods: {
            getDetails() {
                if (this.waiting) return;

                this.waiting = true;
                this.response = null;
                POS.refund(this.operationId).then((response) => {
                    this.response = response
                }).finally(() => {
                    this.waiting = false;
                })

            },
        },
    }
</script>
