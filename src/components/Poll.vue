<template>
  <div>
    <h2 class="text-xl">Poll</h2>
    <button class="bg-green-600 hover:bg-green-700 px-5 py-2 shadow rounded text-white" @click="poll">
      <span v-if="!waiting">Realizar Poll</span>
      <span v-if="waiting">Haciendo Poll...</span>
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
            poll() {
                if (this.waiting) return;

                this.waiting = true;
                this.response = null;
                POS.poll().then((response) => {
                    this.$emit('onPollResponse', response)
                    // this.response = response
                }).finally(() => {
                    this.waiting = false;
                })

            },
        },
    }
</script>
