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
        Conexión con agente: <span class="font-bold">{{ socketConnected ? "Conectado" : "No conectado" }}</span>
      </span>
    </div>
    <div class=" shadow rounded bg-white p-2 mt-2">


      <div class="flex my-64 items-center flex-col justify-center" v-if="!connected">
        <!-- Puedes revisar este componente en src/components/ConnectToPOS.vue 😌 -->
        <connect-to-pos :connected="connected" @connected="onConnect" v-if="connected === false"></connect-to-pos>
      </div>

      <div v-if="connected" class="px-10 pb-20">
        <!-- Puedes revisar este componente en src/components/NewSale.vue 😌 -->
        <new-sale class="pb-20" @onSaleResponse="setResultData"></new-sale>

        <div class="border-t">
          <operation-result :result-data="resultData"></operation-result>
        </div>
        <div class="flex flex-wrap border-t">
          <!-- Puedes revisar este componente en src/components/LastSale.vue 😌 -->
          <div class="box">
            <poll @onPollResponse="setResultData"></poll>
          </div>
          <div class="box">
            <last-sale @onLastSaleResponse="setResultData"></last-sale>
          </div>
          <div class="box">
            <load-keys @onLoadKeyResponse="setResultData"></load-keys>
          </div>
          <div class="box">
            <set-normal-mode @onNormalModeResponse="setResultData"></set-normal-mode>
          </div>
          <div class="box">
            <close-day @onCloseDayResponse="setResultData"></close-day>
          </div>
          <div class="box">
            <get-sales-of-the-day @onGetSales="setResultData"></get-sales-of-the-day>
          </div>
          <div class="box">
            <refund @onRefundResponse="setResultData"></refund>
          </div>
          <div class="box">
            <get-totals @onGetTotalsResponse="setResultData"></get-totals>
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
    import Poll from "../components/Poll"
    import OperationResult from "../components/OperationResult"

    export default {
        components: {
            ConnectToPos, NewSale, LastSale, LoadKeys, SetNormalMode, CloseDay, GetSalesOfTheDay, Refund, GetTotals, Poll, OperationResult
        },
        async mounted() {
            try {
                // POS.socket().on('disconnect', () => {
                //     swal("Se perdió la conexión con el Agente", "Verifique que el agente se haya inicializado en este computador", "error");
                // })

                POS.on('socket_connected', () => {
                  this.socketConnected = true;
                })

                POS.on('socket_disconnected', () => {
                  this.socketConnected = false;
                })

                POS.on('port_opened', (port) => {
                  this.activePort = port;
                  this.connected = true;
                })

                POS.on('port_closed', () => {
                  this.activePort = null;
                  this.connected = false;
                })

                await POS.connect()

                let response = await POS.getPortStatus()
                this.connected = response.connected;
                this.activePort = response.activePort;

                // POSsocket().on('connect', () => {
                //     swal("Se recuperó la conexión con el Agente", "", "success");
                // })

            } catch (e) {
                console.error('Conexión con el agente fallida: ', e);
                swal("No se pudo conectar con el agente Transbank POS", "Verifique que el agente se haya inicializado en este computador", "error")
            }
        },
        data() {
            return {
                socketConnected: false,
                connected: false,
                activePort: null,
                resultData: null,
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
            setResultData(data) {
              this.resultData = data
            },
        },
    }
</script>
