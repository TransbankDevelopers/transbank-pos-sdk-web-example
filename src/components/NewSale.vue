<template>
  <div>
    <h2 class="text-2xl">Realizar una venta</h2>

    <div class="products flex flex-wrap -ml-2">
      <div @click="addProduct(product)"
           class="hover:border-blue-500 cursor-pointer product w-64 rounded-lg shadow-lg border m-2 p-2  flex flex-col items-center text-center"
           v-for="product in products">
        <img class="w-full" :src="product.image" :alt="product.name">
        {{ product.name }}
      </div>
    </div>

    <div class="flex mt-4 border-t-2 pt-4 items-center">
      <span>
        Total venta:
        <span class="text-2xl">${{ total }}</span>
      </span>
      <div v-if="total > 0" class="ml-2">
        | <a href="" class="hover:underline text-red-500" @click.prevent="clearTotal">Borrar venta</a>
      </div>

      <span class="ml-auto" v-if="total > 0">
        <button class="bg-green-600 hover:bg-green-700 px-5 py-2 shadow rounded text-white" @click="doSale">Realizar venta</button>
      </span>

    </div>

    <!-- Detalle de la venta -->
    <div v-if="saleResponse !== null">
      <h2 class="text-2xl">Respuesta: </h2>
      <pre>{{ saleResponse }}</pre>
    </div>

  </div>
</template>
<script>
    import POS from "transbank-pos-sdk-web"
    import swal from "sweetalert"

    export default {
        methods: {
            doSale() {
                // Hacer la venta y cuando llegue la respuesta, vaciar la venta y guardar los detalles de la transacción en
                // la variable saleResponse para poder mostrarlos
                swal("Solicite al cliente que opere el POS", {
                    buttons: false,
                })

                this.saleResponse = null
                POS.doSale(this.total, "ticket1", (data) => {
                    console.log('Mensaje intermedio', data)
                }).then((saleResponse) => {
                    console.log(saleResponse)
                    //Acá llega la respuesta de la venta. Si saleResponse.responseCode es 0, entonces la comproa fue aprobada
                    if (saleResponse.responseCode === 0) {
                        // Mostramos mensaje de éxito y limpiamos el total de la venta si response code es 0
                        swal("Transacción aprobada", "", "success")
                        this.clearTotal()
                    } else {
                        // Mostramos mensaje de error si response code es distinto de 0
                        swal("Transacción fallida", "La transacción no ha sido aprobada. Puede reintentar pago", "error")
                    }

                    this.saleResponse = saleResponse

                })
            },
            clearTotal() {
                this.total = 0
            },
            addProduct(product) {
                this.total += product.price
            },
        },
        data() {
            return {
                total: 0,
                saleResponse: null,
                products: [
                    {
                        "name": "Hamburguesa + papas fritas + bebida",
                        "image": "/img/combo1.png",
                        "price": 5500,
                    },
                    {
                        "name": "Hamburguesa sola",
                        "image": "/img/sandwich1.png",
                        "price": 3500,
                    }, {
                        "name": "Taco",
                        "image": "/img/taco.png",
                        "price": 2000,
                    },
                    {
                        "name": "Café",
                        "image": "/img/coffee1.png",
                        "price": 100,
                    },
                ],
            }
        },
    }
</script>
