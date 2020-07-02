<template>
  <div class="container mx-auto p-2 py-10">
    <div class="flex items-baseline">
      <h1 class="text-2xl font-thin">Punto de venta demo - SDK Web POS</h1>
      <h3 class="ml-auto font-thin" v-if="activePort">
        Conectado a POS en puerto
        <span class="text-gray-500 mr-2">{{ activePort }}</span> |

      </h3>
      <span class="font-normal hover:underline cursor-pointer text-red-500 ml-2"
            @click="closeConenction">Cerrar conexión</span>
    </div>
    <div class=" shadow rounded bg-white p-2">


      <div class="flex my-64 items-center flex-col justify-center" v-if="!connected">
        <!-- Puedes revisar este componente en src/components/ConnectToPOS.vue 😌 -->
        <connect-to-pos :connected="connected" @connected="onConnect" v-if="connected === false"></connect-to-pos>
      </div>


      <div v-if="connected" class="px-10 pb-20">
        <!-- Puedes revisar este componente en src/components/NewSale.vue 😌 -->
        <new-sale class="pb-20"></new-sale>

        <div class="flex">
          <!-- Puedes revisar este componente en src/components/LastSale.vue 😌 -->
          <last-sale class="md:w-1/3"></last-sale>
          <load-keys class="md:w-1/3"></load-keys>
        </div>

      </div>


    </div>
  </div>

</template>

<script>
    // @ is an alias to /src
    import POS from "transbank-pos-sdk-web"
    import swal from "sweetalert"
    import ConnectToPos from "../components/ConnectToPOS"
    import NewSale from "../components/NewSale"
    import LastSale from "../components/LastSale"
    import LoadKeys from "../components/LoadKeys"

    export default {
        components: {
            ConnectToPos, NewSale, LastSale, LoadKeys,
        },
        mounted() {
            POS.connect()
        },
        data() {
            return {
                connected: false,
                activePort: null,
            }
        },
        methods: {
            async closeConenction() {
                let response = await POS.closePort()
                console.log("close port: ", response)
                this.activePort = null
                this.connected = false
            },
            onConnect(port) {
                console.log("PORT", port)
                this.activePort = port
                this.connected = true
            },
        },
    }
</script>
