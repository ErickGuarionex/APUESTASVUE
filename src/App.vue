<script setup>
import { ref, onMounted } from 'vue';

const numero = ref(1);
const apuesta = ref(0);
const dinero = ref(1000);
const resultado = ref('');

const restar = () => {
  if (numero.value > 1) {
    numero.value--;
  }
};

const sumar = () => {
  if (numero.value < 10) {
    numero.value++;
  }
};

const obtenerDinero = () => {
  let dineroGuardado = localStorage.getItem('dinero');
  if (!dineroGuardado) {
    dineroGuardado = 1000; 
    localStorage.setItem('dinero', dineroGuardado);
  }
  return parseInt(dineroGuardado);
};

const guardarDinero = (nuevoDinero) => {
  localStorage.setItem('dinero', nuevoDinero);
  actualizarDineroEnPantalla();
};

const actualizarDineroEnPantalla = () => {
  dinero.value = obtenerDinero();
};

const realizarApuesta = () => {
  const respuestaRandom = Math.ceil(Math.random() * 10);
  let dineroActual = obtenerDinero();
  const apuestaValor = parseInt(apuesta.value);

  resultado.value = `EL NUMERO GANADOR FUE: ${respuestaRandom}`;

  if (apuestaValor <= 0 || apuestaValor > dineroActual) {
    resultado.value = "¡Apuesta inválida!";
    return;
  }

  if (respuestaRandom === numero.value) {
    dineroActual += apuestaValor;
  } else {
    dineroActual -= apuestaValor;
  }
  guardarDinero(dineroActual);
};

onMounted(() => {
  actualizarDineroEnPantalla();
});

</script>
<template>
  <div class="saldo">
    <div id="resultado">{{ resultado }}</div>
    <img class="billetera" src="./billetera.png" alt="">
    <h1 class="titulo">Saldo: <span>{{ dinero }}</span>$</h1>
  </div>

  <!--ICONOS PRINCIPALES-->
  <div class="usuario-apuesta">
    <h1>Cantidad a Apostar</h1>
    <input id="input" type="number" v-model="apuesta">
    <h1 class="t-principal">Apuesta el número a salir y gana!</h1>
    <div class="num-apostado">
      <button @click="restar"><</button>
      <h2>{{ numero }}</h2>
      <button @click="sumar">></button>
    </div>
    <button class="b-apostar" @click="realizarApuesta"><p>Apostar</p></button>
  </div>
</template>

