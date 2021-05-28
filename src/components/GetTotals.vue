<template>
    <div>
        <h2 class="text-xl">Obtener totales</h2>
        <button class="bg-green-600 hover:bg-green-700 px-5 py-2 shadow rounded text-white" @click="getTotals">
            <span v-if="!waiting">Obtener</span>
            <span v-if="waiting">Obteniendo...</span>
        </button>
        <pre>{{ response }}</pre>
    </div>
</template>
<script>
import POS from 'transbank-pos-sdk-web';

export default {
    data () {
        return {
            waiting: false,
            response: null,
        };
    },
    methods: {
        getTotals () {
            if (this.waiting) return;

            this.waiting = true;
            this.response = null;
            POS.getTotals().then((response) => {
                this.$emit('onGetTotalsResponse', response)
                // this.response = response;
            }).finally(() => {
                this.waiting = false;
            });

        },
    },
};
</script>
