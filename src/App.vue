<script setup>
import { ref, reactive, computed } from 'vue'
import Alerta from './components/Alerta.vue'
import Spinner from './components/Spinner.vue'
import useCripto from './composables/useCripto'

const { monedas, criptomonedas, cargando, cotizacion, obtenerCotizacion, mostrarResultado } = useCripto()

const error = ref('')
const cotizar = reactive({
  moneda: '',
  criptomoneda: ''
})

const cotizarCripto = () => {
  //Validar que cotizar este lleno
  if (Object.values(cotizar).includes('')) {
    error.value = 'Todos los campos son obligatorios'
    return
  }
  error.value = ''
  obtenerCotizacion(cotizar)
}   

</script>

<template>
  <div class="contenedor">
    <h1 class="titulo">Cotizador de <span>Criptomonedas</span></h1>

    <div class="contenido">
      <Alerta v-if="error">
        {{ error }}
      </Alerta>
      <form class="formulario" @submit.prevent="cotizarCripto">

        <div class="campo">
          <label for="moneda">Moneda: </label>
          <select name="" id="moneda" v-model="cotizar.moneda">
            <option value="">-- Selecciona --</option>
            <option v-for="moneda in monedas" :value="moneda.codigo">{{ moneda.texto }}</option>
          </select>
        </div>

        <div class="campo">
          <label for="cripto">Criptomoneda: </label>
          <select name="" id="cripto" v-model="cotizar.criptomoneda">
            <option value="">-- Selecciona --</option>
            <option v-for="criptomoneda in criptomonedas" :value="criptomoneda.CoinInfo.Name">{{
              criptomoneda.CoinInfo.FullName }}</option>
          </select>
        </div>
        <input type="submit" value="Cotizar">
      </form>
      <Spinner v-if="cargando" />

      <div class="contenedor-resultado" v-if="mostrarResultado">
        <h2>Cotizacion</h2>
        <div class="resultado">
          <img :src="'https://cryptocompare.com/' + cotizacion.IMAGEURL" alt="imagen cripto">
          <div>
            <p>El precio es de: <span>{{ cotizacion.PRICE }}</span></p>
            <p>El precio mas alto del dia: <span>{{ cotizacion.HIGHDAY }}</span></p>
            <p>El precio mas bajo del dia: <span>{{ cotizacion.LOWDAY }}</span></p>
            <p>Variación ultimas 24 horas: <span>{{ cotizacion.CHANGEPCT24HOUR }}%</span></p>
            <p>Ultima actualización: <span>{{ cotizacion.LASTUPDATE }}</span></p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
