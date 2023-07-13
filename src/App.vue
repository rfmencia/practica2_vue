<template>
  <nav
    v-bind:style="'background-color:' + configuracionPagina.menuColor"
    class="navbar navbar-expand-lg"
  >
    <div class="container-fluid">
      <a class="navbar-brand" href="#">{{ configuracionPagina.marca }}</a>
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li v-for="(valor,index) in configuracionPagina.menus" :key="index" class="nav-item">
            <a class="nav-link" aria-current="page" href="#">{{
              valor.etiqueta
            }}</a>
          </li>
        </ul>
      </div>
    </div>
  </nav>
  <div class="container">
    <div class="row">
      <h3>{{ producto.nombre }}</h3>
    </div>
    <div class="row">
      <div class="col-12 col-sm-6 col-md-4">
        <img v-bind:src="producto.imagen" alt="" width="100%" />
      </div>
      <div class="col-12 col-sm-6 col-md-8">
        <h6>{{ producto.descripcion }}</h6>
        <div class="p-3 mb-2 text-white" style="background-color: gray">
          Precio: {{ producto.precio }}
        </div>
        <h5>Color</h5>
        <div>
          <div
            v-for="(color,index) in producto.colores"
            :key="index"
            class="color-box clic"
            v-bind:style="'background:' + color"
            v-on:click="pedido.color = color"
          ></div>
        </div>
        <h5>Cantidad</h5>
        <div class="quantity">
          <button v-on:click="pedido.cantidad -= 1">-</button>
          <div>{{ pedido.cantidad }}</div>
          <button v-on:click="pedido.cantidad += 1">+</button>
        </div>
        <div class="buy-box">
          <button
            type="button"
            class="btn btn-primary"
            :disabled="pedido.cantidad <= 0"
            @click="mostrarAlerta()"
          >
            Comprar
          </button>
        </div>
      </div>
    </div>
  </div>

  <div class="container">
    <div class="row">
      <h4>Productos relacionados</h4>
    </div>
    <div class="row">
      <div v-for="(prod,index) in productosRelacionados" :key="index" class="col">
        <div class="card" style="width: 18rem">
          <div class="card-body" @click="seleccionarProducto(prod)">
            <h5 class="card-title">{{ prod.nombre }}</h5>
            <img v-bind:src="prod.imagen" alt="" width="100%" />
            <p class="card-text">{{ prod.descripcion }}</p>
            <div
              class="producto-relacionado-precio"
              :style="configuracionPagina.precioEstilos"
            >
              Precio: {{ prod.precio }} BOB
            </div>
            <div>
              <div>
                <div
                  v-for="(color,index) in prod.colores"
                  :key="index"
                  class="color-box"
                  v-bind:style="'background:' + color"
                ></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <footer
    v-bind:style="'background-color:' + configuracionPagina.footerColor"
  ></footer>
</template>

<script>
import { createApp } from 'vue';
import axios from 'axios';
import configuracionPagina from './configuracionPagina.js';
export default {
  name: 'App',
  data() {
    return {
      configuracionPagina:configuracionPagina,
      producto: {
        id: 1,
        imagen:
          "https://cdn.shopify.com/s/files/1/0640/5067/files/pexels-photo-53903_large.jpg?v=1492718386",
        nombre: "Dron LU3 MAX GPS 8K HD",
        descripcion:
          "Dron LU3 MAX GPS 8K HD profesional con <b>cámara Dual</b>,cardán autoestabilizador, Motor sin escobillas para evitar obstáculos, cuadricópteroplegable",
        precio: "620",
        colores: ["red", "blue", "black", "yellow"],
      },
      productosRelacionados: [],
      pedido: {
        id: null,
        cantidad: 0,
        color: null,
      },
    };
  },
  created() {
    axios.get('http://localhost:3000/Productos')
      .then(response => {
        this.productosRelacionados = response.data;
      })
      .catch(error => {
        console.log(error);
      });
  },
  methods: {
    mostrarAlerta() {
      alert(
        "Producto: " +
          this.producto.id +
          " Cantidad: " +
          this.pedido.cantidad +
          " Color: " +
          this.pedido.color
      );
    },
    seleccionarProducto(producto) {
      this.producto = producto;
    },
  },
};
createApp().mount('#app');
</script>
