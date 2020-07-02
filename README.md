# Proyecto de ejemplo usando el SDK Web para POS
El siguiente proyecto es un ejemplo de POS integrado utilizando el SDK WEB, que consta de un SDK de Javscript para la parte web y un programa cliente que corre en el computador que tiene conectado físicamente el POS

Este proyecto es una aplicación realizada en Vue.js que permite demostrar el funcionamiento del SDK Web. 
El código importante está en `src/views/Home.vue` donde se encontrarán varios componentes de Vue `src/components/` ya que cada funcionalidad como el área de nueva venta, carga de llaves y obtener última venta están separados en un componente de Vue para ordenar mejor el código y que sea más entendible. 

#Antes de empezar
Este ejemplo presenta un sistema de caja de demostración que usa el SDK para conectarse y realizar tareas con el POS. 
Para que el ejemplo funcione correctamente, es necesario tener andando [el cliente del SDK WEB](http://github.com/TransbankDevelopers/transbank-pos-sdk-web-client)

## 1. Ejecutar ejemplo

### A) Usando Docker
Para ejecutar el proyecto de esta forma, es necesario tener instalado: 
- [docker y docker-compose](https://docs.docker.com/install/)

Para iniciar el demo, solo hay que ejecutar: 
```bash
docker-compose up
```

Para detener:
```bash
docker-compose down
```

#### Abrir aplicación
La aplicación se ejecutará en [http://localhost:8000](http://localhost:8000) (y fallará en caso de que el puerto 8000 no esté disponible)


### B Usando tu sistema
Para ejecutar el proyecto usando tu sistema, es necesario tener instalado: 
- [Node.js](https://nodejs.org/en/)

Con el código fuente del proyecto en tu computador, puedes ejecutar en la raíz del proyecto los siguientes pasos:

#### 1.- Instalar dependencias
Para instalar las dependencia puedes ejecutar el siguiente comando en tu consola:
```bash
npm install
```
#### 2.- Ejecutar ejemplo
```bash 
npm run serve
```

#### 3.- Abrir aplicación
La aplicación se ejecutará en [http://localhost:3000](http://localhost:3000) (y fallará en caso de que el puerto 3000 no esté disponible)
