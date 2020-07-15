<template>
  <div>
    <h2 class="text-xl">Salir de modo integrado</h2>
    <button class="bg-green-600 hover:bg-green-700 px-5 py-2 shadow rounded text-white" @click="setNormalMode">
      <span v-if="!waiting">Salir de modo integrado</span>
      <span v-if="waiting">Saliendo...</span>
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
            setNormalMode() {
                if (this.waiting) return;

                this.waiting = true;
                this.response = null;
                POS.setNormalMode().then((response) => {
                    this.response = response
                }).finally(() => {
                    this.waiting = false;
                })

            },
        },
    }
</script>
