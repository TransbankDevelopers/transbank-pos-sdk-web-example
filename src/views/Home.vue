<template>
  <div class="container mx-auto p-2 py-10">
    <div class="flex items-baseline">
      <h1 class="text-2xl font-thin">Punto de venta demo - SDK Web POS</h1>
      <h3 class="ml-auto font-thin" v-if="activePort">
        Conectado a POS en puerto
        <span class="text-gray-500 mr-2">{{ activePort }}</span> |

      </h3>
      <span class="font-normal hover:underline cursor-pointer text-red-500 ml-2 mr-2 ml-auto"
            @click="closeConenction" v-if="connected">Cerrar conexión</span>

      <span class="font-normal ml-auto"
            @click="closeConenction">
        Conexión con socket: <span class="font-bold">{{ socketConnected ? "Conectado" : "No conectado" }}</span>
      </span>
    </div>
    <div class=" shadow rounded bg-white p-2 mt-2">


      <div class="flex my-64 items-center flex-col justify-center" v-if="!connected">
        <!-- Puedes revisar este componente en src/components/ConnectToPOS.vue 😌 -->
        <connect-to-pos :connected="connected" @connected="onConnect" v-if="connected === false"></connect-to-pos>
      </div>

      <div v-if="connected" class="px-10 pb-20">
        <!-- Puedes revisar este componente en src/components/NewSale.vue 😌 -->
        <new-sale class="pb-20"></new-sale>

        <div class="flex flex-wrap border-t">
          <!-- Puedes revisar este componente en src/components/LastSale.vue 😌 -->
          <div class="box">
            <last-sale></last-sale>
          </div>
          <div class="box">
            <load-keys></load-keys>
          </div>
          <div class="box">
            <set-normal-mode></set-normal-mode>
          </div>
          <div class="box">
            <close-day></close-day>
          </div>
          <div class="box">
            <get-sales-of-the-day></get-sales-of-the-day>
          </div>
          <div class="box">
            <refund></refund>
          </div>
          <div class="box">
            <get-totals></get-totals>
          </div>

        </div>

      </div>

    </div>
  </div>

</template>
<style type="text/css">
  .box {
    @apply w-full mt-5;
  }
  .box > div {
    @apply shadow-lg p-5 rounded;
  }
  @screen md {
    .box {
      @apply w-1/3 p-5;
    }
  }
</style>

<script>
    // @ is an alias to /src
    import POS from "transbank-pos-sdk-web"
    import swal from "sweetalert"
    import ConnectToPos from "../components/ConnectToPOS"
    import NewSale from "../components/NewSale"
    import LastSale from "../components/LastSale"
    import LoadKeys from "../components/LoadKeys"
    import SetNormalMode from "../components/SetNormalMode"
    import CloseDay from "../components/CloseDay"
    import GetSalesOfTheDay from "../components/GetSalesOfTheDay"
    import Refund from "../components/Refund"
    import GetTotals from "../components/GetTotals"

    export default {
        components: {
            ConnectToPos, NewSale, LastSale, LoadKeys, SetNormalMode, CloseDay, GetSalesOfTheDay, Refund, GetTotals
        },
        async mounted() {
            try {
                await POS.connect()
                this.socketConnected = true
                let response = await POS.getPortStatus()
                this.connected = response.connected;
                this.activePort = response.activePort;
            } catch (e) {
              console.log(e);
                swal("No se pudo conectar con el software cliente", "Verifique que el cliente se haya inicializado en este computador", "error")
            }
        },
        data() {
            return {
                socketConnected: false,
                connected: false,
                activePort: null,
            }
        },
        methods: {
            async closeConenction() {
                await POS.closePort()
                this.activePort = null
                this.connected = false
            },
            onConnect(port) {
                this.activePort = port
                this.connected = true
            },
        },
    }
</script>
