<template>
  <div class="flex w-full flex-col items-center justify-center">
    <div class="w-2/5 mx-auto flex flex-col">
      <span class="text-2xl font-thin mb-4 text-red-500">POS: Desconectado</span>
      <div class="flex">
        <button @click="listPorts()" class="flex-1 m-2 bg-green-500 p-4 rounded shadow outline-none hover:opacity-75 text-white">
          Listar puertos
        </button>
        <button @click="autoconnect()" class="flex-1 m-2 bg-blue-500 p-4 rounded shadow outline-none hover:opacity-75 text-white">
          Descubrir y conectar
        </button>
      </div>
    </div>

    <div v-if="!connected && ports.length > 0" class="mt-10">
      <hr>
      <h3 class="mx-2 font-bold text-lg">Puertos disponibles: </h3>
      <p class="mx-2 mb-2">Encuentra el puerto en el que está conectado el POS y haz click sobre el para conectar</p>
      <button v-if="!connected" @click="setActivePort(port.path)" v-for="port in ports"
              class="bg-blue-500 m-2 p-4 rounded shadow outline-none hover:opacity-75 text-white">
        {{ port.path }}
      </button>
    </div>
  </div>
</template>

<script>
    // @ is an alias to /src
    import POS from "transbank-pos-sdk-web"
    import swal from "sweetalert"

    export default {
        props: ["connected"],
        mounted() {

        },
        data() {
            return {
                ports: [],
                activePort: null,
            }
        },
        methods: {
            autoconnect() {
                POS.autoconnect().then((port) => {
                    if (port === false) {
                        swal('No se encontró ningún POS', '', 'error');
                        return;
                    }
                    this.$emit("connected", port.path)
                    swal('Conectado', 'Dispositivo detectado y conectado: ' + port.path, 'success');
                });
            },
            listPorts() {
                POS.getPorts().then((ports) => {
                  console.log('ports', ports)
                    this.ports = ports
                }).catch(() => {
                    swal("No se pudo obtener puertos.", "¿Está corriendo el agente Transbank POS?", "error")
                })
            },

            async setActivePort(port) {
                this.activePort = port
                if (this.connected) {
                    POS.closePort()
                }
                POS.openPort(port).then((result) => {
                    if (result===true) {
                        swal("Conectado satisfactoriamente", "", "success")

                        this.$emit("connected", port)
                    } else {
                        swal("No conectado", "", "error")
                    }
                }).catch(error => console.log(error))
            },
        },
    }
</script>
